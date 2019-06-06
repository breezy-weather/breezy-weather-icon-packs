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
After creating the XML file, you need to declear this file in `AndroidManifest.xml`. For example:
```
<application>
    ...
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
Weather icons are the most important content in the icon provider, corresponds to `hasWeatherIcons` in configuration file. If you set that `true`, GeometricWeather will try to read weather icons provided by icon provider, otherwise display default icons directly.
You need to put all the weather icon image resources into `res/drawable` in PNG format. And every image should be named according to the regulation:
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
You can also name them according to your own ideas. For example, if I want to use an image as both daytime icon and nighttime icon for cloudy weather, you can create an XML file in `res/xml`. Let's assume the file name is `icon_provider_drawable_filter.xml`. Add  following content to the file:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    ...
    <item name="weather_cloudy_day" value="weather_cloudy" />
    <item name="weather_cloudy_night" value="weather_cloudy" />
    ...
</resources>
```
You need to fill in the standard file name in `name`, fill in the real file name in `value`. (without `.png`)
After creating the file, you also need to declare this file in `AndroidManifest.xml`, For example:
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.DRAWABLE_FILTER"
        android:resource="@xml/icon_provider_drawable_filter" />
    ...
</application>
```
Of course, you can also not provide weather icons. GeometricWeather will display default icons.

---

### Minimal Icons
The minimal icons are used in app widget and notification. Every weather need 4 minimal icon images. For example, here are the names of minimal icons' file name corresponding to daytime cloudy:
```
weather_cloudy_day_mini_light
weather_cloudy_day_mini_grey
weather_cloudy_day_mini_dark
weather_cloudy_day_mini_xml
```
The light icons, grey icons and dark icons need to be stored in `res/drawable` in PNG format. And It is recommended to use the vector icon in XML format as the xml icon. Every image should be named according to the regulation:
```
weather_xxx_day_mini_light
weather_xxx_day_mini_grey
weather_xxx_day_mini_dark
weather_xxx_day_mini_xml
```
`xxx` means weather type. The value of weather type as same as weather icons.
In order to match the font color of app widget and notification, the color of light icons should be `#fafafa`, the color of grey icons should be `#9e9e9e`, the color of dark icons should be `#424242`.
As same as weather icons, if you want to change the file name, you need to add the following content to `icon_provider_drawable_filter.xml`:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    ...
    <item name="weather_cloudy_day_mini_light" value="weather_cloudy_mini_light" />
    <item name="weather_cloudy_day_mini_grey" value="weather_cloudy_mini_grey" />
    <item name="weather_cloudy_day_mini_dark" value="weather_cloudy_mini_dark" />
    <item name="weather_cloudy_day_mini_xml" value="weather_cloudy_mini_xml" />
    ...
</resources>
```
And don't forget to declear this file in `AndroidManifest.xml`.

---

### Animators For Weahter Icons

---

### Shortcut Icons

---

### Sun Drawable & Moon Drawable
