# IconProvider For GeometricWeather
A document about icon provider for GeometricWeather

---

### Introduction
This is a document about icon provider for GeometricWeather. Just like an icon pack for Android Launcher, It could provide weather icons and animators for GeometricWeather.
Now, I will describe in detail how to build an icon provider for GeometricWeather.

---

### Manifest & Config
First of all, in order for the GeometricWeather to identify the icon provider, you need to add the following action to `<intent-filter>` of the `MainActivity` in `AndroidManifest.xml`.
```
<activity android:name=".main.MainActivity">
    ...
    <intent-filter>
        <action android:name="com.wangdaye.geometricweather.ICON_PROVIDER" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```
After adding action, you need to create an XML file in `res/xml` to declare what resources will be provided. For exmaple, the file name is `icon_provider_config.xml` and the code is as follows:
```
<?xml version="1.0" encoding="utf-8"?>
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="true"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
After creating the XML file, you need to tag this file in `AndroidManifest.xml`. For example:
```
<application>
    <meta-data
        android:name="com.wangdaye.geometricweather.PROVIDER_CONFIG"
        android:resource="@xml/icon_provider_config" />
    ...
</application>
```
And be careful, the name in `android:resource` should as same as the file you just created in `res/xml`.
If you didn't create this file, or if you forgot to declare it in `AndroidManifest.xml`, GeometricWeather will use the following code as the default:
```
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="false"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
And I will explain the meaning of those items in following content.

---

### Weather Icons
Weather icons are the most important content in the icon provider, corresponds to `hasWeatherIcons` in configuration file.
You need to put all the weather icon image resources into `res/drawable` in PNG format. And every photo should be named according to the regulation:
```
weather_clear_day
weather_clear_night
weather_partly_cloudy_day
weather_partly_cloudy_night
weather_cloudy_day
weather_cloudy_night
weather_rain_day
weather_rain_night
weather_snow_day
weather_snow_night
weather_wind_day
weather_wind_night
weather_fog_day
weather_fog_night
weather_haze_day
weather_haze_night
weather_sleet_day
weather_sleet_night
weather_hail_day
weather_hail_night
weather_thunder_day
weather_thunder_night
weather_thunderstorm_day
weather_thunderstorm_night
```

---

### Minimal Icons

---

### Animators For Weahter Icons

---

### Shortcut Icons

---

### Sun Drawable & Moon Drawable
