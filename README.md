# object-detection-distance-estimation-bdd100k
YOLOv8n fine-tuned on BDD100K for navigation object detection with monocular distance estimation and ONNX edge optimization

# Object Detection + Distance Estimation for Robotics Navigation
### YOLOv8 | BDD100K | Monocular Depth | ONNX Edge Optimization

## Approach

### 1. Object Detection
**Model:** YOLOv8n — transfer learning from COCO pretrained weights  
**Dataset:** BDD100K — 69,863 train / 10,000 val images

| Class ID | Label | BDD100K Source |
|---|---|---|
| 0 | Traffic Sign | `traffic sign` |
| 1 | Vehicle | `car` + `truck` + `bus` merged |

> **Note:** BDD100K does not contain `cone` or `barrier` classes.
> `traffic sign` covers stop signs, warnings and speed limits.
> Production deployment would require a custom-labelled dataset for cones/barriers.

| Param | Value |
|---|---|
| Epochs | 20 |
| Optimizer | AdamW, lr=0.001 |
| Image size | 640×640 |
| Train images | 5,000 |
| Confidence threshold | 0.40 |

| Class | mAP50 | mAP50-95 |
|---|---|---|
| Traffic Sign | 0.439 | 0.212 |
| Vehicle | 0.690 | 0.411 |
| **All** | **0.564** | **0.312** |

### 2. Distance Estimation
Pinhole camera model: `D = (H_real × f) / H_pixel`

- Focal length: **1050px** (calibrated from BDD100K dashcam ~60° FOV)
- Traffic Sign real height: **0.75m**
- Vehicle real height: **1.50m**
- Height used over width — more stable across viewing angles

### 3. Edge Optimization

| Model | Device | FPS |
|---|---|---|
| PyTorch FP32 | GPU (T4) | 86.03 |
| PyTorch FP32 | CPU | 10.02 |
| ONNX FP16 | CPU | 10.58 |

ONNX FP16 CPU gain is minimal on x86 (no native FP16 ALU).
On ARM/Jetson Nano, FP16 gives ~2–3× speedup.
Next step for production: TensorRT INT8.

### 4. Optical Flow (Extra Credit)
Lucas-Kanade sparse optical flow tracks keypoints on detected
objects between consecutive frames, visualizing motion vectors
for velocity estimation and collision prediction.

## How to Run
1. Open notebook in Google Colab with T4 GPU
2. Generate fresh Kaggle token at kaggle.com/settings
3. Replace token in Cell 1
4. Run all cells sequentially

---

## Contact Information
**Name:** Suryansh Shah  
**Contact:** +91-XXXXXXXXXX  
**Email:** your@email.com  
**GitHub:** suryanshshah2006
