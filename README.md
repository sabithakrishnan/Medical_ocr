# Production-Grade Medical Report OCR Processing Pipeline

This document contains a complete, fault-tolerant Python implementation designed to batch-process scanned medical lab reports from an input directory, clean layout discrepancies using deep-learning spatial word boundary analysis, and compile the results into a unified tabular master spreadsheet.

---

## 🛠️ System Prerequisites & Installation

Unlike traditional layout-dependent OCR platforms (like Pytesseract), this pipeline utilizes a deep-learning sequence analyzer (**EasyOCR**) that groups character arrays based on their relative pixel spatial geometry matrices (`X` and `Y` coordinate windows). This makes it highly resilient against page rotation, camera angles, and surface glare artifacts.

Run the following installation command in your terminal terminal workspace environment:

```bash
pip install easyocr pandas opencv-python pillow touch
```

*Note: The script defaults to CPU computation mode. If your workstation environment has a dedicated NVIDIA CUDA core architecture configuration available, toggle `gpu=True` inside the EasyOCR reader instantiation line for enhanced multi-threaded throughput execution speeds.*

---



## 📈 Core Architectural Mechanics Breakdown

1. **`BBox Spatial Grouping`**: The pipeline calculates the vertical layout line coordinate center `(bbox[0][1] + bbox[2][1]) / 2`. If multiple strings are printed at roughly the same height level across the page, they are clustered into the same execution list block.
2. **`X-Coordinate Sorting`**: Items inside each clustered height level are sorted by their horizontal index `bbox[0][0]`. This naturally strings text segments into correct, un-jumbled left-to-right readable line streams, regardless of bad formatting margins.
3. **`Anchor-Based Slicing`**: Rather than executing rigid index columns, the processing logic searches for reference patterns like `(\d+ - \d+)`. Finding the reference range acts as a structural baseline anchor point to reliably separate test results from units.
