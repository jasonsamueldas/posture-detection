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

## Dataset Used
- Used [posture-recognition](https://www.kaggle.com/datasets/sahasradityathyadi/posture-recognition) dataset from Kaggle

### Python Dependencies

Install required packages with pip:

```bash
pip install numpy opencv-python onnxruntime
```

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

## 🔧 Training the Model on Jetson Nano

To train the posture detection model from scratch using NVIDIA Jetson Nano and Jetson Inference tools:

### 1. Clone Jetson Inference Repository

```bash
git clone --recursive https://github.com/dusty-nv/jetson-inference
```

> ⚠️ This may take 10–15 minutes depending on your internet speed.

---

### 2. Run the Docker Container

```bash
cd jetson-inference
docker/run.sh
```

---

### 3. Prepare the Dataset

- Download your dataset (e.g., from Kaggle).
- Extract it to the following directory:

```
jetson-inference/python/training/classification/data/
```

- Inside your dataset folder (e.g., `data/Posture`), create a `labels.txt` file listing the classes. Example:
  ```
  good-posture
  bad-posture
  ```

---

### 4. Train the Model

Start training using:

```bash
python3 train.py --model-dir=models/Posture --batch-size=4 --workers=1 --epochs=100 data/Posture
```

> ⚠️ Training on Jetson Nano may take 12–14 hours. Avoid touching the device while it’s hot.

---

### 5. Export the Model to ONNX

Once training is complete, export the model:

```bash
python3 onnx_export.py --model-dir=models/Posture
```

Repeat for any other datasets if needed.

---

### 6. Test the Exported Model

Run a test inference using:

```bash
imagenet --model=models/Posture/resnet18.onnx          --input_blob=input_0          --output_blob=output_0          --labels=data/Posture/labels.txt          data/Project/Input data/Project/Output
```

---

### 7. Improve Model Accuracy

To improve performance:
- Tune hyperparameters (e.g., epochs, batch size).
- Use more balanced and diverse data.
- Re-run training as needed.

## License

MIT License