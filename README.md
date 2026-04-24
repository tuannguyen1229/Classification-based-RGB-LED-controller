# Hand Gesture Controlled RGB LED System using TinyML

<div align="center">

![Project Status](https://img.shields.io/badge/Status-Active-success)
![Platform](https://img.shields.io/badge/Platform-ESP32--CAM-blue)
![ML Framework](https://img.shields.io/badge/ML-TinyML-orange)
![Language](https://img.shields.io/badge/Language-C%2B%2B-purple)

*A touchless RGB LED control system powered by on-device machine learning*

[Features](#features) • [Hardware](#hardware-components) • [System Architecture](#system-architecture) • [AI Model](#ai-model-development) • [Use Cases](#use-cases) • [Setup](#installation--setup)

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Hardware Components](#hardware-components)
- [System Architecture](#system-architecture)
- [Use Cases](#use-cases)
- [AI Model Development](#ai-model-development)
- [Installation & Setup](#installation--setup)
- [Demo](#demo)
- [Future Improvements](#future-improvements)
- [Contributors](#contributors)
- [License](#license)

---

## Overview

This project implements a **touchless RGB LED control system** using hand gesture recognition powered by **TinyML** (Tiny Machine Learning) on the **ESP32-CAM** platform. The system recognizes hand gestures in real-time through the onboard OV2640 camera and controls WS2812 RGB LED accordingly without physical contact.

### Key Highlights

- **Real-time gesture recognition** using classification-based ML model
- **Edge computing** - AI inference runs directly on ESP32 (no cloud needed)
- **Low latency** - Instant response with on-device processing
- **Touchless interface** - Hygienic and modern interaction method
- **100% accuracy** - Validated model performance on test dataset

---

##  Features

### **Three Main Functions**

1. **Build hand gesture classification model** 
   - Machine learning model to recognize different hand gestures
   
2. **Distinguish simple hand gestures**
   - Real-time classification of basic hand poses
   
3. **Control RGB LED based on gestures**
   - Map each gesture to specific LED actions

### 🔧 **System Capabilities**

- **Real-time image capture** from OV2640 camera
- **On-device ML inference** using TinyML classification model
- **RGB LED control** corresponding to detected gestures
- **Touchless operation** - No physical contact required
- **Instant response** - Direct control without network latency

---

## Hardware Components

### **1. AI-Thinker ESP32-CAM**

<img src="https://nshopvn.com/wp-content/uploads/2021/11/esp32-can-labeled.jpeg" width="300">

- **Microcontroller**: ESP32 with integrated OV2640 camera
- **Key Features**:
  - Built-in camera for image capture
  - WiFi connectivity for data transmission
  - On-board image processing capability
  - MicroSD card support for data storage
  - Compact size suitable for embedded AI applications
- **Function**: Captures hand gesture images and runs TinyML inference

### **2. WS2812 RGB LED Module**

<img src="https://bizweb.dktcdn.net/100/522/662/products/rgb-led-module.png?v=1723030453110" width="300">

- **Features**:
  - Chainable - supports multiple LEDs
  - Individual LED control
- **Operating Voltage**: 5V DC
- **Function**: Visual output controlled by gesture recognition

---

## System Architecture

```
┌────────────────────────────────────────────────────┐
│                                                    │
│                       PC                           │
│          (Training & Debugging)                    │
│                                                    │
└──────────────────┬─────────────────────────────────┘
                   │
                   │ USB-Serial
                   │
                   ▼
┌──────────────────────────────────┐
│                                  │
│      AI-Thinker ESP32-CAM        │
│   (TinyML Inference Engine)      │
│                                  │
└──────────────┬───────────────────┘
               │
               │ GPIO Data Pin
               │
               ▼
┌──────────────────────────────────┐
│                                  │
│       WS2812 RGB LED             │
│      (Visual Output)             │
│                                  │
└──────────────────────────────────┘
```

### Workflow

1. **Camera Capture**: ESP32-CAM captures hand gesture image (96x96 grayscale)
2. **Preprocessing**: Image converted to model input format
3. **Inference**: TinyML model classifies the gesture
4. **Action**: Corresponding LED control command executed
5. **Output**: WS2812 LED responds with color/brightness change

---

## Use Cases

### **UC-01: Turn LED ON/OFF** 

**Description**: Control LED power state using hand gestures

| Gesture | Action | Result |
|---------|--------|--------|
| `palm_down` (hand facing down) <br> <img src="https://github.com/tuannguyen1229/Classification-based-RGB-LED-controller/blob/main/docs/palm_down.jpg?raw=true" width="100"> | Turn ON | LED RGB lights up|
| `fist` (closed fist) <br> <img src="https://github.com/tuannguyen1229/Classification-based-RGB-LED-controller/blob/main/docs/fist.jpg?raw=true" width="100"> | Turn OFF | LED RGB turns off |

---

### **UC-02: Change LED Color** 

**Description**: Cycle through different colors

| Gesture | Action | Color Sequence |
|---------|--------|----------------|
| `palm_up` (hand facing up) <br> <img src="https://github.com/tuannguyen1229/Classification-based-RGB-LED-controller/blob/main/docs/palm_up.jpg?raw=true" width="100"> | Change color | RED → YELLOW → BLUE → RED... |

**Note**: Colors cycle continuously in order

---

### **UC-03: Adjust LED Brightness** 

**Description**: Control LED brightness levels

| Gesture | Action | Brightness Levels |
|---------|--------|-------------------|
| `finger_L` (thumb + index forming "L") <br> <img src="https://github.com/tuannguyen1229/Classification-based-RGB-LED-controller/blob/main/docs/finger_L.jpg?raw=true" width="100">  | Change brightness | 3 progressive brightness steps |

**Note**: System supports 3 incrementing brightness levels

---

## AI Model Development

### **Step 1: Data Collection & Labeling**

```python
# Data Collection Parameters
- Image Source: ESP32-CAM / Smartphone
- Images per Gesture: 100-200 samples
- Gesture Classes: 4
  ├── finger_L
  ├── fist
  ├── palm_down
  └── palm_up
```
### **Step 2: Data Preprocessing**

```yaml
Image Preprocessing:
  - Input Size: 96x96 pixels
  - Color Mode: Grayscale
  - Purpose: Optimize for ESP32-CAM processing
  - Feature Extraction: Automatic
```

**Benefits of Grayscale**:
- Reduces memory footprint
- Faster processing on embedded device
- Sufficient for gesture recognition

---

### **Step 3: Pipeline Creation**

```
Data Flow:
  Raw Image → Resize (96x96) → Image Block → Transfer Learning → Classification
```

**Components**:
- **Image Block**: Extracts image features
- **Transfer Learning Block**: Uses pre-trained model for classification

---

### **Step 4: Model Training**

**Training Configuration**:
```yaml
Framework: Edge Impulse
Architecture: Transfer Learning (MobileNetV2)
Training Parameters:
  - Epochs: Auto-tuned
  - Learning Rate: Optimized
  - Validation Split: 20%
```

**Training Results**:

| Metric | Value |
|--------|-------|
| Accuracy | **100.0%** |
| Loss | **0.02** |
| F1 Score | **1.00** (all classes) |

**Confusion Matrix**:

|  | finger_L | fist | none | palm_down | palm_up |
|--|----------|------|------|-----------|---------|
| **finger_L** | 100% | 0% | 0% | 0% | 0% |
| **fist** | 0% | 100% | 0% | 0% | 0% |
| **none** | 0% | 0% | 100% | 0% | 0% |
| **palm_down** | 0% | 0% | 0% | 100% | 0% |
| **palm_up** | 0% | 0% | 0% | 0% | 100% |

---

### **Step 5: Model Deployment to ESP32-CAM**

**Deployment Process**:

1. **Export Model**: Edge Impulse exports as Arduino library (.zip)
   - Contains C++ code
   - Includes .tflite model file
   
2. **Import to Arduino IDE**: Add library via Arduino IDE

3. **Flash to ESP32-CAM**: Upload code to device

4. **Run Inference**: Model executes on-device

**Deployment Benefits**:
- No internet connection required
- Privacy-preserving (data stays on device)
- Minimal latency (<100ms inference time)

---

## Installation & Setup

### **Prerequisites**

```bash
Hardware:
  - AI-Thinker ESP32-CAM
  - WS2812 RGB LED Module
  - FTDI USB-to-Serial Programmer (for ESP32-CAM)
  - Jumper wires
  - 5V Power supply

Software:
  - Arduino IDE 1.8.x or 2.x
  - ESP32 Board Support Package
  - Edge Impulse account (for model training)
```

### **Step 1: Hardware Connection**

```
ESP32-CAM Pinout:
  ├── 5V    → Power Supply (+5V)
  ├── GND   → Power Supply (GND) & WS2812 (GND)
  ├── GPIO2 → WS2812 (Data In)
  └── U0R/U0T → FTDI (for programming)

WS2812 Connection:
  ├── VCC → 5V
  ├── GND → GND
  └── DIN → ESP32 GPIO2
```

### **Step 2: Software Setup**

```bash
# 1. Install Arduino IDE
# Download from: https://www.arduino.cc/en/software

# 2. Add ESP32 Board Support
# In Arduino IDE:
# File → Preferences → Additional Board Manager URLs:
# https://dl.espressif.com/dl/package_esp32_index.json

# 3. Install ESP32 Board Package
# Tools → Board → Boards Manager → Search "ESP32" → Install

# 4. Install Required Libraries
# Sketch → Include Library → Manage Libraries:
#   - Adafruit NeoPixel (for WS2812)
#   - [Your Project Name]_inferencing (exported from Edge Impulse)
```

### **Step 3: Upload Code**

```cpp
#include <hand_gesture_1_inferencing.h>
#include "edge-impulse-sdk/dsp/image/image.hpp"

#include "esp_camera.h"
#include <String.h>
// Select camera model - find more camera models in camera_pins.h file here
// https://github.com/espressif/arduino-esp32/blob/master/libraries/ESP32/examples/Camera/CameraWebServer/camera_pins.h

// #define CAMERA_MODEL_ESP_EYE // Has PSRAM
#define CAMERA_MODEL_AI_THINKER // Has PSRAM

#if defined(CAMERA_MODEL_ESP_EYE)
#define PWDN_GPIO_NUM    -1
#define RESET_GPIO_NUM   -1
#define XCLK_GPIO_NUM    4
#define SIOD_GPIO_NUM    18
#define SIOC_GPIO_NUM    23
.....
.....
.....
static int ei_camera_get_data(size_t offset, size_t length, float *out_ptr)
{
    // we already have a RGB888 buffer, so recalculate offset into pixel index
    size_t pixel_ix = offset * 3;
    size_t pixels_left = length;
    size_t out_ptr_ix = 0;

    while (pixels_left != 0) {
        // Swap BGR to RGB here
        // due to https://github.com/espressif/esp32-camera/issues/379
        out_ptr[out_ptr_ix] = (snapshot_buf[pixel_ix + 2] << 16) + (snapshot_buf[pixel_ix + 1] << 8) + snapshot_buf[pixel_ix];

        // go to the next pixel
        out_ptr_ix++;
        pixel_ix+=3;
        pixels_left--;
    }
    // and done!
    return 0;
}

#if !defined(EI_CLASSIFIER_SENSOR) || EI_CLASSIFIER_SENSOR != EI_CLASSIFIER_SENSOR_CAMERA
#error "Invalid model for current sensor"
#endif
```

**Upload Settings**:
```
Board: "AI Thinker ESP32-CAM"
Upload Speed: "115200"
Flash Frequency: "80MHz"
Partition Scheme: "Huge APP (3MB No OTA)"
```

---

## Demo

### **Gesture Recognition in Action**

| Gesture | LED Response |
|---------|--------------|
| palm_down | <img src="docs/demo/palm_down_demo.gif" width="300"> |
| fist | <img src="docs/demo/fist_demo.gif" width="300"> |
| palm_up | <img src="docs/demo/palm_up_demo.gif" width="300"> |
| finger_L | <img src="docs/demo/finger_L_demo.gif" width="300"> |

### **Demo Video**

Video: [Google Drive](https://drive.google.com/file/d/1akqUHzZEkO-JHRlOaAyYGQA5LtOoS_r4/view?usp=sharing)

---

## 📊 Performance Metrics

| Metric | Value |
|--------|-------|
| Inference Time | ~85ms |
| Model Size | ~250KB |
| RAM Usage | ~100KB |
| Accuracy | 100% (validation set) |
| Frame Rate | ~10 FPS |
| Power Consumption | ~300mA @ 5V |

---

## Future Improvements

- [ ] Add more gesture classes (10+ gestures)
- [ ] Implement gesture sequences for complex commands
- [ ] Add MQTT support for IoT integration
- [ ] Web interface for configuration
- [ ] Battery power optimization
- [ ] Multi-LED array control
- [ ] Voice feedback integration
- [ ] Mobile app control

---

## Contributors

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/your-username">
        <img src="https://github.com/your-username.png" width="100px;" alt=""/>
        <br />
        <sub><b>Tuan Nguyen</b></sub>
      </a>
      <br />
      <sub>IoT Engineer</sub>
    </td>
  </tr>
</table>

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- [Edge Impulse](https://www.edgeimpulse.com/) for TinyML platform
- [Espressif](https://www.espressif.com/) for ESP32 platform
- [Adafruit](https://www.adafruit.com/) for NeoPixel library

---

## Contact

**Tuan Nguyen**
- GitHub: [@your-username](https://github.com/your-username)
- Email: your.email@example.com
- LinkedIn: [Your LinkedIn](https://linkedin.com/in/your-profile)

---

<div align="center">

**⭐ Star this repo if you find it helpful!**

Made with ❤️ and TinyML

</div>
