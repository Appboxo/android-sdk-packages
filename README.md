## Appboxo Android SDK

### Getting started

Please see our [sample Android app](https://github.com/Appboxo/android-sample-hostapp) to learn more.

1. Install AndroidStudio 3.0 or later.
2. Make sure that your project meets the minimum requirement of SDK 21
3. Add the below code to your app `build.gradle` file (not your root build.gradle file).


```gradle
dependencies {
    implementation 'com.appboxo:miniapp-sdk:1.3.40'
}
```

Add this in your root `build.gradle` file (**not** your module `build.gradle` file):

```gradle
allprojects {
    repositories {
        maven {
            url "https://maven.pkg.github.com/Appboxo/android-sdk-packages"
            credentials {
                username = "appboxoandroidsdk"
                password = "\u0037\u0039\u0064\u0031\u0065\u0064\u0061\u0036\u0030\u0034\u0063\u0061\u0031\u0066\u0030\u0032\u0066\u0031\u0037\u0066\u0032\u0061\u0039\u0033\u0064\u0035\u0039\u0039\u0061\u0035\u0035\u0062\u0066\u0065\u0031\u0064\u0066\u0064\u0038\u0038"
            }
        }
    }
}
```
4. Init Appboxo SDK in your Application class.

```kotlin
// Kotlin

class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        Appboxo.init(this)
            .setConfig(
                Config.Builder()
                    .setClientId("CLIENT_ID")
                    .multitaskMode(false) //by default 'true', each miniapp appears as a task in the Recents screen.
                    .build()
            )
            .setLogger(DefaultLogger(BuildConfig.DEBUG))
            //or use your own Logger
            /*
            .setLogger(object: Logger{
                override fun error(error: Throwable) {
                    //doSomething
                }

                override fun debug(message: String) {
                    //doSomething
                }
            })
            */
    }
}
```

```java
// Java

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Appboxo.INSTANCE.init(this)
                .setConfig(new Config.Builder()
                        .setClientId("CLIENT_ID")
                        .multitaskMode(false) //by default 'true', each miniapp appears as a task in the Recents screen.
                        .build())
                .setLogger(new DefaultLogger(BuildConfig.DEBUG));
    }
}
```

5. The Appboxo Sdk uses GoogleMaps' SDK in Miniapps. In `AndroidManifest.xml`, add the following element as a child of the `<application>` element, by inserting it just before the closing `</application>` tag:
```xml
<meta-data
  android:name="com.google.android.geo.API_KEY"
  android:value="YOUR_API_KEY" />
```
For more information, see https://developers.google.com/maps/documentation/android-sdk/get-api-key

6. To open miniapps, write in your Activity:

```kotlin
// Kotlin

val miniapp = Appboxo.getMiniapp(appId)
miniapp.open(context)
```

```java
// Java

Miniapp miniapp = Appboxo.INSTANCE.getMiniapp(appId).setAuthPayload(authPayload)
miniapp.open(context);
```

to transfer your data to miniapp use the following method with `data`
```kotlin
// Kotlin
Appboxo.getMiniapp(appId)
        .setData(data)
```

### Miniapp's lifecycle events:

Miniapp's lifecycle events help to track miniapps' activities like: `onLaunch`, `onResume`, `onPause`, `onClose` and `onError`

```kotlin
// Kotlin

miniapp.setLifecycleListener(object : Miniapp.LifecycleListener {
    override fun onLaunch(miniapp: Miniapp) {
        // Called when the miniapp will launch with Appboxo.open(...)
    }

    override fun onResume(miniapp: Miniapp) {
        // Called when the miniapp will start interacting with the user
    }

    override fun onPause(miniapp: Miniapp) {
        // Called when the miniapp loses foreground state
    }

    override fun onClose(miniapp: Miniapp) {
        // Called when clicked close button in miniapp or when destroyed miniapp activity
    }

    override fun onError(miniapp: Miniapp, message: String) {
        // Called when miniapp fails to launch due to internet connection issues
    }
})
```

```java
//Java

miniapp.setLifecycleListener(new Miniapp.LifecycleListener() {
    @Override
    public void onLaunch(@NotNull Miniapp miniapp) {
        //Called when the miniapp will launch with Appboxo.open(...)
    }

    @Override
    public void onResume(@NotNull Miniapp miniapp) {
        //Called when the miniapp will start interacting with the user
    }

    @Override
    public void onPause(@NotNull Miniapp miniapp) {
        //Called when the miniapp loses foreground state
    }

    @Override
    public void onClose(@NotNull Miniapp miniapp) {
        //Called when clicked close button in miniapp or when destroyed miniapp activity
    }

    @Override
    public void onError(@NotNull Miniapp miniapp, @NotNull String message) {

    }
});
```

### `logout` to clear mininapp data
When host app user logs out, it is highly important to clear all miniapp storage data. To clear call `.logout()`:

```kotlin
// Kotlin
Appboxo.logout()
```

### Dark mode

Dark mode support for splash screen and other native components used inside miniapp.

```kotlin
// Kotlin
Appboxo.setConfig(Config.Builder()
    .setClientId("CLIENT_ID")
    .setTheme(Config.Theme.DARK) //[DARK, LIGHT, SYSTEM]. SYSTEM - by default
    .build())
``` 
Use MiniappConfig to override theme of miniapp. 
```kotlin
// Kotlin
Appboxo.getMiniapp("MINIAPP_ID")
    .setConfig(MiniappConfig.Builder()
                    .setTheme(Config.Theme.LIGHT)
                    .build())
    .open()
```

### Miniapp URL listener

To listen for any URL change events use `.setUrlChangeListener`:

```kotlin
// Kotlin
Appboxo.getMiniapp("MINIAPP_ID")
    .setConfig(
        MiniappConfig.Builder()
            .build()
    )
    .setUrlChangeListener { appboxoActivity, miniapp, uri ->
        Log.e("URL Path", uri.path)
        uri.queryParameterNames.forEach {
            Log.e("URL params", "$it = ${uri.getQueryParameter(it)}")
        }
    }
    .open(this)
```

### Append custom params to miniapp URL

To append custom data params to miniapp URL use `.setExtraUrlParams`:

```kotlin
// Kotlin
Appboxo.getMiniapp("MINIAPP_ID")
    .setConfig(
        MiniappConfig.Builder()
            .setExtraUrlParams(
                mapOf(
                    "extra_param_1" to "test",
                    "extra_param_2" to "test_2"
                )
            )
            .build()
    )
    .open(this)
```

### Custom action menu item

You can add your custom action menu item to existing default action menu item by defining `.setCustomActionMenuItem` and listening for click event using `.setCustomActionMenuItemClickListener`


```kotlin
// Kotlin
Appboxo.getMiniapp("MINIAPP_ID")
    .setConfig(
        MiniappConfig.Builder()
            .setCustomActionMenuItem(R.drawable.ic_custom_menu)
            .build()
    )
    .setCustomActionMenuItemClickListener { appboxoActivity, miniapp ->
        // Action menu item clicked
    }
    .open(this)
```

To show/hide custom action menu item use `.showCustomActionMenuItem()` and `.hideCustomActionMenuItem()`
