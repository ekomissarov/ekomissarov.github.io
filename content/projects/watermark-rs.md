+++
title = 'Invisible Image Watermarking'
date = 2026-10-30
draft = false
description = 'Experimental invisible image watermarking implemented in Python and Rust, featuring blind extraction, error correction, and a desktop GUI.'
tags = ['Rust', 'Python', 'Image Processing', 'DCT', 'Error Correction']
+++

An experimental image watermarking system implemented in **Python and Rust**, with a command-line interface and a desktop GUI.

The project explores how short messages can be embedded into images with minimal visible changes and recovered without access to the original image.

**Source code:**

- [Rust implementation](https://github.com/ekomissarov/watermark-rs)
- [Python implementation](https://github.com/ekomissarov/persistent_wtrmrk)

## Overview

The main engineering challenge is balancing three competing objectives:

- **Imperceptibility:** minimize visible changes to the original image.
- **Robustness:** preserve the embedded information after common image transformations.
- **Recoverability:** extract the message without requiring the original image.

The implementation uses frequency-domain watermarking, spatial redundancy, error-correcting codes, and synchronization signals.

The system is designed primarily as a research prototype rather than a production-ready document protection solution.

## How It Works

### Frequency-Domain Embedding

The image is divided into 16 × 16 pixel blocks.

Each block is transformed using the Discrete Cosine Transform (DCT). Information is encoded by modifying the relationship between two mid-frequency coefficients.

The embedding strength is adjusted according to local image characteristics.

This content-adaptive approach attempts to avoid visually sensitive areas, such as uniform backgrounds and text-like regions.

### Redundancy and Error Correction

The watermark contains a short UTF-8 message of up to 16 bytes.

The message is protected using:

- Hamming(15,11) error-correcting codes.
- A BLAKE2s checksum for detecting corrupted payloads.
- A repeated spatial pattern.
- Synchronization signals for recovering alignment.

These mechanisms help the decoder recover information after certain image transformations.

### Blind Extraction

The decoder does not require the original image.

It searches for the watermark using frequency-domain observations and synchronization signals.

The extraction process can compensate for some changes in scale, alignment, and rotation.

Recovery is not guaranteed for every image or transformation.

## Two Implementations

### Python

The Python implementation provides a command-line interface for embedding and extracting watermarks.

It uses NumPy, SciPy, OpenCV, and Pillow.

The implementation also serves as a reference for cross-language compatibility testing.

### Rust

The Rust implementation provides:

- A command-line interface.
- Parallel processing using Rayon.
- A desktop graphical interface built with egui.
- Interactive image preview and zooming.
- Visualization of modified image regions.
- Background processing for computationally expensive operations.

Both implementations use a compatible watermark format with the default layout seed.

## Cross-Language Compatibility

One of the project's design goals is interoperability.

An image watermarked by the Python implementation can be decoded by the Rust implementation, and vice versa.

The repositories include cross-language tests covering basic interoperability and selected image transformations.

## Validation

The documented tests include successful recovery after:

- JPEG recompression.
- Image resizing.
- Small rotations.
- Selected cropping operations.

These results demonstrate functionality under specific test conditions rather than general robustness guarantees.

Physical print-and-scan recovery remains an open validation task.

## Limitations

The project is experimental.

Watermark recovery depends on image size, texture, and the severity of subsequent transformations.

Images with large uniform areas may not provide enough suitable regions for reliable embedding.

The watermark is not encrypted or cryptographically authenticated. It should not be considered proof of ownership or document authenticity.

## Source Code

The project is available in two repositories:

**[watermark-rs — Rust CLI and GUI](https://github.com/ekomissarov/watermark-rs)**

**[persistent_wtrmrk — Python implementation](https://github.com/ekomissarov/persistent_wtrmrk)**
