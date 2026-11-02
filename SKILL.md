---
name: geoskill-object-detection-yolo
description: '滑窗+HOG/阈值特征+NMS+地理编码，输出检测框GeoJSON（离线numpy等价实现）'
---

# 遥感目标检测 | Remote Sensing Object Detection

Automatically detects targets of interest in remote sensing imagery (bright/high-contrast features such as buildings, vehicles, and ships), and outputs detection-box GeoJSON with WGS-84 geographic coordinates plus a score raster.

This skill is an **offline numpy-equivalent implementation** of deep object detectors such as YOLO: without relying on torch/ultralytics, it fully reproduces the detection pipeline of "candidate generation → redundancy suppression → geographic output" via sliding-window scanning + objectness scoring (local brightness z-score or HOG gradient energy) + non-maximum suppression (NMS) + pixel-box geocoding, and every step can be verified by unit tests independently.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-object-detection-yolo.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2: real imagery + HOG features

```bash
python geoskill-object-detection-yolo.py --input scene.tif --feature hog --score-thresh 2.0 --output-dir ./out
```

### Example 3: adjust window/step/NMS thresholds

```bash
python geoskill-object-detection-yolo.py --bbox 121.4 31.1 121.6 31.3 --synthetic --win-size 12 --step 4 --iou-thresh 0.3 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `detections.geojson` | GeoJSON | Detection-box polygons + score/pixel_box attributes |
| `score_map.tif` | GeoTIFF | Detection score raster (max within boxes) |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/exit code) |

## Data Source / 数据源 / Source

Local single/multi-band GeoTIFF (first band used as intensity), or `--synthetic` scene (dark background + bright square targets).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
