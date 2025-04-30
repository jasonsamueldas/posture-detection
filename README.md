# Posture Detection with ONNX and OpenCV

This project is a real-time posture detection system that uses a webcam feed and a ResNet-18 model (in ONNX format) to classify posture as either **good** or **bad**. If bad posture is detected, the system alerts the user via sound and on-screen messages.

## Features

- Real-time posture classification using ONNX model inference
- Visual and audio alerts for bad posture
- Time tracking for good and bad posture
- User-friendly on-screen interface with statistics

## Requirements

- Python 3.7+
- Windows OS (due to `winsound` usage for audio)
- Webcam

### Python Dependencies

Install required packages with pip:

```bash
pip install numpy opencv-python onnxruntime
```

## Dataset Used
- Used [posture-recognition](https://www.kaggle.com/datasets/sahasradityathyadi/posture-recognition) dataset from Kaggle

## Setup

1. **Model & Labels**:
   - Place your `resnet18.onnx` model and `labels.txt` in the specified directory:
     ```
     C:\Users\jason\temp1\posture-detection\src\
     ```
   - `labels.txt` should contain two lines (in lowercase):
     ```
     good-posture
     bad-posture
     ```

2. **Adjust Configurations**:
   - If necessary, modify the `MODEL_PATH`, `LABELS_PATH`, and `CAMERA_INDEX` in the script to match your setup.

## Running the Script

Run the script using Python:

```bash
python posture_detection.py
```

You will see the webcam window with posture status, confidence, and session time overlays.

Press `q` to quit the program at any time.

## Audio Alerts

- A loud beep plays when bad posture is detected.
- The frame freezes briefly to visually reinforce the alert.
- Additional beeps repeat every 10 seconds if bad posture persists.

## Output Example

On exiting the program, you'll get a summary like:

```
Tracking stopped.
Total session time: 14:20
Good posture time: 10:35 (73.8%)
Bad posture time: 03:45 (26.2%)
```

## Notes

- Make sure your webcam has sufficient lighting and your upper body is visible.
- This project assumes binary classification between "good" and "bad" posture.
- For cross-platform sound support, replace `winsound.Beep()` with an alternative like `playsound`.


