# DoAKickFlip

This work uncovers a new attack surface to obtain an unauthenticated access of the target vehicles equipped with a kick-to-open tailgate by a physical event injection to generate the trunk system's state machine confusion. 

## Demo

- All demos in this research paper are available at https://www.youtube.com/playlist?list=PLDAjgjwZH3zmOoKnDZAg1Jb25UPUEU1Bo

## Attack Process Description

### Pre-attack Preparation:

- Before performing an attack, the attacker collects training data by placing the Raspberry Pi equipped with a USB mini microphone at the position where the attack device would be mounted during deployment (inside the rear bumper cavity), and recording the chime while repeatedly cycling the tailgate. The captured audio is processed in 200 ms frames using a sliding window with a 50 ms hop size, producing 75% overlap between consecutive frames. Each audio segment is converted to a decibel-scale spectrogram via the Short-Time Fourier Transform (STFT). To detect chime activity in each 200 ms chunk, a lightweight convolutional neural network (CNN) is emploied to operate on the input of two-channel spectrogram. The trained detection model will be upload to the Raspberry Pi.

### Attack Execution:

- As shown by the connection and components in 'Design>gadgetsize.pdf', the attacking device will be covertly mounted to the rear bumper cavity or chassis of the vehicle after the victim leaves. Once the victim returns and opens the tailgate, the adversary initiates the attack program, which begins listening passively. When the victim triggers the closing sequence with a kick gesture, the attack device automatically detects the onset of the closing operation through an acoustic side channel, specially the audible chime emitted during liftgate descent. After a calibrated delay, the device emits a spoofed kick event via EMI, halting at a position just short of latch engagement, leaving the vehicle physically unlocked while presenting the appearance of a completed locking interaction. The attacker then gains access to the trunk after the victim departs.

## Project Structure

This repository is organized into the following directories:

### Code

The AudioProcess directory contains tools for audio processing, segmentation, and labeling:

- `main.py`: A GUI application for visualizing audio spectrograms and creating labeled segments
- `clip.py`: Core functionality for splitting audio files into smaller segments based on markers
- `preprocess.py`: Preprocessing utilities for audio data
- `requirements.txt`: Python dependencies for the ClipTool

The Detection directory contains

- `audio_cnn_model_rpi_2ch.pth`: pretrained model
- `classification_new.py`: code needed to be download onto the Raspberry Pi to classify the first chime once tailgate starting lowering
- `model_info_rpi_2ch.pth`: pretrained model information
- `requirements.txt`: Python dependencies for the the audio classification
  
#### Description

- Contains the description of key steps to conduct the end-to-end attack on the target Audi Q5.

### Design

The Design directory contains wiring configurations for both indoor and outdoor attack experiments:

- `IndoorOscilloscope.pdf`: wiring configurations of indoor tests. The sensing module's response to kick motion can be observed through the signals on the connected oscilloscope. 
- `gadgetsize.pdf`: all the components comprised in the end-to-end attack device and their wiring configuration
- `shieldsensortest.pdf`: bench test configuration to identify the frequencies at which the adversary can inject a kick event

### LIN BUS

The LIN BUS directory stores all datasets exported from an Audi Q5 via its LIN bus connected to the kick sensing module and the corresponding waveforms caputred from an connectedd oscilloscope. 
-'box no key' refers to the signal is triggered by the portable attack device while vehicle's keyfob is not within the effective range
-'box with key' refers to the signal is triggered by the portable attack device while vehicle's keyfob is within the effective range
-'kick no key' refers to the signal is triggered by an kick motion while vehicle's keyfob is not within the effective range
-'kick with key' refers to the signal is triggered by an kick motion while vehicle's keyfob is within the effective range



### Module Testing
The Module Testing directory contains 4 typical sensing modules: two from Brose and two from Huf. Each file contains the whole sensing module and the label printed on the control part.


### Paper

The Paper directory contains research documentation:

- `Liftgate_CHES.pdf`: Research paper documenting the methodology and results


### Results
The Results directory contains the test results:
- `End-to-End Attack Test Records': the 100 liftgate gap measurements recorded from two parking lots under different ambient noise and wind speed conditions
  
## Getting Started

### ClipTool

1. Install dependencies:
   ```
   cd ClipTool
   pip install -r requirements.txt
   ```

2. Run the GUI application:
   ```
   python main.py
   ```


### Classification

1. Install dependencies:
   ```
   cd Code
   pip install -r requirements.txt
   ```
  
2. Run classification:
   ```
   python classification_rpi.py
   ```

## For Quick Test of the Classification Model On Audi Q5

- Download the three files in Folder "Detection", and run "classification_rpi.py"

## Features

- Audio visualization and segmentation with interactive GUI
- Automated audio clip extraction based on markers
- CNN-based classification of automotive chime sounds
- Model-driven sound detection
- Web interface for audio processing

## Requirements

- Python 3.6+
- PyTorch
- Librosa
- Matplotlib
- Tkinter (for GUI)
- Flask (for web interface)

## License

This project is proprietary and confidential.
