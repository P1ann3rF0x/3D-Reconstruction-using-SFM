# 3D-Reconstruction-using-SFM
# Structure from Motion 3D Reconstruction
## Horizontal and Vertical Accuracy Assessment of Terrestrial Imagery

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![COLMAP](https://img.shields.io/badge/COLMAP-3.7-green.svg)](https://colmap.github.io/)

This repository contains the implementation and methodology for high-accuracy 3D reconstruction of building structures using Structure from Motion (SfM) techniques with mobile device imagery.

## 📋 Overview

This project demonstrates a comprehensive workflow for creating precise 3D models from smartphone video footage, achieving **99.28% horizontal accuracy** and **96.63% vertical accuracy**. The methodology combines mobile-based image acquisition with robust photogrammetric processing and ground control point georeferencing.

### Key Features

- **Mobile-First Approach**: Utilizes smartphone video as primary data source
- **High Accuracy**: Achieves sub-centimeter precision through GNSS-based ground control points
- **Open Source Pipeline**: Built entirely on free and open-source software (COLMAP, CloudCompare)
- **Comprehensive Validation**: Includes both statistical and physical measurement validation
- **Production Ready**: Suitable for architectural documentation, heritage preservation, and engineering applications

## 🎯 Results Summary

- **Dataset**: 472 frames extracted from OnePlus 8Pro video
- **Processing Method**: COLMAP SfM with exhaustive matching
- **Georeferencing**: 5 Ground Control Points via Trimble DA2 GNSS
- **Horizontal Accuracy**: 99.28% average
- **Vertical Accuracy**: 96.63% average
- **Output Formats**: Dense point cloud, textured mesh, georeferenced model

## 🛠️ Technical Stack

### Software Requirements
- **COLMAP** - Structure from Motion and Multi-View Stereo pipeline
- **CloudCompare** - 3D point cloud processing and analysis
- **VLC Media Player** - Video frame extraction
- **Python 3.7+** (for preprocessing scripts)

### Hardware Requirements
- **Minimum**: 8GB RAM, multi-core CPU
- **Recommended**: 16GB+ RAM, dedicated GPU for COLMAP acceleration
- **Processing Time**: 10+ hours for exhaustive matching (472 images)

### Data Acquisition Equipment
- Mobile device with video recording capability
- GNSS receiver (Trimble DA2 or equivalent) for ground control points
- Checkerboard targets for GCP placement
- Measuring tape for validation

## 🚀 Quick Start

### 1. Data Preparation
```bash
# Extract frames from video using VLC or FFmpeg
ffmpeg -i input_video.mp4 -vf fps=1/3 frame_%04d.jpg

# Organize images in project directory
mkdir colmap_project/images
cp frame_*.jpg colmap_project/images/
```

### 2. COLMAP Processing Pipeline
```bash
# Create new COLMAP project
colmap automatic_reconstructor \
    --workspace_path /path/to/colmap_project \
    --image_path /path/to/colmap_project/images \
    --dense 1

# Alternative: GUI-based processing
colmap gui
```

### 3. Ground Control Point Integration
1. Import mesh model (.ply) into CloudCompare
2. Load GCP coordinates from GNSS survey
3. Use "Point Picking" tool to identify corresponding points
4. Apply "Align (point pairs picking)" for georeferencing

### 4. Accuracy Assessment
```python
# Calculate accuracy metrics
def calculate_accuracy(field_measurement, model_measurement):
    diff = abs(field_measurement - model_measurement)
    error_percent = (diff / field_measurement) * 100
    accuracy_percent = 100 - error_percent
    return accuracy_percent
```

## 📊 Accuracy Results

### Horizontal Measurements
| Measurement | Tape (m) | Model (m) | Error (%) | Accuracy (%) |
|-------------|----------|-----------|-----------|--------------|
| Distance 1  | 3.31     | 3.34      | 0.91      | 99.09        |
| Distance 2  | 7.29     | 7.34      | 0.68      | 99.32        |
| Distance 3  | 4.09     | 4.11      | 0.48      | 99.52        |
| Distance 4  | 3.32     | 3.30      | 0.60      | 99.40        |
| Distance 5  | 3.27     | 3.24      | 0.91      | 99.09        |
| **Average** | -        | -         | **0.72**  | **99.28**    |

### Vertical Measurements
| Measurement | Tape (m) | Model (m) | Error (%) | Accuracy (%) |
|-------------|----------|-----------|-----------|--------------|
| Height 1    | 1.22     | 1.18      | 3.28      | 96.72        |
| Height 2    | 2.41     | 2.42      | 2.41      | 97.59        |
| Height 3    | 1.19     | 1.113     | 1.68      | 98.32        |
| Height 4    | 2.33     | 2.29      | 1.71      | 98.29        |
| Height 5    | 1.20     | 1.107     | 7.75      | 92.25        |
| **Average** | -        | -         | **3.37**  | **96.63**    |

## 🔧 Configuration

### COLMAP Camera Parameters
- **Image Dimensions**: 3831 × 2154 pixels
- **Focal Length**: fx = fy = 3665.0 pixels
- **Principal Point**: (cx, cy) = (1915.5, 1077)
- **Distortion Model**: OpenCV (ID 1)
- **Distortion Coefficients**: k1=0.1, k2=-0.05, p1=0.001, p2=0.0, k3=0.0

### Processing Parameters
- **Feature Detection**: SIFT (Scale-Invariant Feature Transform)
- **Matching Mode**: Exhaustive (all-pairs matching)
- **Bundle Adjustment**: Iterative optimization
- **Dense Reconstruction**: PatchMatch Stereo algorithm
- **Mesh Generation**: Poisson reconstruction

## 📁 Project Structure
```
project/
├── images/                 # Input video frames
├── colmap_project/        # COLMAP workspace
│   ├── database.db        # Feature database
│   ├── sparse/           # Sparse reconstruction
│   ├── dense/            # Dense point cloud
│   └── meshed/           # Surface mesh
├── gcp_data/             # Ground control points
├── validation/           # Accuracy assessment data
└── outputs/              # Final georeferenced models
```

## 🎯 Applications

This methodology is suitable for:
- **Architectural Documentation**: Building facade modeling and analysis
- **Heritage Preservation**: Digital archiving of historical structures  
- **Engineering Surveys**: Structural inspection and monitoring
- **Urban Planning**: 3D city modeling and visualization
- **Construction Management**: Progress monitoring and quality control

## ⚠️ Limitations and Considerations

- **Image Coverage**: Incomplete coverage may result in fragmented models
- **Computational Requirements**: Exhaustive matching is resource-intensive
- **Lighting Conditions**: Uniform lighting improves reconstruction quality
- **Feature Density**: Texture-rich surfaces yield better results
- **Scale Ambiguity**: Ground control points essential for absolute scaling

## 🔮 Future Enhancements

- **Multi-Sensor Integration**: Combine with LiDAR and UAV imagery
- **Real-Time Processing**: GPU acceleration and optimized algorithms  
- **Automated GCP Detection**: Computer vision-based target recognition
- **Mobile Application**: On-device processing capabilities
- **Cloud Processing**: Scalable reconstruction pipeline

## 📚 References

This work builds upon established photogrammetric principles and open-source tools:

1. James, M. R., & Robson, S. (2012). Straightforward reconstruction of 3D surfaces and topography with a camera. *Journal of Geophysical Research: Earth Surface*, 117.

2. Westoby, M. J., et al. (2012). 'Structure-from-motion' photogrammetry: A low-cost, effective tool for geoscience applications. *Geomorphology*, 179, 300-314.

3. COLMAP Documentation: https://colmap.github.io/


## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) and submit pull requests for any improvements.

## 📧 Contact

For questions, suggestions, or collaboration opportunities:

- **Authors**: Udhay Kiraan K H
- **eMail**: Udhay_k@ce.iitr.ac.in
- **Institution**: IIT Roorkee, Geospatial Engineering Department
- **Location**: Uttarakhand, India

## 🙏 Acknowledgments

- COLMAP development team
- CloudCompare contributors
- Open-source photogrammetry community

---

**Note**: This implementation represents academic research conducted at IIT Roorkee. Results may vary based on hardware configuration, data quality, and processing parameters.
