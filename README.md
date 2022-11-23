![Thunderhead SDK](https://i.imgur.com/gfizURy.png "Thunderhead")
![MXO_Logo](https://user-images.githubusercontent.com/75626649/203578506-ade030aa-22d1-406d-9e66-16747259fb1c.png)

## Resources

* [Migration Guides](https://github.com/thunderheadone/one-sdk-android/tree/master/migration-guides)
* [API docs](https://thunderheadone.github.io/one-sdk-android/)

## Table of Contents

* [Prerequisites](#prerequisites)
* [Step 1: Add the Thunderhead SDK to your app](#step-1-add-the-thunderhead-sdk-to-your-app)
* [Step 2: Configure the codeless Thunderhead SDK for Android](#step-2-configure-the-codeless-thunderhead-sdk-for-android)
    * [Set up the SDK in User mode for Play Store builds](#set-up-the-sdk-in-user-mode-for-play-store-builds)
    * [Set up the SDK in Admin mode for internal distribution](#set-up-the-sdk-in-admin-mode-for-internal-distribution)
* [Additional codeless integration considerations](#additional-codeless-integration-considerations)
    * [Sending codeless Interactions based on the list of Interactions created under a Touchpoint](#sending-codeless-interactions-based-on-the-list-of-interactions-created-under-a-touchpoint)
    * [Thunderhead Application Manifest file permissions](#thunderhead-application-manifest-file-permissions)
    * [Configure and reconfigure the SDK](#configure-and-reconfigure-the-sdk)
    * [SDK initialization not required](#sdk-initialization-not-required)
* [Additional features of the Thunderhead SDK](ADDITIONAL-FEATURES.md)
* [Troubleshooting guide](#troubleshooting-guide)
* [Questions or need help](#questions-or-need-help)
    * [Salesforce Interaction Studio support](#salesforce-interaction-studio-support)
    * [Thunderhead ONE support](#thunderhead-one-support)

## Prerequisites

+ [Android Gradle Plugin](https://developer.android.com/studio/releases/gradle-plugin) 3.6.x
+ Android 5.0+ (API 21) and above
+ [Gradle](https://gradle.org/releases/) 5.6.4

## Step 1: Add the Thunderhead SDK to your app

1. Open your existing Android application in Android Studio.
2. Include the Thunderhead SDK as a dependency in your project:
    Navigate to your **app-level** build.gradle.
    
    Add the following, under the dependencies section:
    
    For **Thunderhead ONE** integrations:

    ```gradle
    dependencies {     
      implementation "com.thunderhead.android:one-sdk:{SDK_VERSION}"
    }
    ```
    
    For **Salesforce Interaction Studio** integrations:
    
    ```gradle
    dependencies {     
      implementation "com.thunderhead.android:is-sdk:{SDK_VERSION}"
    }
    ```

3. Add the Thunderhead SDK configuration within the same **app-level** `build.gradle` file. 

    Add `RenderScript` support under the `defaultConfig` section:

    ```gradle
    defaultConfig {
        renderscriptTargetApi 22
        renderscriptSupportModeEnabled true
    }
    ```

    Add the following, under the repositories section:

    ``` gradle 
    repositories {
        maven {
            url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
        }
    }
    ```

    Append the following configuration, for both **Thunderhead ONE** and **Salesforce Interaction Studio** integrations:

    ``` gradle 
    apply plugin: 'com.thunderhead.android.orchestration-plugin'
    ```
  
4. Add Java 8 Support

    Add the following, under the `android` section

    ```groovy
    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
    ```

5. Update your `build.gradle` file to add codeless identity transfer support.

    Navigate to the **top-level** `build.gradle` file and add both a maven repository url and class path dependencies as shown below:

    ``` gradle 
    buildscript {
        repositories {
            google()
            mavenCentral()
            maven {
                name 'Thunderhead'
                url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
            }
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:4.2.0'
            classpath 'com.thunderhead.android:orchestration-plugin:6.0.1'
        }
    }
    ```

####  `build.gradle` examples

#####  **Thunderhead ONE** `build.gradle` examples

###### Example of the **top-level** `build.gradle` file after integration

``` gradle
buildscript {
    repositories {
        google()
        mavenCentral()
        maven {
            name 'Thunderhead'
            url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.2.0'
        classpath 'com.thunderhead.android:orchestration-plugin:6.0.1'
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}
```

###### Example of the **app-level** `build.gradle` file after integration

``` gradle
apply plugin: 'com.android.application'
apply plugin: 'com.thunderhead.android.orchestration-plugin'

android {
    compileSdkVersion 29
    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }

    defaultConfig {
        applicationId "com.thunderhead.android.demo"
        minSdkVersion 21
        targetSdkVersion 29
        versionCode 1
        versionName "1.0"

        renderscriptTargetApi 22
        renderscriptSupportModeEnabled true
    }
}

dependencies {     
  implementation "com.thunderhead.android:one-sdk:{SDK_VERSION}"
}

repositories {
    maven {
       url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
    }
}

```

#####  **Salesforce Interaction Studio** `build.gradle` examples

###### Example of the **top-level** `build.gradle` file after integration

``` gradle
buildscript {
    repositories {
        google()
        mavenCentral()
        maven {
            name 'Thunderhead'
            url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.2.0'
        classpath 'com.thunderhead.android:orchestration-plugin:6.0.1'
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}
```

###### Example of the **app-level** `build.gradle` file after integration

``` gradle
apply plugin: 'com.android.application'
apply plugin: 'com.thunderhead.android.orchestration-plugin'

android {
    compileSdkVersion 29
    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
    defaultConfig {
        applicationId "com.thunderhead.android.demo"
        minSdkVersion 21
        targetSdkVersion 29
        versionCode 1
        versionName "1.0"

        renderscriptTargetApi 22
        renderscriptSupportModeEnabled true
    }
}

dependencies {     
  implementation "com.thunderhead.android:is-sdk:{SDK_VERSION}"
}

repositories {
    maven {
       url 'https://thunderhead.mycloudrepo.io/public/repositories/one-sdk-android'
    }
}

```

For further documentation on the `orchestration-plugin` please see the [Orchestration Plugin Readme](https://github.com/thunderheadone/one-android-orchestration-plugin/blob/master/README.md).

## Step 2: Configure the codeless Thunderhead SDK for Android

Enable your app to automatically recognize **Interactions** in your app, by executing the following steps:

* *Developer note:* Android Studio `Instant Run` is not currently supported and must be disabled.

### Set up the SDK in User mode for Play Store builds

To start capturing insights and configuring orchestrations in User mode, you must first configure the Thunderhead SDK with your Thunderhead API parameters. 
You can find your Thunderhead API parameters on the _API Credentials_ page in Thunderhead ONE or Salesforce Interaction Studio.

For more information on finding these parameters: 
* For **Thunderhead ONE** integrations, see [Find the Information required when Integrating ONE with your Mobile Solutions](https://permalink.thunderhead.com/mobile-docs/one-mobile-integration-info) 
* For **Salesforce Interaction Studio** integrations, see [Find the Information required when Integrating Interaction Studio with your Mobile App](https://permalink.thunderhead.com/mobile-docs/is-mobile-integration-info) 

When you have your parameters, configure the SDK. We recommend adding the following lines of code for User Mode under the Application’s subclass `onCreate()` method, though this is not required. 
You must ensure the `oneConfigure` top-level Kotlin function or `setConfiguration` Java method is invoked after `super.onCreate()` is called.

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigure
import com.thunderhead.mobile.configuration.OneMode;
// The rest of the imports

class YourApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        oneConfigure {
            siteKey = SITE_KEY
            apiKey = API_KEY
            sharedSecret = SHARED_SECRET
            userId = USER_ID
            host = URI(HOST)
            touchpoint = URI(TOUCHPOINT)
            mode = OneMode.USER
        }
    }
    
    companion object {
        const val SITE_KEY = "ONE-XXXXXXXXXX-1022"
        const val API_KEY = "f713d44a-8af0-4e79-ba7e-xxxxxxxxx"
        const val SHARED_SECRET = "bb8bacb2-ffc2-4c52-aaf4-xxx"
        const val USER_ID = "yourUsername@yourCompanyName" // For Interaction Studio integrations use a numeric user id - see https://permalink.thunderhead.com/mobile-docs/is-mobile-integration-info-credentials
        const val HOST = "https://xx.thunderhead.com"
        const val TOUCHPOINT = "myAppsNameURI"
    }
}
```

`Java`
```java 
import com.thunderhead.mobile.configuration.OneConfiguration;
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.configuration.OneMode;
// The rest of the imports

public class YourApplication extends Application {
  
  private static final String siteKey = "ONE-XXXXXXXXXX-1022";
  private static final String apiKey = "f713d44a-8af0-4e79-ba7e-xxxxxxxxx";
  private static final String sharedSecret = "bb8bacb2-ffc2-4c52-aaf4-xxx";
  private static final String userId = "yourUsername@yourCompanyName"; // For Interaction Studio integrations use a numeric user id - see https://permalink.thunderhead.com/mobile-docs/is-mobile-integration-info-credentials
  private static final String host = "https://xx.thunderhead.com";
  private static final String touchpointURI = "myAppsNameURI";
  
    @Override
    public void onCreate() {
      super.onCreate();
      final OneConfiguration oneConfiguration = new OneConfiguration.Builder()
        .siteKey(siteKey)
        .apiKey(apiKey)
        .sharedSecret(sharedSecret)
        .userId(userId)
        .host(URI.create(host))
        .touchpoint(URI.create(touchpointURI))
        .mode(OneMode.USER)
        .build();

        One.setConfiguration(oneConfiguration);
    }
}
```

### Set up the SDK in Admin mode for internal distribution

To use the SDK in Admin mode, change the `mode` parameter to `OneMode.ADMIN`.

*Note:* 
- If you are running in Admin mode on Android 6.0+, you must enable the “draw over other apps” permission through your OS settings. 
- Dynamic configuration of both Admin and User mode is supported.

**You have now successfully integrated the codeless Thunderhead SDK for Android.**

### Additional codeless integration considerations

#### Sending codeless Interactions based on the list of Interactions created under a Touchpoint

In order to reduce the number of unnecessary Interaction requests sent automatically by the SDK, only codeless Interactions with explicit Interaction paths created under a Touchpoint and configured with at least one point are sent to Thunderhead ONE or Salesforce Interaction Studio. This configuration change has been introduced in version 8.1.0 of the Android SDK.

*Note:*
- The SDK will only send codeless Interactions if they have been created under a Touchpoint and/or if they match wildcard rules defined under a Touchpoint.
- For a codeless Interaction to be sent by the SDK this Interaction needs to contain at least one Activity Capture Point, Attribute Capture Point, and/or Optimization Point.
- If you are running the SDK in [User mode](#set-up-the-sdk-in-user-mode), you need to ensure that all Interactions and related points have been fully published, before the SDK will trigger a request.

#### Thunderhead Application Manifest file permissions

The following permissions are included in the Thunderhead SDK's `AndroidManifest.xml` and will be merged with your applications AndroidManifest.xml:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
```
*Note:* 
- The `SYSTEM_ALERT_WINDOW` permission is needed only for Admin mode builds. Add this as a flavor-specific permission  in your setup to avoid having to show it as a permission change to your Play Store users.
- You can remove this permission in User mode builds by adding the following to your manifest: 
```xml 
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" tools:node="remove" />
```

#### Configure and reconfigure the SDK

You can configure and reconfigure the SDK as many times as necessary. 
* The SDK does not support partial, or piecemeal, configuration. You must provide all parameters, either all valid or invalid (`empty string` or `null`).  
* When configured with invalid parameters, the SDK is set in an *unconfigured* state.

See [here](https://github.com/thunderheadone/one-sdk-android/tree/master/examples/dynamic-configuration-example) for an example app that demonstrates dynamic configuration.

#### SDK initialization not required

The Thunderhead SDK is automatically initialized in an *unconfigured* state.
* When *unconfigured*, the SDK queues end-user data locally and uploads that data to the server once the SDK is configured with valid parameters.
* You can disable this functionality, at any time, by setting the `oneOptOutConfiguration` to `true`. See more about opt out [here](#opt-an-end-user-out-of-tracking).

## Additional Features
For additional features of the Thunderhead SDK, please follow the [Additional Features Guide](ADDITIONAL-FEATURES.md)

## Troubleshooting guide
Having trouble with Thunderhead and your android project? Visit the [Troubleshooting Guide](TROUBLESHOOTING-GUIDE.md)

## Questions or need help

### Salesforce Interaction Studio support
_For Salesforce Marketing Cloud Interaction Studio questions, please submit a support ticket via https://help.salesforce.com/home_

### Thunderhead ONE support
_The Thunderhead team is available 24/7 to answer any questions you have. Just email onesupport@thunderhead.com or visit our docs page for more detailed installation and usage information._

