+++
title = 'Invisible Image Watermarking'
date = 2026-10-30
draft = false
description = 'An experimental invisible image watermarking system implemented in Python and Rust.'
tags = ['Rust', 'Python', 'Image Processing', 'DCT', 'Error Correction']
showToc = true
+++

An experimental system for embedding and recovering invisible digital watermarks in images, implemented independently in **Python and Rust**.

The project explores how short messages can be embedded into images with minimal visible distortion and subsequently recovered without access to the original image.

Two implementations are available:

- **[watermark-rs](https://github.com/ekomissarov/watermark-rs)** — Rust implementation with CLI and desktop GUI.
- **[persistent_wtrmrk](https://github.com/ekomissarov/persistent_wtrmrk)** — Python implementation with CLI.

Both implementations use a compatible watermark format.

## Motivation

Invisible image watermarking presents an interesting engineering problem: embedding information into an image while preserving its visual appearance and maintaining the ability to recover that information after image transformations.

Three objectives must be balanced:

1. **Imperceptibility** — modifications should be difficult to distinguish visually.
2. **Robustness** — embedded information should survive common transformations such as compression, resizing, and limited geometric distortion.
3. **Capacity** — the system should accommodate a useful amount of information without significantly compromising the first two objectives.

These objectives are inherently competing. Increasing embedding strength may improve recovery but introduce visible artifacts. Increasing redundancy may improve robustness but reduce payload capacity.

The project investigates these trade-offs through frequency-domain embedding, error correction, and synchronization techniques.

## How It Works

The watermarking pipeline consists of several stages:

```text
                 EMBEDDING

Message
   |
   v
Payload Encoding
   |
   v
Error Correction
   |
   v
Spatial Distribution ------+
                           |
Original Image             |
   |                       |
   v                       |
DCT Transformation         |
   |                       |
   +-----------------------+
   |
   v
Coefficient Modification
   |
   v
Inverse DCT
   |
   v
Watermarked Image


                 EXTRACTION

Watermarked Image
   |
   v
Synchronization
   |
   v
DCT Analysis
   |
   v
Bit Recovery
   |
   v
Error Correction
   |
   v
Payload Validation
   |
   v
Recovered Message
```

### Frequency-Domain Embedding

The image is divided into blocks of 16 × 16 pixels.

Each block is transformed using the **Discrete Cosine Transform (DCT)**, which represents spatial image information as a combination of frequency components.

Instead of directly modifying pixel values, the algorithm embeds information by adjusting relationships between selected mid-frequency DCT coefficients.

Conceptually, a bit can be represented by the relative magnitude of two coefficients:

$$
b =
\begin{cases}
1, & c_1 > c_2 \\
0, & c_1 \leq c_2
\end{cases}
$$

where $c_1$ and $c_2$ are selected DCT coefficients.

The actual embedding procedure adjusts coefficient relationships with sufficient separation to improve recovery reliability.

Mid-frequency coefficients offer a practical compromise: low-frequency modifications may become visually noticeable, while high-frequency modifications are more likely to disappear during lossy compression.

### Content-Adaptive Embedding

Not all image regions are equally suitable for watermarking.

Uniform areas, smooth gradients, and text-like structures can be sensitive to modifications. More textured regions may tolerate stronger embedding without noticeable visual degradation.

The algorithm evaluates local image characteristics and adjusts embedding strength accordingly.

This approach aims to balance visual quality and robustness rather than applying a uniform modification across the entire image.

### Payload Encoding and Error Correction

The system supports short UTF-8 messages of up to 16 bytes.

Before embedding, the payload is encoded with additional information used for error detection and recovery.

The implementation uses:

- **Hamming(15,11)** error-correcting codes.
- **BLAKE2s** checksum validation.
- Spatial redundancy across multiple image regions.
- Synchronization signals to assist extraction.

Hamming(15,11) encodes 11 data bits into 15-bit codewords, allowing correction of a single-bit error within each codeword.

The checksum helps distinguish successfully recovered messages from corrupted payloads.

Spatial redundancy increases the probability that enough information remains available after partial image degradation.

### Blind Extraction

The extraction algorithm does not require the original image.

Instead, it analyzes the frequency-domain structure of the received image and attempts to recover the embedded payload.

This is known as **blind watermark extraction**.

The decoder performs synchronization, coefficient analysis, bit recovery, error correction, and payload validation.

The system includes mechanisms intended to compensate for limited changes in image alignment, scale, and rotation.

However, successful recovery depends on the image content and the transformations applied after embedding.

## Implementations

### Python

The Python implementation serves as an experimental reference implementation.

It provides command-line tools for embedding and extracting watermarks and uses a scientific computing stack that includes:

- NumPy
- SciPy
- OpenCV
- Pillow

Python provides a convenient environment for experimenting with signal-processing algorithms, inspecting intermediate results, and evaluating alternative approaches.

Repository: **[persistent_wtrmrk](https://github.com/ekomissarov/persistent_wtrmrk)**

### Rust

The Rust implementation follows the same watermark format while emphasizing performance, explicit data handling, and application-level usability.

It includes:

- A command-line interface.
- Parallel computation using Rayon.
- A desktop GUI built with egui.
- Image preview and zooming.
- Visualization of modified image regions.
- Background execution of computationally intensive operations.

The GUI provides an interface for embedding and extracting messages without using the command line.

Repository: **[watermark-rs](https://github.com/ekomissarov/watermark-rs)**

## Cross-Language Compatibility

An important design goal is interoperability between the two implementations.

Both use a compatible payload format and embedding layout, allowing images produced by one implementation to be processed by the other under compatible settings.

This provides an additional way to validate implementation correctness.

Cross-language testing is particularly useful for detecting discrepancies in:

- Binary encoding and decoding.
- Numerical computations.
- Pseudorandom sequence generation.
- Image preprocessing.
- Payload validation.

Maintaining compatibility requires the two implementations to agree not only on the high-level algorithm but also on its implementation details.

## Validation

The repositories document experiments involving several common image transformations.

These include:

| Transformation | Validation scope |
|---|---|
| JPEG recompression | Recovery tested after lossy compression |
| Resizing | Recovery tested after changes in image dimensions |
| Small rotations | Recovery tested under limited angular distortion |
| Cropping | Recovery tested under selected cropping conditions |
| Cross-language extraction | Compatibility tested between Python and Rust |

These results should be interpreted as experimental observations under specific conditions, not as guarantees of robustness.

Recovery performance depends on image characteristics, embedding parameters, and transformation severity.

A systematic benchmark across diverse image datasets would be necessary to characterize reliability more comprehensively.

## Limitations

Several limitations remain.

**Image dependence.** Some images provide fewer suitable regions for embedding, particularly those containing large uniform areas.

**Geometric transformations.** Synchronization can compensate for certain transformations, but sufficiently strong distortions may prevent successful extraction.

**Limited payload capacity.** The system prioritizes robustness over embedding large amounts of information.

**Physical reproduction.** Robustness to printing, scanning, and photographing physical copies has not yet been systematically validated.

**Security.** The watermark is not encrypted or cryptographically authenticated. It should not be treated as proof of image ownership, authorship, or authenticity.

The system is an experimental engineering project rather than a production-ready digital rights management solution.

## Future Work

Potential areas for further investigation include:

- Systematic robustness benchmarking.
- Quantitative image quality evaluation using PSNR and SSIM.
- More advanced error-correcting codes.
- Improved synchronization under geometric transformations.
- Performance comparisons between Python and Rust.
- Evaluation of physical print-and-scan recovery.

Of particular interest is measuring the trade-off between embedding strength, visual distortion, and extraction success rate.

## Source Code

Both implementations are publicly available:

- **Rust:** [github.com/ekomissarov/watermark-rs](https://github.com/ekomissarov/watermark-rs)
- **Python:** [github.com/ekomissarov/persistent_wtrmrk](https://github.com/ekomissarov/persistent_wtrmrk)

The repositories contain implementation details, usage instructions, and available tests.
