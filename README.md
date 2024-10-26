## Appboxo Android SDK

### Getting started

Please see our [sample Android app](https://github.com/Appboxo/sample-android-hostapp) to learn more.

Add the below code to your app `build.gradle` file (not your root build.gradle file).

```gradle
dependencies {
    implementation 'com.appboxo:sdk:x.x.x'
}
```

Add this in your root `build.gradle` file (**not** your module `build.gradle` file):

```gradle
allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
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


Init Appboxo SDK in your Application class.

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
                    .debug(BuildConfig.DEBUG) //by default 'false', enables webview debugging
                    .sandboxMode(true) //by default 'false'
                    .permissionsPage(false) // use it to hide "Settings" from Miniapp menu, by default 'true'
                    .showClearCache(false) // use it to hide "Clear cache" from Miniapp menu, by default 'true'
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
                        .debug(BuildConfig.DEBUG) //by default 'false', enables webview debugging
                        .sandboxMode(true) //by default 'false'
                        .build())
                .setLogger(new DefaultLogger(BuildConfig.DEBUG));
    }
}
```

To open miniapps, write in your Activity:

```kotlin
// Kotlin

val miniapp = Appboxo.getMiniapp(appId)
miniapp.open(context)
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
    
    override fun onUserInteraction(miniapp: Miniapp) {
        // Called whenever touch event is dispatched to the miniapp activity.
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
    
    @Override
    public void onUserInteraction(@NotNull Miniapp miniapp) {

    }
});
```
### Auth flow

```kotlin
miniapp.setAuthListener { appboxoActivity, miniapp ->
    //get AuthCode from hostapp backend and send it to miniapp
    miniapp.setAuthCode(authCode)
}
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
### Handle custom event from miniapp:

```kotlin
//kotlin

miniApp.setCustomEventListener { miniAppActivity, miniApp, customEvent ->
    //doSomething
    customEvent.payload = mapOf("message" to "text",
                                "id" to 123,
                                "checked" to true)
    miniApp.sendEvent(customEvent)
}
```

```java
//java

miniApp.setCustomEventListener(new Miniapp.CustomEventListener() {
    @Override
    public void handle(@NotNull Activity miniAppActivity, @NotNull Miniapp miniapp, @NotNull CustomEvent customEvent) {
        Map<String, Object> payload = new HashMap<>();
        payload.put("message", "message");
        payload.put("id", 123);
        payload.put("checked", true);
        customEvent.setPayload(payload);
        miniApp.sendEvent(customEvent);
    }
});
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

### Get miniapp list
```kotlin
Appboxo.getMiniapps(object: MiniappListCallback{
            override fun onFailure(e: Exception) {
                ...
            }

            override fun onSuccess(miniapps: List<MiniappData>) {
                ...
            }
        })
```
