# AI Photographer – Professional DSLR Camera with Live AI Assistant

A professional, on-device AI-assisted camera application for Android, designed with a premium DSLR-style interface (dark theme + gold accents).

## Features

### Camera
- Full-screen CameraX preview
- Tap-to-focus
- Exposure compensation
- Zoom (pinch + slider)
- Flash / torch cycling (Off → On → Auto)
- Front / rear camera switch
- Rule-of-thirds grid overlay
- Timer (3 / 5 / 10 s)
- Capture with shutter animation feedback

### Photography Modes
- Auto, Portrait, Landscape, Street, Night, Macro, Cinematic, Food, Selfie  
Each mode shows contextual advice. Selfie prefers the front camera.

### Live AI Assistant (on-device)
- ML Kit Face Detection (fast mode)
- Composition guidance (rule of thirds, headroom, distance)
- Expression / eye-open checks
- Mode-aware suggestions
- Throttled updates for battery & performance
- Latest-frame-only analysis (STRATEGY_KEEP_ONLY_LATEST)

Examples:
- "Move slightly closer."
- "Excellent composition. Capture now!"
- "Place the subject near a left or right grid line."
- "Turn your face slightly toward the camera."

### Gallery & Editing
- Integrated gallery of captured photos
- Long-press to delete
- Basic editor: brightness, contrast, saturation (ColorMatrix)
- Share via system sheet
- Original photo is never overwritten; Save creates a new file

### Technical
- Kotlin + View Binding
- CameraX 1.3.4 + Camera2 interop
- ML Kit Face Detection + Pose Detection (pose ready for future extension)
- Min SDK 23, Target / Compile SDK 34
- Material Design 3 dark theme
- FileProvider for sharing
- MediaStore for Android 10+ saves
- Efficient single-thread analysis executor

## Target Device
Optimized for mid-range devices such as Realme Narzo N53 (Android 15).  
All AI runs on-device. No photos are uploaded without explicit user action (share).

## Build Instructions

### Requirements
- Android Studio Hedgehog / Iguana or newer (or command-line SDK)
- JDK 17
- Android SDK Platform 34 + Build-Tools 34

### From Android Studio
1. Open the `AIPhotographer` folder.
2. Let Gradle sync.
3. Run on a device or emulator that has a camera.

### From command line
```bash
./gradlew assembleDebug
# APK: app/build/outputs/apk/debug/app-debug.apk
```

### GitHub Actions
A workflow is included under `.github/workflows/build.yml` that builds the debug APK on push.

## Permissions
- CAMERA (required)
- READ_MEDIA_IMAGES / legacy storage (for gallery on older APIs)

## Limitations & Graceful Degradation
- Full manual ISO / shutter speed / white-balance controls depend on Camera2 characteristics and are not exposed as free-form dials on every device; the app uses CameraX’s supported controls (exposure compensation, flash, zoom, focus).
- Pose detection is integrated in the AI module but the current live guidance focuses on face + composition for performance on lower-RAM devices.
- Advanced colour grading / cinematic LUTs and portrait enhancement are represented by the basic ColorMatrix editor; heavy GPU filters would require additional native libraries.
- HDR and certain extension modes are available only when the device reports support via CameraX Extensions.

## Project Structure
```
app/src/main/java/com/aiphotographer/app/
  ai/          – AIPhotographyAssistant, PhotoMode
  camera/      – CameraManager, PhotoModeHelper
  ui/          – MainActivity, GalleryActivity, EditActivity, GridOverlayView
  utils/       – PhotoStorage
  AIPhotographerApp.kt
```

## Privacy
All analysis is on-device. Photos stay on the device unless the user explicitly shares them.

---
Built as a complete, installable Android Studio project.
