# PayValida Antifraud SDK

This is the PayValida Antifraud SDK for Android and iOS applications, designed
to provide 3DS validation and Kount fraud detection capabilities for Android and
iOS applications.

## Using the SDK

### Android Integration

1. **Add the AAR:**
   - Download and unzip `PayValidaAntifraudSDK-Android.zip`.
   - Copy the `antifraud-sdk-release.aar` file into the `libs` directory of your
     Android application module (e.g., `your-app/app/libs`). Create the `libs`
     directory if it doesn't exist.
   - Add the AAR as a dependency in your module-level `build.gradle.kts` (or
     `build.gradle`) file:

     ```kotlin
     // For app/build.gradle.kts
     dependencies {
         implementation(files("libs/antifraud-sdk-release.aar"))
         // ... other dependencies
     }
     ```
     Or for `app/build.gradle` (Groovy):
     ```groovy
     dependencies {
         implementation files('libs/antifraud-sdk-release.aar')
         // ... other dependencies
     }
     ```
   - Sync your Gradle project.

2. **Usage Example (Kotlin):**

   ```kotlin
   // Ensure to import necessary classes.
   // If PayvalidaAntifraudSdk and Environment are in the root of your commonMain:
   import PayvalidaAntifraudSdk
   import Environment
   // If they are packaged (e.g., in com.payvalida.antifraud):
   // import com.payvalida.antifraud.PayvalidaAntifraudSdk
   // import com.payvalida.antifraud.Environment
   import models.FormData // Assuming FormData is in a 'models' package

   // ... inside your Activity or ViewModel
   suspend fun perform3DSValidation(formData: FormData) {
       val sdk = PayvalidaAntifraudSdk(Environment.SANDBOX) // Or Environment.PRODUCTION
       try {
           val response = sdk.startThreeDSValidator(formData)
           if (response.isApproved) {
               // Handle successful validation
           } else {
               // Handle failed validation, check response.error
           }
       } catch (e: Exception) {
           // Handle exceptions
       }
   }
   ```

### iOS Integration (Swift)

1. **Add the XCFramework:**
   - Download and unzip `PayValidaAntifraudSDK-iOS.zip`. This will give you the
     `PayValidaAntifraudSDK.xcframework` directory.
   - Open your Xcode project.
   - Drag the `PayValidaAntifraudSDK.xcframework` directory from Finder into
     your Xcode project's "Frameworks, Libraries, and Embedded Content" section
     (found under your app target's "General" settings).
   - When prompted, ensure "Copy items if needed" is checked.
   - Verify that `PayValidaAntifraudSDK.xcframework` is listed with "Embed &
     Sign" under the "Embed" column.

2. **Usage Example (Swift):**

   ```swift
   import PayValidaAntifraudSDK
   // Import models if they are exposed and needed, e.g., import models

   class MyViewController: UIViewController {

       func perform3DSValidation(formData: FormData) { // Assuming FormData is a Swift-compatible type
           let sdk = PayValidaAntifraudSdk(env: .sandbox) // Or .production

           Task {
               do {
                   // Note: startThreeDSValidator is a suspend function in Kotlin.
                   // It will be available as an async function in Swift.
                   let response = try await sdk.startThreeDSValidator(data: formData)

                   if response.isApproved {
                       // Handle successful validation
                   } else {
                       // Handle failed validation, check response.error
                       if let errorDescription = response.error {
                           print("Validation error: \(errorDescription)")
                       }
                   }
               } catch {
                   // Handle exceptions
                   print("An error occurred: \(error)")
               }
           }
       }
   }
   ```
