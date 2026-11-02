# 遥感目标检测 | Remote Sensing Object Detection

在遥感影像上自动检测感兴趣目标（建筑、车辆、船只等明亮/高对比地物），输出带 WGS-84 地理坐标的检测框 GeoJSON 与得分栅格。

本 skill 是 YOLO 等深度目标检测器的**离线 numpy 等价实现**：不依赖 torch/ultralytics，用滑窗扫描 + 目标性打分（局部亮度 z-score 或 HOG 梯度能量）+ 非极大值抑制（NMS）+ 像素框地理编码，完整复现"候选生成 -> 冗余抑制 -> 地理输出"的检测流水线，每一步都可单独单元测试验证。

## 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-object-detection-yolo.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 示例 2：真实影像 + HOG 特征

```bash
python geoskill-object-detection-yolo.py --input scene.tif --feature hog --score-thresh 2.0 --output-dir ./out
```

### 示例 3：调整窗口/步长/NMS 阈值

```bash
python geoskill-object-detection-yolo.py --bbox 121.4 31.1 121.6 31.3 --synthetic --win-size 12 --step 4 --iou-thresh 0.3 --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `detections.geojson` | GeoJSON | 检测框多边形 + score/pixel_box 属性 |
| `score_map.tif` | GeoTIFF | 检测得分栅格（框内最大值） |
| `output-manifest.json` | JSON | 运行清单（输入/输出/QA/退出码） |

## 数据源 / Source

本地单/多波段 GeoTIFF（取首波段作强度），或 --synthetic 合成场景（暗背景 + 明亮方形目标）。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传任何用户数据。

## License

MIT
