# android-ssl-pinning-bypass


---
![android](https://github.com/user-attachments/assets/7369766a-9843-43f5-bce3-04cbfeaa5a44)

# Android SSL Pinning Bypass

This repository contains scripts and tools to bypass SSL pinning in Android applications. SSL pinning is a security mechanism used by apps to prevent man-in-the-middle (MitM) attacks by ensuring that the app communicates only with a server using a specific SSL certificate. However, during security testing or reverse engineering, it might be necessary to bypass SSL pinning to intercept and analyze network traffic.

## Table of Contents

- [Introduction](#introduction)
- [How SSL Pinning Works](#how-ssl-pinning-works)
- [Bypass Techniques](#bypass-techniques)
  - [Frida](#frida)
  - [Xposed Framework](#xposed-framework)
  - [Custom Certificates](#custom-certificates)
- [Requirements](#requirements)
- [Setup and Usage](#setup-and-usage)
  - [Using Frida](#using-frida)
  - [Using Xposed](#using-xposed)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Introduction

SSL pinning is a technique used by mobile apps to enhance security by ensuring they only accept a specific certificate or public key. However, during penetration testing, it is often necessary to bypass SSL pinning to inspect the network traffic between the app and the server. This repository provides methods and tools to bypass SSL pinning in Android applications, allowing testers to carry out a more thorough security assessment.

## How SSL Pinning Works

In SSL pinning, an app is configured to only accept connections to a server that provides a certificate matching a pinned certificate or public key embedded within the app. This prevents the app from being tricked into connecting to a malicious server, even if the attacker can present a valid SSL certificate issued by a trusted certificate authority (CA).

## Bypass Techniques

### Frida

Frida is a dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers. It allows you to inject JavaScript into native apps on Windows, macOS, Linux, iOS, Android, and QNX.

- **Advantages**: Does not require root access, versatile and can be used on-the-fly.
- **Script Example**: A Frida script can be used to bypass SSL pinning by hooking specific methods in the app that perform certificate validation.

### Xposed Framework

The Xposed Framework allows you to modify the behavior of the system and apps without touching any APKs. It is useful for bypassing SSL pinning by modifying the way an app verifies certificates.

- **Advantages**: Powerful and persistent, works well for rooted devices.
- **Module Example**: There are several Xposed modules like "JustTrustMe" that can disable SSL pinning in many apps.

### Custom Certificates

Another method to bypass SSL pinning is to install a custom certificate authority (CA) on the device and modify the app to accept this CA.

- **Advantages**: Simple and effective for certain types of apps, especially those that use standard HTTP libraries.
- **Disadvantages**: Requires root access, and may not work with apps that enforce strict pinning.

## Requirements

- Android device or emulator
- [Frida](https://frida.re)
- [Xposed Framework](https://repo.xposed.info/)
- Root access (for Xposed or custom certificate methods)
- adb (Android Debug Bridge)

## Setup and Usage

### Using Frida

1. **Install Frida**:
   - Install Frida on your computer and Android device. Follow the instructions on the [Frida website](https://frida.re/docs/installation/).

2. **Start Frida Server**:
   - Run the Frida server on your Android device:
     ```bash
     adb push frida-server /data/local/tmp/
     adb shell "chmod 755 /data/local/tmp/frida-server"
     adb shell "/data/local/tmp/frida-server &"
     ```

3. **Run the Frida Script**:
   - Use a Frida script to hook into the app and bypass SSL pinning:
     ```bash
     frida -U -f com.example.app -l ssl-pinning-bypass.js --no-pause
     ```

### Using Xposed

1. **Install Xposed Framework**:
   - Install the Xposed Framework on your rooted Android device.

2. **Install a SSL Pinning Bypass Module**:
   - Install a module like "JustTrustMe" from the Xposed Module Repository.

3. **Activate the Module**:
   - Activate the module in the Xposed Installer and reboot your device.

4. **Test the Application**:
   - Run the application on your Android device. The SSL pinning should now be bypassed.

## Troubleshooting

- **Frida Not Hooking**: Ensure Frida server is running correctly on your Android device and that your device is properly connected to your computer.
- **Xposed Module Not Working**: Verify that the module is compatible with the version of Android and Xposed you are using.

---

# Android SSL Pinning Bypass Using Objection

This repository provides a step-by-step guide on how to bypass SSL pinning in Android applications using the Objection tool. SSL pinning is a security feature used by mobile apps to prevent man-in-the-middle (MitM) attacks by verifying the server's certificate. However, during penetration testing, it is often necessary to bypass SSL pinning to inspect the app's network traffic.

## Table of Contents

- [Introduction](#introduction)
- [Why Bypass SSL Pinning?](#why-bypass-ssl-pinning)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Using Objection to Bypass SSL Pinning](#using-objection-to-bypass-ssl-pinning)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Objection is a powerful runtime mobile exploration toolkit, powered by Frida, that allows you to perform various security assessments on Android applications without needing root access. One of its key features is the ability to bypass SSL pinning, which is crucial for inspecting HTTPS traffic between the app and the server during penetration testing.

## Prerequisites

Before you begin, ensure you have the following:

- **Android Device or Emulator**: A physical device or Android emulator where the target app is installed.
- **Objection**: Installed on your system (see the installation instructions below).
- **Frida**: Installed on both your computer and Android device.
- **ADB (Android Debug Bridge)**: Used for connecting to the Android device or emulator.

## Installation

### 1. Install Python and Pip

Ensure Python 3.x and pip are installed on your system. If not, install them:

- **On Ubuntu/Debian**:
  ```bash
  sudo apt-get install python3 python3-pip
  ```
- **On macOS** (using Homebrew):
  ```bash
  brew install python3
  ```

### 2. Install Frida

Install Frida using pip:

```bash
pip install frida-tools
```

### 3. Install Objection

Install Objection via pip:

```bash
pip install objection
```

### 4. Verify Installation

Ensure Objection is correctly installed by running:

```bash
objection --help
```

## Using Objection to Bypass SSL Pinning

### Step 1: Connect to Your Android Device

First, connect your Android device to your computer using ADB:

```bash
adb devices
```

Ensure your device is listed. If not, troubleshoot your ADB connection.

### Step 2: Identify the Target App

Identify the package name of the app you want to test. You can list all installed packages with:

```bash
adb shell pm list packages
```

Alternatively, if you know the app's name, you can filter the results:

```bash
adb shell pm list packages | grep <app-name>
```

### Step 3: Start Objection with the App

Launch Objection with the target app:

```bash
objection -g <app_package_name> explore
```

Replace `<app_package_name>` with the actual package name of the target app.

### Step 4: Bypass SSL Pinning

Once inside the Objection interactive shell, run the following command to disable SSL pinning:

```bash
android sslpinning disable
```

This command hooks into the app’s SSL pinning logic and disables it, allowing you to intercept the app’s HTTPS traffic using tools like Burp Suite.

### Step 5: Inspect Network Traffic

With SSL pinning disabled, you can now use a proxy tool (e.g., Burp Suite) to inspect the network traffic between the app and its server.

## Troubleshooting

- **Objection Not Hooking Properly**:
  - Ensure Frida is correctly installed and the Frida server is running on your Android device.
  - Verify that your device is properly connected via ADB.

- **SSL Pinning Still Active**:
  - Some apps may use more advanced SSL pinning techniques that Objection cannot bypass by default. In such cases, you may need to write custom Frida scripts.

---
## Contributing

Contributions are welcome! If you have improvements or additional methods to bypass SSL pinning, feel free to fork this repository, make your changes, and submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

This README provides a clear, organized guide on using the repository to bypass SSL pinning in Android applications, making it accessible to developers and security professionals alike. Adjustments can be made based on the specific tools or methods included in your project.




