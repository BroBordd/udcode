# UD Code (Ultra-Dense Code)

> Convert any file into a high-capacity, camera-scannable square image and back, achieving up to 10x the data density of traditional QR codes.

**UD Code** is a small dumb Python utility designed for ultra-dense data storage and transfer. It uses a custom **8-color palette (3 bits per pixel)**, combined with built-in compression and robust alignment markers, to securely and efficiently encode binary files into images.

---

## Features

* **Ultra-Dense Encoding:** Achieves high efficiency by mapping 3 data bits to every logical pixel.
* **Built-in Compression:** Uses aggressive `zlib` compression (level 9) to minimize image size and maximize data capacity.
* **Camera-Ready:** Generates square images with large, high-contrast pixels (8x8 screen pixels) and four corner markers for reliable scanning and alignment by cameras/scanners.
* **Error Detection:** Includes a CRC32 checksum in the header for detecting corrupted data during decoding.
* **Universal:** Encodes *any* file type (executables, archives, documents, etc.).

---

## Installation

### Prerequisites

You need **Python 3** and the **Pillow (PIL)** library:

```bash
pip install Pillow
```

### Usage

1.  Clone the repository:
    ```bash
    git clone https://github.com/BroBordd/udcode.git
    cd udcode
    ```
2.  Run the script directly using `python udcode.py`.

---

## Usage Examples

The tool uses three main commands: `encode`, `decode`, and `info`.

### 1. Encode (File to Image)

Convert a file (like a PDF or ZIP archive) into a `.png` image.

```bash
# Basic encoding with compression enabled (default)
python udcode.py encode document.pdf output.png

# Encode a pre-compressed file, disabling internal compression
python udcode.py encode large_data.zip qrcode.png --no-compress
```

### 2. Decode (Image to File)

Reconstruct the original file from a **UD Code** image.

```bash
# Decode the image back to the original filename
python udcode.py decode output.png restored.pdf

# Decode and pipe output directly to stdout
python udcode.py decode qrcode.png - > restored_data.bin
```

### 3. Pipeline / Pipe Support (stdin/stdout)

**UD Code** fully supports piping (`-`) for efficient command-line workflows.

```bash
# Pipe a file's content directly into the encoder
cat large_log.txt | python udcode.py encode - output.png

# Decode from an image and view the contents (if text)
python udcode.py decode output.png - | head
```

---

## ⚙️ Technical Specifications

| Parameter | Value | Details |
| :--- | :--- | :--- |
| **Data Encoding** | 3 bits/pixel | Utilizes a custom **8-color palette** (RGB values) for maximum data density. |
| **Marker Size** | 3x3 logical pixels | Four corner markers are implemented using a Black/White checkerboard pattern for robust perspective correction. |
| **Logical Pixel Size** | 8x8 physical pixels | Ensures each data element is large enough to be clearly resolved by standard smartphone cameras. |
| **Header** | 13 bytes | Stores compressed size, original size, CRC32 checksum, and compression flag. |

### The 8-Color Palette

The system relies on 8 high-contrast colors to minimize errors during scanning:

* Black `(0, 0, 0)` (000)
* Red `(255, 0, 0)` (001)
* Green `(0, 255, 0)` (010)
* Blue `(0, 0, 255)` (011)
* Yellow `(255, 255, 0)` (100)
* Magenta `(255, 0, 255)` (101)
* Cyan `(0, 255, 255)` (110)
* White `(255, 255, 255)` (111)

---

## License

This project is licensed under the MIT License.
