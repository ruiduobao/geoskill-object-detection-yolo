# Remote Sensing Object Detection (geoskill-object-detection-yolo)

> Sliding window + HOG/threshold features + NMS + geocoding, outputting detection-box GeoJSON (offline numpy equivalent implementation)

---

## 1. Overview

Automatically detects objects of interest (bright/high-contrast features such as buildings, vehicles, and ships) in remote sensing imagery, and outputs detection-box GeoJSON with WGS-84 geographic coordinates plus a score raster. This skill is an **offline numpy equivalent implementation** of deep object detectors such as YOLO: without relying on torch/ultralytics, it fully reproduces the "candidate generation → redundancy suppression → geographic output" detection pipeline using sliding-window scanning + objectness scoring (local brightness z-score or HOG gradient energy) + non-maximum suppression (NMS) + pixel-box geocoding, and every step can be verified by standalone unit tests.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-object-detection-yolo.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `detections.geojson` | GeoJSON | Detection-box polygons + score/pixel_box attributes |
| `score_map.tif` | GeoTIFF | Detection score raster (maximum within boxes) |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
