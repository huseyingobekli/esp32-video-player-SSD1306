# ESP32 Video Player

SSD1306 OLED ekranda video oynatıcı. 128x64 çözünürlükte 30 FPS'te siyah-beyaz video oynatır.

## Donanım

- **MCU**: ESP32-S3 DevKit
- **Display**: SSD1306 128x64 OLED (I2C)
- **Pinler**: SDA=GPIO4, SCL=GPIO5, Adres=0x3C
- **Bellek**: 16MB Flash + PSRAM

## Dosyalar

- `src/main.cpp` - Video oynatıcı firmware
- `compress.py` - MP4 → binary dönüştürücü
- `data/video.bin` - Video dosyası (SPIFFS'te)

## Kurulum

### 1. Bağımlılıkları Yükle

```bash
pip install opencv-python numpy
```

### 2. Video Hazırla

MP4 dosyasını `videos/` klasörüne koy, sonra:

```bash
python compress.py
```

Bu `data/video.bin` oluşturur.

### 3. Yükle

```bash
platformio run --target upload --upload-port COM5
platformio run --target uploadfs --upload-port COM5
```

### 4. Kontrol Et

```bash
platformio device monitor --port COM5 --baud 115200
```

## Özellikler

- 1024 byte/frame (128×64 bit)
- 30 FPS playback
- SPIFFS'ten okur, otomatik loop
- Seri port debug çıkışı

## Sorun Gider

| Sorun                              | Çözüm                                       |
| ---------------------------------- | ------------------------------------------- |
| "Video looping..." ama görüntü yok | `platformio run --target uploadfs` çalıştır |
| "video.bin not found!"             | video.bin dosyası SPIFFS'e yükle            |
| Ekran boş                          | I2C kablolarını kontrol et (SDA=4, SCL=5)   |
| Python hata                        | `pip install opencv-python numpy`           |

## Ayarlar

**Video oyuncusu** (`src/main.cpp`):

```cpp
#define TARGET_FPS 30              // Frame rate
#define I2C_SDA 4                  // SDA pin
#define I2C_SCL 5                  // SCL pin
```

**Dönüştürücü** (`compress.py`):

```python
video_path = Path("./videos/test.mp4")
output_path = Path("./data/video.bin")
```
