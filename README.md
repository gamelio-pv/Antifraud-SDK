# PayValida Antifraud SDK

This is the PayValida SDK for Android and iOS applications, designed
to provide fraud detection capabilities for Android and iOS applications.

## Using the SDK

### Android Integration

1. **Add the AAR:**
   - Download and unzip
     [antifraud-sdk-release.aar](https://github.com/gamelio-pv/Antifraud-SDK/releases/download/1.1.0/antifraud-sdk-release.aar).
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

2. **Usage Examples (Kotlin):**

    **3DS Validation:**
    ```kotlin
    import payvalida.PayvalidaAntifraudSdk
    import payvalida.models.FormData
    import payvalida.Environment

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

    **Kount Fraud Detection:**
    ```kotlin
    import payvalida.PayvalidaAntifraudSdk
    import payvalida.models.KountData
    import payvalida.Environment

    // ... inside your Activity or ViewModel
    suspend fun performKountValidation(kountData: KountData) {
        val sdk = PayvalidaAntifraudSdk(Environment.SANDBOX) // Or Environment.PRODUCTION
        try {
            val response = sdk.startKountValidator(kountData)
            if (response.isApproved) {
                // Handle successful fraud analysis
                // response.data?.kountSessiontId contains the Kount session ID
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
   - Download and unzip
     [PayValidaAntifraudSDK-iOS.zip](https://github.com/gamelio-pv/Antifraud-SDK/releases/download/1.1.0/PayvalidaAntifraudSDK-iOS.zip.).
     This will give you the `PayValidaAntifraudSDK.xcframework` directory.
   - Open your Xcode project.
   - Drag the `PayValidaAntifraudSDK.xcframework` directory from Finder into
     your Xcode project's "Frameworks, Libraries, and Embedded Content" section
     (found under your app target's "General" settings).
   - When prompted, ensure "Copy items if needed" is checked.
   - Verify that `PayValidaAntifraudSDK.xcframework` is listed with "Embed &
     Sign" under the "Embed" column.

2. **Usage Examples (Swift):**

   **3DS Validation:**
    ```swift
    import payvalida

    class MyViewController: UIViewController {

        func perform3DSValidation(formData: FormData) {
            let sdk = PayvalidaAntifraudSdk(env: Environment.sandbox) // Or Environment.production

            Task {
                do {
                    let formData = FormData(
                      // ...
                    )

                    // startThreeDSValidator will be available as an async function in Swift.
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

    **Kount Fraud Detection:**
    ```swift
    import payvalida

    class MyViewController: UIViewController {

        func performKountValidation(kountData: KountData) {
            let sdk = PayvalidaAntifraudSdk(env: Environment.sandbox) // Or Environment.production

            Task {
                do {
                    let kountData = KountData(
                        token: "your-token",
                        cardNumber: "4111111111111111",
                        expirationMonth: "12",
                        expirationYear: "25"
                    )

                    // startKountValidator will be available as an async function in Swift.
                    let response = try await sdk.startKountValidator(data: kountData)

                    if response.isApproved {
                        // Handle successful fraud analysis
                        if let sessionId = response.data?.kountSessiontId {
                            print("Kount session ID: \(sessionId)")
                        }
                    } else {
                        // Handle failed validation, check response.error
                        if let errorDescription = response.error {
                            print("Kount validation error: \(errorDescription)")
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
