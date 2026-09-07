# Trinetra V1 — Visual Aircraft Tracking

Hackathon MVP:
- YOLO aircraft detection
- ByteTrack tracking via Ultralytics
- target selection
- Kalman-style constant-velocity prediction
- occlusion state
- simple appearance matching for reacquisition
- live OpenCV HUD

## 1. Requirements

Python 3.10+ recommended.

Create a virtual environment:

### macOS / Linux
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Windows
```powershell
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## 2. Run V1

Use a webcam:
```bash
python main.py --source 0
```

Use a video:
```bash
python main.py --source path/to/video.mp4
```

The default detector is `yolo11n.pt`. On first run, Ultralytics downloads the model.

## 3. Important hackathon note

The stock YOLO model is not guaranteed to detect your custom cardboard jets reliably.

For the FIRST integration test, use:
- any object/video YOLO can detect, OR
- download/use a custom aircraft model later.

The pipeline itself is ready for a custom model:
```bash
python main.py --source 0 --model path/to/best.pt
```

## Controls

- `q` — quit
- `t` — select the currently largest tracked aircraft as target
- `r` — reset target
- `o` — toggle demo occlusion mode
- `p` — print current target state

## Demo flow

1. Start with two visible aircraft.
2. Press `t` while the desired target is visible.
3. Move/hide the target behind an obstacle.
4. Trinetra switches to OCCLUDED and predicts its position.
5. Bring the aircraft back.
6. The system attempts to reacquire it using position + appearance similarity.

## Architecture

Camera/video -> YOLO -> ByteTrack -> Target Manager -> Prediction/Re-ID -> HUD

Blender and phone streams can later replace `source` without changing the core tracking modules.
