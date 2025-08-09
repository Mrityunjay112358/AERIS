# Voice Analysis System with ESP32 Integration

A complete voice analysis system that identifies speakers and emotions, then sends the results to an ESP32 device via TCP for display and haptic feedback.

Open the zipped file 'Mrutnjay-main (1).zip' and extract its contents before proceeding.

## 🎯 Features

### Python Voice Analysis System
- **Speaker Identification**: Train models to recognize different speakers
- **Emotion Detection**: Analyze emotional states from voice (Happy, Sad, Angry, Calm, Excited, Neutral)
- **GUI Application**: User-friendly interface with tabs for training, recording, analysis
- **TCP Communication**: Send results to ESP32 devices
- **CLI Tools**: Command-line interface for quick testing

### ESP32 Display System
- **OLED Display**: Shows person names, emotions, and emoji
- **Haptic Feedback**: Different frequencies for persons and emotions
- **LED Indicators**: Visual feedback for person identification
- **WiFi Connectivity**: Receives data via TCP

## 📋 Requirements

### Python Dependencies
```bash
pip install -r requirements.txt
```

### Arduino Libraries (for ESP32)
- WiFi (built-in)
- ArduinoJson
- Wire (built-in)
- Adafruit_GFX
- Adafruit_SSD1306

## 🔧 Hardware Setup

### ESP32 Connections
```
OLED Display (SSD1306):
- VCC → 3.3V
- GND → GND  
- SDA → GPIO 6
- SCL → GPIO 7

Haptic Motor/Buzzer:
- Positive → GPIO 13
- Negative → GND

LEDs (Optional):
- Person 1 LED → GPIO 2
- Person 2 LED → GPIO 4
- Person 3 LED → GPIO 5
- Emotion LED → GPIO 18
```

## ⚙️ Setup Instructions

### 1. Python Environment Setup
```bash
# Create virtual environment
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# Install dependencies
pip install -r requirements.txt
```

### 2. ESP32 Setup
1. Open `ESP32_Voice_TCP_Server.ino` in Arduino IDE
2. Update WiFi credentials:
   ```cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   ```
3. Install required libraries via Library Manager
4. Upload to ESP32
5. Note the IP address shown in Serial Monitor

### 3. Configure Python Application
Update ESP32 IP address in the Python code or use command-line parameters.

## 🚀 Usage

### GUI Application
```bash
python voice_app_gui.py
```

**Tabs:**
1. **Training**: Train speaker recognition models
2. **Voice Recording**: Collect voice samples
3. **Identification**: Identify speakers from audio files
4. **Mood Analysis**: Analyze emotions and perform complete analysis
5. **ESP32 Control**: Configure TCP communication

### Command Line Tools

#### Test ESP32 Connection
```bash
python esp32_cli.py --ip 192.168.1.100 test
```

#### Send Manual Codes
```bash
# Send Person 1, Happy emotion
python esp32_cli.py --ip 192.168.1.100 send 1 1

# Send Person 2, Excited emotion  
python esp32_cli.py --ip 192.168.1.100 send 2 5
```

#### Analyze Audio and Send to ESP32
```bash
python esp32_cli.py --ip 192.168.1.100 analyze recording.wav
```

#### Complete Test Suite
```bash
python test_esp32_communication.py
```

## 📊 Code Mappings

### Person Codes
- 0 = Unknown/Default
- 1 = Person 1
- 2 = Person 2  
- 3 = Person 3

### Emotion Codes
- 0 = Neutral
- 1 = Happy
- 2 = Sad
- 3 = Angry
- 4 = Calm
- 5 = Excited

## 📡 Data Formats

### Simple Format
```
P:1,E:2
```
Person code 1, Emotion code 2

### JSON Format
```json
{
  "person": 1,
  "emotion": 2, 
  "confidence_person": 85.0,
  "confidence_emotion": 92.0,
  "timestamp": 1642781234
}
```

## Training Data Structure

Organize your training data in the following folder structure:

```
training_data/
├── person1/
│   ├── sample1.wav
│   ├── sample2.wav
│   └── sample3.wav
├── person2/
│   ├── sample1.wav
│   ├── sample2.wav
│   └── sample3.wav
└── person3/
    ├── sample1.wav
    ├── sample2.wav
    └── sample3.wav
```

## 📁 Project Structure

```
test_voice/
├── voice_app_gui.py              # Main GUI application
├── voice_id.py                   # Voice identification engine
├── voice_mood_simple.py          # Emotion analysis engine
├── voice_recorder.py             # Audio recording utilities
├── tcp_client.py                 # TCP communication client
├── esp32_cli.py                  # Command-line interface
├── test_esp32_communication.py   # ESP32 communication test
├── ESP32_Voice_TCP_Server.ino    # ESP32 Arduino sketch
├── requirements.txt              # Python dependencies
├── models/                       # Trained model storage
├── training_data/                # Voice training samples
│   ├── person1/                  # Person 1 voice samples
│   ├── person2/                  # Person 2 voice samples
│   └── ...
└── README.md                     # This file
```

## 🎵 Training Voice Models

1. **Collect Samples**: Use "Voice Recording" tab to collect 5 samples per person
2. **Train Model**: Use "Training" tab to train the recognition model
3. **Save Model**: Save trained models for later use
4. **Test**: Use "Identification" or "Mood Analysis" tabs to test

## 🔊 ESP32 Haptic Feedback

Different frequencies are used for identification:

**Person Frequencies:**
- Unknown: 50Hz
- Person 1: 100Hz
- Person 2: 200Hz
- Person 3: 300Hz

**Emotion Frequencies:**
- Neutral: 150Hz
- Happy: 250Hz
- Sad: 100Hz
- Angry: 400Hz
- Calm: 180Hz
- Excited: 350Hz

## Features Extracted

The system extracts the following voice features:

1. **MFCC (Mel-frequency cepstral coefficients)**: 13 coefficients + statistics
2. **Chroma features**: Pitch class profiles
3. **Spectral contrast**: Difference between peaks and valleys in spectrum
4. **Zero crossing rate**: Rate of sign changes in the signal
5. **Spectral rolloff**: Frequency below which 85% of energy is contained
6. **Spectral centroid**: Center of mass of spectrum
7. **Tempo**: Estimated tempo of the audio

## 🐛 Troubleshooting

### ESP32 Connection Issues
1. Check WiFi credentials
2. Verify ESP32 IP address
3. Ensure port 8080 is not blocked
4. Check serial monitor for error messages

### Voice Analysis Issues
1. Ensure audio files are in supported formats (.wav, .mp3, .flac, .m4a)
2. Check microphone permissions
3. Verify training data has multiple speakers
4. Ensure models are trained before identification

### OLED Display Issues
1. Check I2C connections (SDA=GPIO6, SCL=GPIO7)
2. Verify OLED address (0x3C)
3. Ensure adequate power supply

### Common Issues

1. **PyAudio Installation Error**:
   - Windows: Use `pipwin install pyaudio`
   - macOS: `brew install portaudio` then `pip install pyaudio`
   - Linux: `sudo apt-get install portaudio19-dev` then `pip install pyaudio`

2. **No Audio Device Found**:
   - Check microphone permissions
   - Ensure microphone is connected and working

3. **Low Accuracy**:
   - Collect more training samples
   - Improve recording quality
   - Ensure speakers have distinct voices

## 📈 Performance

- **Speaker Identification**: 85-95% accuracy with good training data
- **Emotion Detection**: Rule-based analysis, expandable to ML models
- **TCP Latency**: < 100ms on local network
- **Display Update**: Real-time updates on ESP32

## Technical Details

### Dependencies

- **librosa**: Audio analysis and feature extraction
- **scikit-learn**: Machine learning algorithms
- **pyaudio**: Audio recording
- **numpy**: Numerical computing
- **tkinter**: GUI framework (included with Python)

### System Requirements

- Python 3.7+
- Microphone for recording
- Minimum 4GB RAM recommended
- Windows/macOS/Linux compatible

## 🔮 Future Enhancements

- [ ] Machine learning emotion models
- [ ] Multiple ESP32 device support
- [ ] Web interface for remote monitoring
- [ ] Database logging of analysis results
- [ ] Advanced haptic patterns
- [ ] Mobile app integration

## 📄 License

This project is open source. Feel free to modify and distribute.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📞 Support

For issues and questions, please check the troubleshooting section or create an issue in the repository.
