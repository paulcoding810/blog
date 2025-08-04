---
date: "2025-08-04T20:55:15+07:00"
draft: false
title: "Android Camera Guide"
summary: "A comprehensive guide to Android CameraX and Camera2 libraries."
categories:
  - Code
tags:
  - Android
  - CameraX
  - Camera2
---

## Choosing the Right Camera Library

Android provides multiple camera libraries:

- **CameraX**: Modern, lifecycle-aware, and easy to use.
- **Camera2**: Flexible but requires more setup.
- **Camera**: Deprecated and not recommended for new projects.

[Learn more](https://developer.android.com/media/camera/choose-camera-library)

## Key Components of CameraX

| Component           | Description                                                                             |
| ------------------- | --------------------------------------------------------------------------------------- |
| **CameraProvider**  | Manages camera connections and lifecycle binding.                                       |
| **UseCase**         | Represents camera features like Preview, ImageCapture, VideoCapture, and ImageAnalysis. |
| **Lifecycle-Aware** | Automatically handles camera resources based on lifecycle events.                       |

### Use Cases

#### a. Preview: Display the Camera Feed

```kotlin
val preview = Preview.Builder().build().also {
    it.setSurfaceProvider(viewFinder.surfaceProvider)
}
```

#### b. ImageCapture: Capture Still Images

```kotlin
val imageCapture = ImageCapture.Builder()
    .setTargetRotation(Surface.ROTATION_0)
    .setCaptureMode(ImageCapture.CAPTURE_MODE_MINIMIZE_LATENCY)
    .build()

imageCapture.takePicture(
    outputFileOptions,
    ContextCompat.getMainExecutor(context),
    object : ImageCapture.OnImageSavedCallback {
        override fun onImageSaved(outputFileResults: ImageCapture.OutputFileResults) {
            // Handle successful image capture
        }

        override fun onError(exception: ImageCaptureException) {
            // Handle error
        }
    }
)
```

#### c. VideoCapture: Record Videos

```kotlin
val videoCapture = VideoCapture.Builder().build()
videoCapture.startRecording(
    outputFileOptions,
    ContextCompat.getMainExecutor(context),
    object : VideoCapture.OnVideoSavedCallback {
        override fun onVideoSaved(outputFileResults: VideoCapture.OutputFileResults) {
            // Handle successful video capture
        }

        override fun onError(videoCaptureError: Int, message: String, cause: Throwable?) {
            // Handle error
        }
    }
)
```

#### d. ImageAnalysis: Analyze Camera Frames

```kotlin
val imageAnalysis = ImageAnalysis.Builder().build().also {
    it.setAnalyzer(ContextCompat.getMainExecutor(context), YourImageAnalyzer())
}
class YourImageAnalyzer : ImageAnalysis.Analyzer {
    override fun analyze(image: ImageProxy) {
        // Process the image frame
        image.close() // Don't forget to close the image
    }
}
```

### Camera Initialization

```kotlin
val cameraProviderFuture = ProcessCameraProvider.getInstance(context)
cameraProviderFuture.addListener({
    val cameraProvider = cameraProviderFuture.get()

    // Bind use cases to lifecycle
    cameraProvider.bindToLifecycle(
        lifecycleOwner,
        CameraSelector.DEFAULT_BACK_CAMERA,
        preview,
        imageCapture,
        videoCapture,
        imageAnalysis
    )
}, ContextCompat.getMainExecutor(context))
```

### Switching Cameras

```kotlin
val newCameraSelector = CameraSelector.DEFAULT_FRONT_CAMERA
cameraProvider.unbindAll()
cameraProvider.bindToLifecycle(this, newCameraSelector, preview, imageCapture)
```

## Face Detection with ML Kit

```kotlin
val faceDetector = FaceDetection.getClient(
    FaceDetectorOptions.Builder()
        .setPerformanceMode(FaceDetectorOptions.PERFORMANCE_MODE_FAST)
        .setLandmarkMode(FaceDetectorOptions.LANDMARK_MODE_ALL)
        .build()
)
imageAnalysis.setAnalyzer(ContextCompat.getMainExecutor(context), { imageProxy ->
    val mediaImage = imageProxy.image ?: return@setAnalyzer
    val rotationDegrees = imageProxy.imageInfo.rotationDegrees

    val inputImage = InputImage.fromMediaImage(mediaImage, rotationDegrees)
    faceDetector.process(inputImage)
        .addOnSuccessListener { faces ->
            // Process detected faces
            for (face in faces) {
                // Handle each face
            }
        }
        .addOnFailureListener { e ->
            // Handle error
        }
        .addOnCompleteListener {
            imageProxy.close() // Close the image proxy
        }
})
```

## Camera Permissions

Ensure you handle camera permissions in your app. Use `ActivityCompat.requestPermissions` to request permissions at runtime.

## CameraX vs CameraProvider Comparison

| Feature                | CameraController                                            | CameraProvider                                              |
| ---------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
| **Lifecycle**          | Manages camera lifecycle automatically                      | Requires manual binding to lifecycle                        |
| **Use Cases**          | Supports Preview, ImageCapture, VideoCapture, ImageAnalysis | Supports Preview, ImageCapture, VideoCapture, ImageAnalysis |
| **Camera Selection**   | Simplified camera selection                                 | Uses CameraSelector for camera selection                    |
| **Configuration**      | Provides a simple API for configuration                     | More flexible but requires more setup                       |
| **Threading**          | Uses main thread for callbacks                              | Allows custom executor for callbacks                        |
| **Error Handling**     | Built-in error handling                                     | Requires custom error handling                              |
| **Face Detection**     | Built-in support for face detection                         | Requires additional libraries (e.g., ML Kit)                |
| **Camera Switching**   | Simplified camera switching                                 | Requires unbinding and rebinding                            |
| **Performance**        | Optimized for common use cases                              | More flexible but may require optimization                  |
| **Dependencies**       | Minimal dependencies                                        | Requires CameraX and related libraries                      |
| **Use Case Binding**   | Automatically binds use cases                               | Requires manual binding of use cases                        |
| **Camera Control**     | Provides camera control methods                             | Requires CameraControl for advanced control                 |
| **Image Capture**      | Simplified image capture API                                | More flexible but requires more setup                       |
| **Video Capture**      | Simplified video capture API                                | More flexible but requires more setup                       |
| **Image Analysis**     | Simplified image analysis API                               | More flexible but requires more setup                       |
| **Camera Permissions** | Handles camera permissions automatically                    | Requires manual permission handling                         |
| **Architecture**       | Follows CameraX architecture                                | Follows CameraX architecture                                |
| **Surface**            | PreviewView                                                 | Custom Surface                                              |

For more details, refer to the [CameraX Architecture](https://developer.android.com/media/camera/camerax/architecture).
