# License Plate Recognition in a Parking Lot


## Overview

This project focuses on automatic motorcycle license plate detection and recognition for a parking lot environment. The system takes an image captured by a surveillance camera, detects the license plate region, segments the characters, and predicts the final license plate string.

The project is designed for a constrained parking lot scenario where:
- each image contains one license plate,
- the camera observes the vehicle from a mostly frontal, top-down angle,
- the plate is fully visible,
- and the distance from the camera is relatively stable.

## Problem Statement

Given an input image containing a motorcycle license plate, the goal is to:
1. detect the license plate region,
2. extract and normalize the plate,
3. segment individual characters,
4. recognize each character,
5. return the final license plate text.

## Pipeline

The full pipeline consists of two major stages.

### 1. License Plate Detection

The system uses **YOLOv8** to detect the bounding box of the license plate in the input image.

### 2. License Plate Recognition

After detection, the cropped plate goes through a recognition pipeline:
- **Rotation correction and cropping** using grayscale conversion, bilateral filtering, Otsu thresholding, Canny edge detection, and the Hough Transform.
- **Character segmentation** using **Connected Components Labeling (CCL)**.
- **Character recognition** using one of three classifiers:
  - **CNN**
  - **SVM**
  - **Softmax Regression**

## Methods and Models

### Detection Model
- **Model:** YOLOv8n
- **Framework:** Ultralytics YOLOv8
- **Task:** Object detection
- **Image size:** 640 × 640
- **Epochs:** 50
- **Batch size:** 32

### Character Recognition Models

#### CNN
- Input size: 32 × 32
- Framework: TensorFlow
- Optimizer: Adam
- Epochs: 100
- Early stopping epoch: 14
- Best epoch: 4
- Batch size: 32

#### SVM
- Input size: 20 × 20
- Framework: scikit-learn
- Kernel: Linear

#### Softmax Regression
- Input size: 20 × 20
- Framework: scikit-learn
- Max iterations: 250
- Multi-class setting: multinomial

## Datasets

### 1. License Plate Detection Dataset
- Total labeled with Roboflow
- Training set: **1,244 images** (70%)
- Validation set: **175 images** (10%)
- Testing set: **349 images** (20%)

### 2. Character Recognition Dataset
- Extracted using Connected Components Labeling
- Total: **3,722 character images**
- Number of classes: **31 characters**
- Training set: **3,347 images** (90%)
- Testing set: **375 images** (10%)

## Evaluation Metrics

### Detection
- Precision
- Recall
- F1-Score
- mAP@0.5
- mAP@0.5:0.95

### Character Recognition
- Accuracy
- Precision
- Recall
- F1-Score

### Full Pipeline
- Accuracy = correctly recognized license plates / total license plates
- A plate is counted as correct only when **all characters** are predicted correctly.

## Results

### YOLOv8 Detection

| Model | Precision | Recall | F1-Score | mAP@0.5 | mAP@0.5:0.95 |
|---|---:|---:|---:|---:|---:|
| YOLOv8 | 99.98% | 100% | 100% | 99.5% | 94.1% |

### Character Recognition

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|
| CNN | 91.73% | 93.08% | 92.40% | 91.75% |
| Softmax Regression | 91.64% | 93.08% | 92.18% | 92.03% |
| SVM | **93.86%** | **94.96%** | **94.19%** | **94.09%** |

### End-to-End Pipeline

| Model | Accuracy |
|---|---:|
| YOLO + CNN | **72.99%** |
| YOLO + SVM | 68.39% |
| YOLO + Softmax Regression | 65.8% |

## Limitations

The current system still has several limitations:
- **Sensitivity to lighting:** glare can distort characters and hurt segmentation quality.
- **Characters attached to the plate border:** CCL may remove characters that touch the frame.
- **Character confusion:** visually similar characters such as **8** and **B** may be confused.
- **Obstacles on the plate:** rivets or occlusions may deform character shapes.

## Future Work

Possible improvements for the project include:
- extending the system to other vehicle types,
- improving robustness to environmental conditions,
- exploring better character segmentation methods,
- and deploying the system as a practical software application.

## Technology Stack

- Python
- OpenCV
- Ultralytics YOLOv8
- TensorFlow
- scikit-learn
- NumPy
