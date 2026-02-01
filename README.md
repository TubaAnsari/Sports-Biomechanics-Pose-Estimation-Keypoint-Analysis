# Sports-Biomechanics-Pose-Estimation-Keypoint-Analysis 
# Cricket Batting Biomechanics Analysis using Computer Vision

## Project Overview
A computer vision pipeline for analyzing cricket batting technique using pose estimation and biomechanical metrics. This project implements a complete sports analytics workflow from video input to actionable performance insights.

## Assignment Context
This project was developed as part of an AI/ML Computer Vision (Sports Analytics) Internship assignment, focusing on:
- **Pose-based movement analysis** for cricket batsmen
- **Biomechanical metric extraction** from side-view videos
- **Interpretable performance metrics** rather than just visualizations

## Project Structure
```bash
cricket-pose-analysis/
├── Biomechanis_analysis.ipynb # Main Colab notebook
├── requirements.txt # Dependencies
├── README.md # This file
│── skeleton_overlay.mp4 # Video with pose skeleton
│── cricket_batsman_keypoints.csv # Frame-wise keypoint coordinates
│── cricket_batsman_keypoints.json # Structured keypoint data
│── cricket_batting_metrics.png # Biomechanical metrics visualization
└── Input_vid(batsman).mp4
```


##  Quick Start

### 1. Google Colab Setup
```bash
# Run these in Colab cell
!pip install mediapipe opencv-python pandas numpy matplotlib scipy scikit-learn ffmpeg-python
```
###2. Upload Video

Upload a side-view cricket batting video
Recommended: 30+ FPS, 720p+ resolution, clear side view

### 3. Run the Analysis

Execute cells in order:

1. Install dependencies
2. Import libraries
3. Upload video
4. Download pose model
5. Initialize pose analyzer
6. Process video and extract landmarks
7. Create skeleton overlay
8. Compute biomechanical metrics
9. Generate visualizations

##  Technical Implementation
### Pose Estimation
1. Model: MediaPipe Pose Landmarker (Heavy model)
2. API: MediaPipe Tasks API (not deprecated solutions module)
3. Keypoints: 33 body landmarks
4. Processing: Real-time capable, GPU-accelerated

### Core Pipeline
1. Video Processing: Frame-by-frame pose detection using OpenCV
2. Landmark Extraction: 2D coordinates (x, y, z) with visibility scores
3. Skeleton Overlay: Green connections + red keypoints on original video
4. Data Export: CSV and JSON formats for further analysis
5. Metric Computation: Cricket-specific biomechanical metrics

## Biomechanical Metrics
### 1. Front Knee Angle Analysis
Purpose: Assess balance, weight transfer, and power generation

Measurement: Angle between hip, knee, and ankle

Optimal Range: 120-150° at impact, 90-120° in follow-through

Interpretation: Lower angles indicate better knee bend and weight transfer

### 2. Head Stability Metric
Purpose: Evaluate stability of head position during shot

Measurement: Variance in nose position (x, y coordinates)

Interpretation: Lower variance = more stable head = better technique

Importance: Stable head allows better judgment of line and length

### 3. Bat Speed Indicator
Purpose: Estimate bat speed and power generation

Measurement: Speed of bottom hand (wrist) movement

Calculation: Pixel displacement per second

Rating: Fast (>0.5), Moderate (0.3-0.5), Slow (<0.3)

### 4. Range of Motion Analysis: 

Purpose: Assess joint mobility and stroke completeness

Measurement: Difference between min and max joint angles

Interpretation: Higher ROM = better weight transfer and follow-through

##  Output Files
### 1. skeleton_overlay.mp4
Original video with pose skeleton overlay

Green lines: Body connections

Red dots: Joint keypoints

Format: MP4, same resolution as input

### 2. cricket_batsman_keypoints.csv
Frame-wise keypoint coordinates

Columns: frame, landmark_id, landmark_name, x, y, z, visibility

33 landmarks × number of frames

Suitable for spreadsheet analysis

### 3. cricket_batsman_keypoints.json
Structured keypoint data

Includes video metadata (FPS, resolution, frame count)

Nested frame-by-frame landmark data

Suitable for programmatic analysis

### 4. cricket_batting_metrics.png
Multi-panel visualization

Includes: Knee angle over time, head stability, bat speed, ROM

Publication-ready figure format

##  Model Improvement Analysis
### Observed Limitations

Jitter: Keypoint jitter during fast movements

Occlusion: Bat occlusion causes missed detections

Limb Confusion: Left/right confusion in extreme poses

Frame Drops: Missed detections in rapid motion

### Improvement Strategies
Temporal Smoothing: Kalman filtering for jitter reduction

Sport-specific Tuning: Fine-tune on cricket videos

Multi-frame Analysis: Use temporal context

Occlusion Handling: Predictive modeling for occluded joints

### Data Collection Plan
High-quality side-view videos of professional players

Multiple lighting conditions and backgrounds

Various shot types (drive, pull, sweep, cut)

Expert-annotated keypoints for validation

## Dependencies

mediapipe>=0.10.0

opencv-python>=4.8.0

numpy>=1.24.0

pandas>=2.0.0

matplotlib>=3.7.0

scipy>=1.10.0

scikit-learn>=1.3.0

ffmpeg-python>=0.2.0


##  Performance Notes
CPU Processing: ~10 FPS on Colab CPU

GPU Acceleration: ~30 FPS with Colab GPU (3x faster)

Memory Usage: ~2GB for 1080p video processing

Model Size: ~40MB (MediaPipe heavy model)

##  Usage Example
```python
# Initialize analyzer
analyzer = PoseAnalyzer()

# Process video
results = analyzer.process_video('batting_video.mp4')

# Create skeleton overlay
analyzer.create_skeleton_overlay('batting_video.mp4', 'output.mp4')

# Compute metrics
metrics = CricketBattingMetrics(results['landmarks'], results['video_info'])
knee_angles = metrics.get_joint_angle_over_time('front_knee')
stability = metrics.calculate_stability_metric()
```
##  Architecture
```
Input Video
    ↓
Frame Extraction (OpenCV)
    ↓
Pose Estimation (MediaPipe)
    ↓
Landmark Processing
    ├── Skeleton Overlay Generation
    ├── CSV/JSON Export
    └── Biomechanical Analysis
        ├── Joint Angle Computation
        ├── Stability Metrics
        ├── Speed Analysis
        └── Visualization
```
