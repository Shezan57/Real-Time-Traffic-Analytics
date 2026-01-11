# Real-Time Traffic Analytics

A computer vision system that detects vehicles in dashcam video and estimates their speed using **Perspective Transformation (Homography)**.

## 🎯 Features

- **Vehicle Detection**: YOLOv11 (latest model) for detecting cars, trucks, motorcycles, buses
- **Object Tracking**: ByteTrack for consistent vehicle IDs across frames
- **Speed Estimation**: Perspective transform maps pixels to real-world meters
- **Anomaly Detection**: Highlights speeding vehicles (>100 km/h) in RED
- **Dashboard**: Real-time statistics display

## 🧮 The Math: Homography Explained

The core of speed estimation is **Perspective Transformation**:

```
A homography is a 3x3 matrix H that maps points from image plane to ground plane:

[x']   [h11 h12 h13]   [x]
[y'] = [h21 h22 h23] * [y]
[w']   [h31 h32 h33]   [1]

Real coordinates: x' = (h11*x + h12*y + h13) / w'
                  y' = (h21*x + h22*y + h23) / w'
```

**Why do we need this?**
- Objects far from camera appear smaller (perspective distortion)
- 10 pixels near camera ≠ 10 pixels far from camera in real distance
- Homography corrects this to calculate actual meters traveled

## 📁 Project Structure

```
├── traffic_analytics_colab.py  # Main Colab notebook (copy cells)
├── config.py                    # Configuration parameters
├── perspective_transform.py     # Homography math module
├── tracker.py                   # Vehicle detection & tracking
├── visualizer.py               # Annotation & drawing
└── README.md
```

## 🚀 Quick Start (Google Colab)

1. Open Google Colab
2. Copy cells from `traffic_analytics_colab.py` one by one
3. Run each cell in order
4. Adjust `SOURCE_POLYGON` points for your specific video

## ⚙️ Configuration

Edit these in the notebook for your video:

```python
# Detection zone (4 points on the road in pixels)
SOURCE_POLYGON = np.array([
    [400, 300],   # Top-left
    [880, 300],   # Top-right
    [1100, 600],  # Bottom-right
    [180, 600],   # Bottom-left
], dtype=np.float32)

# Real-world dimensions (meters)
TARGET_RECT = np.array([
    [0, 0], [3.7, 0], [3.7, 20], [0, 20]  # 3.7m wide, 20m long
], dtype=np.float32)

SPEED_LIMIT_KMH = 100.0  # Speeding threshold
```

## 📊 Output

- Annotated video with:
  - Cyan detection zone overlay
  - Green boxes for normal vehicles
  - Red boxes for speeding vehicles
  - Speed labels on each vehicle
  - Dashboard with statistics

## 🔧 Tech Stack

- **Model**: YOLOv11-Medium (ultralytics)
- **Tracking**: ByteTrack (supervision)
- **Transform**: cv2.getPerspectiveTransform
- **Visualization**: OpenCV + supervision

## 📝 License

MIT License
