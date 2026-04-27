


![demo](https://github.com/user-attachments/assets/dbe84db7-262d-4fed-a8d9-848db047eaaa)

# ESP32 Video Player

A high-performance video player for SSD1306 OLED displays. It plays monochrome video at 128x64 resolution at a steady 30 FPS.

## Hardware

- **MCU**: ESP32-S3 DevKit
- **Display**: SSD1306 128x64 OLED (I2C)
- **Pins**: SDA=GPIO4, SCL=GPIO5, Address=0x3C
- **Memory**: 16MB Flash + PSRAM

## Project Structure

- `src/main.cpp` - Video oynatıcı firmware
- `compress.py` - MP4 → binary dönüştürücü
- `data/video.bin` - Video dosyası (SPIFFS'te)

```text
esp32-video-player-SSD1306/
├── .git/                          # Git repository
├── .pio/                          # PlatformIO build files
├── .vscode/                       # VS Code settings
├── src/
│   ├── main.cpp                   # Video player firmware
│   └── CMakeLists.txt
├── data/
│   ├── video.bin                  # Binary video file
│   └── video1.bin
├── videos/
│   └── test.mp4                   # Source MP4 video
├── include/                       # Header files
├── lib/                           # Libraries
├── test/                          # Test files
├── CMakeLists.txt
├── compress.py                    # MP4 → binary converter
├── platformio.ini                 # PlatformIO config
├── README.md                      # Project docs
├── .gitignore
└── sdkconfig.esp32-s3-devkitc-1-n16r8v
```

## Setup 
### 1. Prepare the Video

Place your MP4 file into the videos/ folder, then run the compressor:

```bash
python compress.py
```
This generates `data/video.bin` optimized for the OLED's memory layout.

### 2.Upload to ESP32

Flash the firmware and the filesystem (SPIFFS) using PlatformIO:
```bash
python -m platformio run --target uploadfs
```
or
you can upload with one click from  --> Platformio Tab/(your board name)/Platforms/Upload Filesystem Image 
<img width="304" height="575" alt="image" src="https://github.com/user-attachments/assets/707c4d4e-caa2-4e63-adf1-0545677aeb43" />



## Features

- Efficient Encoding: 1024 bytes per frame (128×64 bits).
- High Performance: 30 FPS smooth playback.
- Auto-Loop: Automatically restarts the video from SPIFFS.
- Serial Debugging: Real-time status updates via Serial.

## Troubleshooting With Project

| Issue                               | Solution                                                                      |
| ----------------------------------  | ----------------------------------------------------------------------------- |
| "Video looping..." but screen black | before upload your code run --> `python -m platformio run --target uploadfs`  |
| "video.bin not found!"              | Ensure video.bin exists in the data/ folder before uploading FS.              |

## Troubleshooting With PlatformIO

| Issue                               | Solution                                                                      |
| ----------------------------------  | ----------------------------------------------------------------------------- |
| "ESP32-S3-N16R8" not in board list  | You have to add custom board --> <a href="https://community.platformio.org/t/how-do-you-set-up-a-new-board-esp32-s3-n16r8/32306/4">How to do</a>  |




## Configuration

**Firmware Settings** (`src/main.cpp`):

```cpp
#define TARGET_FPS 30               // Desired Frame Rate
#define I2C_SDA 4                   // SDA Pin 
#define I2C_SCL 5                   // SCL Pin
```

**Converter** (`compress.py`):

```python
video_path = Path("./videos/test.mp4")
output_path = Path("./data/video.bin")
```
##Credits
based on <a href="https://github.com/younes-makhchan/ESP32_Video_Display/tree/master">ESP32_Video_Display</a> and  <a href="https://github.com/hackffm/ESP32_BadApple">ESP32_Video_Display</a>
