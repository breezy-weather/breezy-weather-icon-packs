# IconProvider For GeometricWeather

---

### Introduction
This is a document about icon provider for GeometricWeather. Just like an icon pack for Android Launcher, It could provide weather icons and animators for GeometricWeather.
Now, I will describe in detail how to build an icon provider for GeometricWeather. If there is anything unclear, please refer to these 2 project: GeometricWeather and FullSizePixelIconProvider, or just cantact me.

---

### Manifest & Config
First of all, in order for GeometricWeather to identify the icon provider, you need to add the following action to `<intent-filter>` of the `MainActivity` in `AndroidManifest.xml`.
```
<activity android:name=".main.MainActivity">
    ...
    <intent-filter>
        <action android:name="com.wangdaye.geometricweather.ICON_PROVIDER" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```
After adding action, you need to create an configuration file in `res/xml` to declare what resources will be provided. For exmaple, the file name is `icon_provider_config.xml` and the code is as follows:
```
<?xml version="1.0" encoding="utf-8"?>
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="true"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
After creating the XML file, you need to declear this file in `AndroidManifest.xml`:
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
If you don't create this file, or if you forget to declare it in `AndroidManifest.xml`, GeometricWeather will use the following code as the default:
```
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="false"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
Each item in configuration file corresponds to a specific type of resources. If you set it true, GeometricWeather will try to read resources provided by icon provider. Conversely, if you set it false, GeometricWeather will display its own resources directly. If you don't declare the value of an item, that item will be set to the default value. I will explain the meaning of those items in following content.

In addition to the configuration file, you can also create the following files and add them to `AndroidManifest.xml`:
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.PROVIDER_CONFIG"
        android:resource="@xml/icon_provider_config" />
    <meta-data
        android:name="com.wangdaye.geometricweather.DRAWABLE_FILTER"
        android:resource="@xml/icon_provider_drawable_filter" />
    <meta-data
        android:name="com.wangdaye.geometricweather.ANIMATOR_FILTER"
        android:resource="@xml/icon_provider_animator_filter" />
    <meta-data
        android:name="com.wangdaye.geometricweather.SHORTCUT_FILTER"
        android:resource="@xml/icon_provider_shortcut_filter" />
    <meta-data
        android:name="com.wangdaye.geometricweather.SUN_MOON_FILTER"
        android:resource="@xml/icon_provider_sun_moon_filter" />
    ...
</application>
```
All image resources and animator resources in icon provider need to be named according to specific regulation, so that GeometricWeather can load these resources correctly. If you want to change the file name, you need to create the corresponding filter file to mark their file names.
* `DRAWABLE_FILTER`: Image resources for weather icons, layered weather icons and minimal icons.
* `ANIMATOR_FILTER`: Animator resources for layered weather icons.
* `SHORTCUT_FILTER`: Image resources for shortcut icons.
* `SUN_MOON_FILTER`: Sun drawable and moon drawable. (Java code)

---

### Weather Icons (256 * 256, PNG format)
These resources correspond to `hasWeatherIcons` in configuration file.
You need to put all the weather icon resources into `res/drawable` in PNG format. Image size should be `256 * 256`. And every image should be named according to the following regulation:
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
You can also name them according to your own ideas. For example, if I want to set an image `weather_cloudy.png` as both daytime icon and nighttime icon for cloudy weather, you can create a drawable filter `icon_provider_drawable_filter.xml` in `res/xml` and declear the file names through adding following content:
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
After creating the file, you also need to declare this file in `AndroidManifest.xml`:
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.DRAWABLE_FILTER"
        android:resource="@xml/icon_provider_drawable_filter" />
    ...
</application>
```
Of course, if icon provider don't provide any weather icons. GeometricWeather will display the default icons.

---

### Minimal Icons (256 * 256, PNG format / 24dp, XML format)
These resources correspond to `hasMinimalIcons` in configuration file.
The minimal icons are used in app widget and notification. Every weather need 4 minimal icon images. For example, here are the minimal icons corresponding to daytime cloudy:
```
weather_cloudy_day_mini_light.png
weather_cloudy_day_mini_grey.png
weather_cloudy_day_mini_dark.png
weather_cloudy_day_mini_xml.xml
```
The light icons, grey icons and dark icons need to be stored in `res/drawable` in PNG format, thier size should be `256 * 256`. And It is recommended to use `24dp` vector icons in XML format as the xml icon. Every image should be named according to the regulation:
```
weather_xxx_day_mini_light
weather_xxx_day_mini_grey
weather_xxx_day_mini_dark
weather_xxx_day_mini_xml
```
`xxx` means weather type. The value of weather type as same as weather icons.

In order to match the font color of app widget and notification, the color of light icons should be `#fafafa`, the color of grey icons should be `#9e9e9e`, the color of dark icons should be `#424242`.

If you want to change the file name, you need to add the following content to `icon_provider_drawable_filter.xml`:
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

### Animators For Weahter Icons (256 * 256, PNG format / XML animator file)
These resources correspond to `hasWeatherAnimators` in configuration file.
To achieve animated weather icons, you need to split each weather icon into 1-3 layers(`256 * 256`) and create XML format animator file for each layer. 
![](https://github.com/WangDaYeeeeee/IconProvider-For-GeometricWeather/blob/master/pictures/weather_icon_animator_layers.png)

As the daytime thunderstorm example, the layer icons' naming format is as follows:
```
weather_thunderstorm_day_1
weather_thunderstorm_day_2
weather_thunderstorm_day_3
```
If you want to change the file name, you need to declare the real names in `icon_provider_drawable_filter.xml`:
```
<resources>
    ...
    <item name="weather_thunderstorm_day_1" value="weather_thunderstorm_1" />
    <item name="weather_thunderstorm_day_2" value="weather_thunderstorm_2" />
    <item name="weather_thunderstorm_day_3" value="weather_thunderstorm_3" />
    ...
</resources>
```
And don't forget to declear this file in `AndroidManifest.xml`.

In addition to image resources, animator resources also need to be named as follows:
```
weather_thunderstorm_day_1
weather_thunderstorm_day_2
weather_thunderstorm_day_3
```
All of the animator resources should be stored in `res/animator` with XML format.
Similar to weather icons, if you want to change the file name, you should:
1. Create a animator filter in `res/xml`, for exmaple: `icon_provider_animator_filter.xml`.
2. Declear the real file names in `icon_provider_animator_filter.xml`:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    ...
    <item name="weather_thunderstorm_day_1" value="weather_thunderstorm_1" />
    <item name="weather_thunderstorm_day_2" value="weather_thunderstorm_2" />
    <item name="weather_thunderstorm_day_3" value="weather_thunderstorm_3" />

    <item name="weather_thunderstorm_night_1" value="weather_thunderstorm_1" />
    <item name="weather_thunderstorm_night_2" value="weather_thunderstorm_2" />
    <item name="weather_thunderstorm_night_3" value="weather_thunderstorm_3" />
    ...
</resources>
```
3. Declear this animator filter in `AndroidManifest.xml`:
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.ANIMATOR_FILTER"
        android:resource="@xml/icon_provider_animator_filter" />
    ...
</application>
```

---

### Shortcut Icons (192 * 192, PNG format / 768 * 768, PNG format)
These resources correspond to `hasShortcutIcons` in configuration file.
If you want provide shortcut icons, you should provider 2 image resources for each weather, for example:
```
shortcuts_cloudy_day
shortcuts_cloudy_day_foreground
```
* `shortcuts_cloudy_day`: shortcut icon for daytime cloudy. Its size is `192 * 192`, and its foreground is a `96 * 96` weather icon, its background is a circle (`176 * 176`).
* `shortcuts_cloudy_day_foreground`: foreground of shortcut adaptive icon for daytime cloudy. Its size is `768 * 768`, and its foreground is a `256 * 256` weather icon.

If you want to change the file name of shortcut icons, you should create a shortcut filter in `res/xml` and declear this file in `AndroidManifest.xml`:
```
<resources>
    ...
    <item name="shortcuts_cloudy_day" value="shortcuts_cloudy" />
    <item name="shortcuts_cloudy_day_foreground" value="shortcuts_cloudy_foreground" />
    ...
</resources>
```
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.SHORTCUT_FILTER"
        android:resource="@xml/icon_provider_shortcut_filter" />
    ...
</application>
```

---

### Sun Drawable & Moon Drawable (java class)
These resources correspond to `hasSunMoonDrawables` in configuration file.
You need to create sun drawable and moon drawable with java code because drawing shapes on the canvas has better performance than drawing bitmap directly. 
However, if you aren't good at coding, you can also ignore this part, GeometricWeather will use `weather_clear_day` and `weather_clear_night` as the sun icon and moon icon.
And be careful, if you want to provide the drawable with java class, you must declare those 2 drawables in sun moon filter:
1. Create the filter `icon_provider_sun_moon_filter.xml` in `res/xml`.
2. Add the following code (replace the contents of value with the the drawable in your icon provider):
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="sun" value="wangdaye.com.geometricweather.ui.image.SunDrawable" />
    <item name="moon" value="wangdaye.com.geometricweather.ui.image.MoonDrawable" />
</resources>
```
3. Declear this file in `AndroidManifest.xml`:
```
<application>
    ...
    <meta-data
        android:name="com.wangdaye.geometricweather.SUN_MOON_FILTER"
        android:resource="@xml/icon_provider_sun_moon_filter" />
    ...
</application>
```
