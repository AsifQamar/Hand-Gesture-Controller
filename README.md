# Gesture Control for Media 🖐️

This Python script allows you to control media playback on your computer using hand gestures captured through your webcam. By simply showing a certain number of fingers, you can play/pause, skip tracks, and adjust the volume.



## How It Works

The script uses **OpenCV** to access the webcam feed and **MediaPipe** to perform real-time hand tracking. It identifies the landmarks on your hand and calculates the number of fingers you are holding up. This count is then mapped to specific keyboard commands, which are executed using **PyAutoGUI**.

A simple debounce timer is included to prevent a single gesture from triggering an action multiple times in rapid succession.

***

## Features
- **Real-time Gesture Recognition**: Instantly detects hand gestures from your webcam.
- **Intuitive Controls**: Uses the number of extended fingers to control media.
- **Cross-platform**: Works on any operating system with Python and a webcam.
- **Customizable**: Easily change the key presses to control different applications.

***

## Gestures
The following gestures are mapped to media control keys:

| Fingers | Action | Key Press |
| :---: | :--- | :---: |
| **1** | Next | `right arrow` |
| **2** | Play / Pause | `k` |
| **3** | Volume Up | `up arrow` |
| **4** | Previous | `left arrow` |
| **5** | Volume Down | `down arrow` |

*Note: The 'k' key is commonly used for play/pause in web-based video players like YouTube.*

***

## Installation

1.  **Clone the repository or download the script.**

2.  **Install the required Python libraries:**
    Make sure you have Python 3 installed. Then, run the following command in your terminal:
    ```bash
    pip install opencv-python mediapipe pyautogui
    ```

***

## Usage

1.  Run the script from your terminal:
    ```bash
    python your_script_name.py
    ```
2.  A window will open showing your webcam feed with the hand landmarks overlaid.
3.  Make sure your target application (e.g., YouTube in a browser, VLC, Spotify) is the **active window**.
4.  Show one of the gestures to the camera to control the media.

To stop the script, press the **`Esc`** key.

***

## Code Overview

- **Initialization**:
    - `cv2.VideoCapture(0)`: Starts capturing video from the default webcam.
    - `mp.solutions.hands.Hands()`: Initializes the MediaPipe Hands model for detection.

- **`count_fingers(lst)` function**:
    - This is the core logic for gesture recognition.
    - It calculates a dynamic threshold based on the distance between the wrist and the middle finger's knuckle to adapt to different hand sizes and distances from the camera.
    - It checks if the tip of each finger is sufficiently far from its base to be considered "up".
    - The thumb is handled separately by checking its horizontal position relative to the index finger.

- **Main Loop**:
    - Reads each frame from the webcam.
    - Processes the frame with the MediaPipe model to find hand landmarks.
    - If a hand is detected, it calls `count_fingers()` to determine the gesture.
    - A simple timer (`start_init`, `end_time - start_time`) ensures that a command is sent only once every `0.5` seconds to prevent spamming keys.
    - `pyautogui.press()` sends the corresponding keystroke to the operating system.
