# Video Event Annotator

A lightweight Tkinter-based video annotation tool for frame-level event labeling.

The tool allows an annotator to:

- Browse multiple videos
- Jump directly to frames
- Navigate using metadata markers
- Create frame-range events
- Edit and delete events
- Save annotations automatically into per-video JSON files

---

# Features

## Video Navigation

- Next / Previous video
- Direct frame jump
- Frame stepping:
  - ±1 frame
  - ±10 frames
  - ±100 frames
- Keyboard shortcuts

## Metadata Markers

The tool can load marker metadata from a separate JSON file.

Markers are displayed in a side panel.

Double-clicking a marker automatically jumps to the corresponding frame.

## Event Annotation

Each event consists of:

- Event ID
- Label
- Start Frame
- End Frame

Supported operations:

- Add Event
- Update Existing Event
- Delete Event
- Double-click an event to revisit its start frame

## Automatic Persistence

Each video stores its annotations in an individual JSON file.

Example:

```text
1A.mp4
1A.json

1B.mp4
1B.json

2A.mp4
2A.json
```

No manual saving is required.

Changes are written immediately after adding, updating, or deleting events.

---

# Folder Structure

Example:

```text
workspace/
├── videos/
│   ├── 1A.mp4
│   ├── 1A.json
│   ├── 1B.mp4
│   ├── 1B.json
│   ├── 2A.mp4
│   └── ...
│
└── 00sequenceresult.json
```

The metadata file (`00sequenceresult.json`) contains marker information used by the annotator.

---

# Event JSON Format

Example:

```json
{
  "video": "1A",
  "events": [
    {
      "id": "evt_001",
      "label": "A",
      "start_frame": 301,
      "end_frame": 325
    },
    {
      "id": "evt_002",
      "label": "C",
      "start_frame": 540,
      "end_frame": 612
    }
  ]
}
```

---

# Annotation Workflow

## 1. Open a Video

Use:

```text
Prev Video
Next Video
```

to switch between videos.

---

## 2. Navigate to a Candidate Location

You can either:

- Enter a frame number directly and click `Go`
- Double-click a metadata marker

The viewer will jump to the corresponding frame.

---

## 3. Locate Event Boundaries

Use frame stepping buttons or keyboard shortcuts to identify the precise start and end frames.

Available frame steps:

```text
±1
±10
±100
```

---

## 4. Capture Event Range

When the start frame is identified:

```text
Use Current As Start
```

When the end frame is identified:

```text
Use Current As End
```

---

## 5. Create Event

Fill:

```text
Label
Start Frame
End Frame
```

Then click:

```text
Add Event
```

The event will immediately appear in the event table and be saved to disk.

---

## 6. Edit Existing Event

Double-click an event in the event table.

The editor fields will automatically populate with the event information.

Modify the values and click:

```text
Update Selected Event
```

Changes are saved automatically.

---

## 7. Delete Event

Select an event in the event table.

Click:

```text
Delete Selected Event
```

The event is removed immediately and the JSON file is updated automatically.

---

# Keyboard Shortcuts

## Video Navigation

```text
n    Next Video
p    Previous Video
```

## Frame Navigation

```text
Left Arrow     -1 frame
Right Arrow    +1 frame
```

```text
Shift + Left   -10 frames
Shift + Right  +10 frames
```

### macOS / Remote Server Configuration

On some macOS systems:

```text
Control + Left
Control + Right
```

may be intercepted by the operating system.

If configured accordingly, use:

```text
Control + Shift + Left   -100 frames
Control + Shift + Right  +100 frames
```

instead.

---

# Dependencies

Required packages:

```bash
pip install pillow
pip install imageio
pip install opencv-python
```

Python version:

```text
Python 3.8+
```

---

# Metadata Format

Marker information is loaded from:

```text
00sequenceresult.json
```

The application expects a metadata entry whose key matches the video filename stem.

Example:

```json
{
  "1A": {
    "timesec": "A(0:39)B(1:12)C(2:05)"
  }
}
```

Markers are converted into frame indices using:

```text
frame = seconds × fps
```

Current default:

```text
fps = 7
```

---

# Notes

- Frame indexing is 1-based.
- Event IDs are generated automatically.
- Event files are saved per-video.
- Event changes are autosaved.
- Double-clicking a marker jumps to the marker frame.
- Double-clicking an event jumps to the event start frame.
- Switching videos automatically loads that video's event annotations.
- The zoom slider changes display size only and does not affect stored frame numbers.

---

# Known Limitations

- The tool assumes a constant frame rate when converting metadata timestamps to frames.
- Metadata loading currently uses a fixed file path defined in `_load_metadata()`.
- Event validation (e.g., overlapping events or end frame before start frame) is not currently enforced.
- Large videos may take longer to seek depending on the backend (OpenCV or imageio).

---

# Features

- Video browsing
- Metadata marker navigation
- Frame stepping
- Event creation
- Event editing
- Event deletion
- Automatic JSON persistence