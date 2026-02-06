# Android APK Reverse Engineering & Compatibility Patch
This project demonstrates a complete workflow for Reverse Engineering an Android application. The goal was to patch a legacy "Device ID" application to make it compatible with modern Android 14 (API 34) devices.

🛠️ The Challenge
When attempting to run the legacy APK on modern Android versions, several issues occurred:

Manifest Incompatibility: Missing uses-sdk tags and android:exported attributes required by Android 12+.

Resource Linking Errors: Failed aapt2 compilation due to broken references in public.xml and anims.xml after decompilation.

Signature Requirements: Modern Android systems reject unsigned or incorrectly signed APKs.

🚀 The Solution
I implemented a systematic 4-step approach to fix these issues:

1. Decompilation & Manifest Patching
Used Apktool to decompile the APK. I manually injected the necessary SDK tags and set android:exported="true" for the main activity to comply with Android 14 security standards.

2. Automated Resource Cleaning (PowerShell)
To solve the aapt2 linking errors, I developed a PowerShell automation command to filter out "Dangling References" (like abc_fade_in) from public.xml.

PowerShell
powershell -Command "(Get-Content 'res\values\public.xml') | Where-Object { $_ -notmatch 'abc_fade_in|abc_fade_out|abc_slide_in_bottom|abc_slide_in_top' } | Set-Content 'res\values\public.xml'"
3. Rebuilding & Java-based Signing
Rebuilt the APK using 8 threads for maximum performance. The final package was signed using uber-apk-signer, ensuring it passed Android's V2/V3 signature verification.

4. Deployment via ADB
Deployed the final patched APK directly to the device using ADB (Android Debug Bridge).

🧰 Tools Used
Apktool: For decompiling and rebuilding resources.

Java (JDK 25): For running the signing tools and managing JAR files.

uber-apk-signer: For Zipalign and Digital Signing.

ADB & scrcpy: For device testing and screen mirroring.

PowerShell: For automated text processing and patching.

🎓 Academic Context
This project was completed as part of my self-directed learning in Computer Science.

⚠️ Disclaimer
This project is for educational purposes only. Always respect the terms of service of any software you analyze.
