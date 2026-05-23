# 👁️ Eye-Controlled Video Player

> A Human-Computer Interaction (HCI) project that enables **hands-free video playback control** using real-time eye tracking and blink detection via webcam.

---

## 📌 Overview

The **Eye-Controlled Video Player** is an assistive technology and HCI research project that uses computer vision to track the user's pupils and detect blinks in real time. The system maps eye movements to mouse cursor movement and uses blinks as click events — allowing users to control a video player **entirely without hands**.

This project was developed as part of an HCI course and demonstrates how gaze-based interaction can be used to enhance accessibility and explore natural user interfaces.

---

## ✨ Features

- 🎥 **Video Playback** — Load and play `.mp4`, `.avi`, and `.mkv` video files
- 👁️ **Real-Time Eye Tracking** — Tracks pupil position to move the mouse cursor
- 😉 **Blink-to-Click** — Detects eye blinks and triggers mouse clicks automatically
- 📷 **Live Camera Feed** — Displays the webcam feed with pupil overlay in the UI
- ⏯️ **Playback Controls** — Previous, Next, Play/Pause, Skip Forward/Backward (±10s)
- 🎞️ **Playlist Support** — Add multiple videos and navigate between them
- 🖥️ **Standalone Executable** — Can be packaged as a `.exe` using PyInstaller

---

## 🛠️ Tech Stack

| Component         | Technology                  |
|-------------------|-----------------------------|
| GUI Framework     | `tkinter`                   |
| Computer Vision   | `OpenCV (cv2)`              |
| Face/Eye Tracking | `MediaPipe Face Mesh`       |
| Mouse Control     | `PyAutoGUI`                 |
| Video Playback    | `python-vlc` (LibVLC)       |
| Image Processing  | `Pillow (PIL)`              |
| Numeric Computing | `NumPy`                     |
| Packaging         | `PyInstaller`               |

---

## 📁 Project Structure

```
eye-tracking-HCI--main/
│
├── test.py              # Main application entry point
├── test.spec            # PyInstaller build specification
├── .gitignore           # Git ignore rules
│
└── HCI PROJECT/
    └── Eye-Controlled_Cursor_Phase1 (2).pdf   # Project report / documentation
```

---

## ⚙️ How It Works

### 1. Face Mesh & Landmark Detection
Using **MediaPipe Face Mesh**, the app detects 468 facial landmarks on the user's face in real time. The **iris landmark (index 468)** is used to determine the pupil's current position.

### 2. Gaze-to-Cursor Mapping
The pupil coordinates from the camera frame are interpolated to the screen's full resolution using `numpy.interp`. **Exponential smoothing** (`smooth_factor = 0.2`) prevents jitter and provides a stable cursor experience.

### 3. Blink Detection (Eye Aspect Ratio)
The **Eye Aspect Ratio (EAR)** formula is used to detect blinks:

```
EAR = (vertical_dist_1 + vertical_dist_2) / (2 × horizontal_dist)
```

When the EAR drops below the threshold (`0.20`), a blink is detected and a mouse click is triggered via `pyautogui.click()`. A flag prevents repeated clicks from a sustained closed-eye state.

### 4. Threaded Architecture
- The **camera loop** runs in a background daemon thread to avoid blocking the GUI.
- Click events are dispatched via short-lived threads to prevent UI lag.

---

## 🚀 Getting Started

### Prerequisites

- Python **3.8 – 3.11** (recommended)
- A working **webcam**
- **VLC Media Player** installed (required by `python-vlc`)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/eye-tracking-HCI.git
   cd eye-tracking-HCI
   ```

2. **Install Python dependencies:**
   ```bash
   pip install opencv-python mediapipe numpy pyautogui pillow python-vlc
   ```

3. **Run the application:**
   ```bash
   python test.py
   ```

---

## 📦 Building a Standalone Executable

The project includes a `test.spec` file for packaging with **PyInstaller**.

```bash
pip install pyinstaller
pyinstaller test.spec
```

The compiled executable will be found in the `dist/` folder.

> **Note:** `dist/` and `build/` directories are excluded from version control via `.gitignore`.

---

## 🖥️ Usage Guide

1. Launch the application (`python test.py`).
2. Click **"Add Videos"** to load one or more video files.
3. The video will start playing automatically in the player panel.
4. **Look at the screen** — your gaze will move the mouse cursor.
5. **Blink** to click on UI elements (play/pause, skip, etc.).
6. Use the on-screen buttons to manually control playback as needed.

### Playback Controls

| Button         | Action                          |
|----------------|---------------------------------|
| ⏮ Previous    | Go to previous video in playlist|
| ⏪ Back 10s   | Skip backward 10 seconds        |
| ▶️ Play/Pause  | Toggle play and pause           |
| ⏩ Forward 10s | Skip forward 10 seconds         |
| ⏭ Next        | Go to next video in playlist    |

---

## 🔧 Configuration

You can adjust these constants in `test.py` to tune the behavior:

| Variable          | Default | Description                                        |
|-------------------|---------|----------------------------------------------------|
| `blink_threshold` | `0.20`  | EAR value below which a blink is detected          |
| `smooth_factor`   | `0.2`   | Cursor smoothing (lower = smoother, higher = faster)|

---

## 📋 Requirements Summary

```
opencv-python
mediapipe
numpy
pyautogui
Pillow
python-vlc
tkinter        # Included with Python standard library
```

---

## 📄 Documentation

The full project report and phase documentation are available in:

```
HCI PROJECT/Eye-Controlled_Cursor_Phase1 (2).pdf
```

---

## ⚠️ Known Limitations

- Requires good **lighting conditions** for accurate eye tracking.
- Performance may vary depending on webcam quality and hardware.
- VLC must be installed separately — `python-vlc` is a wrapper around the VLC library.
- Currently supports **one face** at a time (`max_num_faces=1`).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to open an issue or submit a pull request.

---

## 📜 License

This project is developed for academic/educational purposes as part of an HCI course. Please refer to your institution's academic integrity policies before reusing.

---

*Built with ❤️ using Python, MediaPipe, and OpenCV.*
