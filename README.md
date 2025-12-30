![Human Benchmark Aim Test Demo](aim-demo.png)

# Human Benchmark Aim Bot

This is a small Python script that automatically plays the **Human Benchmark Aim Trainer** test:  
<https://humanbenchmark.com/tests/aim>

It uses screen capture and template matching to locate the red target on your screen and click it as fast as possible.

On my setup it averages around **225 ms** per target.

---

## How it works

- Uses `mss` to grab live screenshots of a screen region
- Converts the screenshot to grayscale
- Uses OpenCV template matching to find the target based on a reference image (`target.png`)
- Caches the best scale of the template image to speed up repeated detections
- Uses `pyautogui` to click the center of the detected target
- Repeats until it has clicked a fixed number of targets (default 30)

Core logic:

- `visionClass` handles:
  - Screen capture
  - Loading the template image
  - Scanning a region of the screen for the target
  - Returning the center coordinates for `pyautogui` to click

In `__main__`, the script:

1. Creates a `visionClass` instance with a confidence threshold (default `0.8`)
2. Loads `target.png` in grayscale
3. Loops:
   - Scans the configured region around the Aim Trainer area
   - If a match is found, clicks the target and increments a counter
   - Stops after 30 successful clicks or if an error occurs

---

## Requirements

- Python 3.8 or later
- A desktop environment (not headless)

Python packages:

- `opencv-python`
- `numpy`
- `mss`
- `pyautogui`
- `imutils`

Install dependencies:

```bash
pip install opencv-python numpy mss pyautogui imutils
````
---

## Setup for Human Benchmark

1. **Get a reference target image**

   * Open [https://humanbenchmark.com/tests/aim](https://humanbenchmark.com/tests/aim)
   * Start the test so the red target appears
   * Screenshot and crop the target
   * Save it as `target.png`

2. **Update the path**

   ```python
   img = vision.load_asset_grayscale("target.png")
   ```

3. **Adjust the scan region**

   ```python
   res = vision.scan_region(img, (350, 155, 1050, 549))
   ```

---

## Running the bot

```bash
python vision_controller.py
```

Open the Aim Trainer tab -- https://humanbenchmark.com/tests/aim
Press `Ctrl+C` to stop early.

---

## Performance

Average result on my setup: **~225 ms per target**
Your results may vary depending on resolution, scaling, and hardware.

---

## Disclaimer

This project is for learning and computer-vision experimentation.
Using automation on online tests may violate site rules — use responsibly.
