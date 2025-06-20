# Face Recognition Door System

## Overview

This project implements a face recognition door access system running on a Raspberry Pi, combining a touchscreen interface with face recognition, Google Sheets logging, and relay control for physical access (e.g., opening a door). The application is written in Python, using PyQt5 for the GUI, OpenCV and face_recognition for image processing, and gspread + Google APIs for logging entry events. It is designed for kiosk-style, full-screen operation.

## Features

- **Touchscreen GUI** with PyQt5, optimized for 720x1280 resolution.
- **User Registration**: 
  - Users register by scanning a QR code (linking to a Google Form), then capturing a face image with the camera.
  - Prevents duplicate IDs and ensures a face is present in the image.
- **Admin Login**: 
  - Protected login for administrators (hardcoded credentials: `ROOT`/`ADMIN`).
  - Virtual keyboard provided for touchscreen input.
- **Face Recognition**:
  - Live video feed with face detection.
  - Recognizes registered faces and logs successful entries with timestamp to Google Sheets.
  - Controls GPIO pins to trigger relays (e.g., open a door) upon successful recognition.
- **Google Sheets Integration**:
  - Logs each access event (`name`, `timestamp`) to a Google Sheet.
- **Relay Control**:
  - Uses Raspberry Pi GPIO pins to control external devices (e.g., unlock doors).
- **Auto-Restart and Cleanup**:
  - Restarts the program on exit for reliability.
  - Cleans up GPIO state on start.
- **Loading Animations and Progress Bars**:
  - Smooth user experience with progress indicators and GIFs during operations.
- **Network Info Display**:
  - Shows the device's IP address, useful for VNC/control.
- **Error Handling and User Guidance**:
  - Handles multiple faces, no face detected, and duplicate ID scenarios.
  - Provides user feedback via dialog boxes and labels.

## Application Flow

### 1. Startup
- The application starts in full-screen mode and displays a loading animation.
- The main menu presents two options: "Register" and "Main Menu" (for face recognition/entry).

### 2. Registration Process
- User confirms they want to register.
- A QR code linking to a Google Form is shown.
- After completing the form, user proceeds to capture their face.
- The system ensures the ID is unique and a face is present.
- If successful, the face image is saved and the user can add more or proceed to scanning.

### 3. Face Recognition (Entry)
- Users approach the kiosk and put their face in the indicated area.
- If a single registered face is detected with high confidence, the system:
  - Displays user's name and confidence.
  - Opens the door by triggering relays.
  - Logs the entry to Google Sheets.
  - Shows the matched face image for a few seconds.
- If not recognized or multiple/no faces are detected, instructs the user accordingly.

### 4. Admin Login
- Separate dialog for admin login using a virtual keyboard.
- If login successful, admin can access additional features (e.g., registration window).

### 5. Error Handling & System Management
- Application auto-restarts on exit.
- Handles hardware (camera, GPIO) and network resource management.
- Robust error dialogs and feedback for all failure cases.

## File Structure

- **main.py** (shown above): Main application code.
- **/image/**: Directory for storing user face images.
- **/loading.gif**: Loading animation for the UI.
- **/uploadlog.json**: Google Service Account credentials for Sheets API.
- **dlib_face_recognition_resnet_model_v1.dat**: Dlib model for face encoding.
- **requirements.txt**: List of required Python packages.

## Hardware Requirements

- Raspberry Pi (with GPIO)
- Camera (USB or Pi Camera)
- Touchscreen display (for full-screen GUI)
- Relays connected to GPIO 23 and 24

## Python Dependencies

- PyQt5
- OpenCV (`cv2`)
- face_recognition
- dlib
- gspread
- oauth2client
- google-api-python-client
- google-auth, google-auth-oauthlib
- qrcode
- RPi.GPIO

Install with:
```bash
pip install -r requirements.txt
```

## Setup Instructions

1. **Google Sheets API**
   - Create a Google Cloud project, enable Sheets API, and download a service account JSON.
   - Place `uploadlog.json` in the project directory.
   - Set the correct spreadsheet key in `spreadsheet_key`.

2. **Face Dataset**
   - Place face images named as `[user_id].jpg` in the `/image/` directory.

3. **Run the App**
   ```bash
   python main.py
   ```

## Security Note

- The admin login credentials are hardcoded as `ROOT`/`ADMIN`. Change these in the code for production.
- The app runs with full system access, so restrict physical and network access to trusted personnel.

## License

[MIT License](LICENSE)

## Contact

For questions or support, please contact the project maintainer.
