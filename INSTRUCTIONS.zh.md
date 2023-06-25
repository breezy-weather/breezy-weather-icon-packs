# Instructions to create an icon pack for Breezy Weather

[English instructions](INSTRUCTIONS.md)

## 简介
如同启动器图标包一样，几何天气可以依靠图标扩展包来获取各种风格的天气图标及天气动画，我会在接下来的内容里简短地介绍如果编写一个天气扩展包应用，如果有什么不清楚的地方，请参考几何天气和Pixel全量扩展包这2个项目，或者直接联系我。

---

## Manifest & 配置文件
首先，为了让几何天气能够识别到图标包应用，我们要在`AndroidManifest.xml`中为`MainActivity`的 `<intent-filter>`添加一个`action`：
```
<activity android:name=".main.MainActivity">
    ...
    <intent-filter>
        <action android:name="org.breezyweather.ICON_PROVIDER" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

在这之后，你可以在`res/xml`编写一份文件来标注图标包所提供的资源类目，例如，创建XML文件`icon_provider_config.xml`并添加如下内容：
```
<?xml version="1.0" encoding="utf-8"?>
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="true"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
然后在`AndroidManifest.xml`中声明配置文件：
```
<application>
    ...
    <meta-data
        android:name="org.breezyweather.PROVIDER_CONFIG"
        android:resource="@xml/icon_provider_config" />
    ...
</application>
```
`android:resource`中需要填写你所创建的配置文件的文件名
如果你没有创建配置文件，或者创建了但未在`AndroidManifest.xml`中声明它，几何天气会以下面的内容作为默认配置：
```
<config
    hasWeatherIcons="true"
    hasWeatherAnimators="false"
    hasMinimalIcons="true"
    hasShortcutIcons="true"
    hasSunMoonDrawables="true" />
```
配置中每一项都代表着相应的内容，比如`hasWeatherIcons`表示是否提供天气图标，如果设置为true，几何天气在显示天气图标时会先尝试获取扩展包中的图标资源，如果没有获取成功则使用自带的图标；如果设置为false，则直接使用几何天气自带的图标。如果你没有声明这一项的值，则使用上面的默认值代替。每一项的具体含义我会在后面用到的时候给出。

除了配置文件，你还可以创建过滤器文件，同样的，这些文件也必须在`AndroidManifest.xml`中进行声明：
```
<application>
    ...
    <meta-data
        android:name="org.breezyweather.PROVIDER_CONFIG"
        android:resource="@xml/icon_provider_config" />
    <meta-data
        android:name="org.breezyweather.DRAWABLE_FILTER"
        android:resource="@xml/icon_provider_drawable_filter" />
    <meta-data
        android:name="org.breezyweather.ANIMATOR_FILTER"
        android:resource="@xml/icon_provider_animator_filter" />
    <meta-data
        android:name="org.breezyweather.SHORTCUT_FILTER"
        android:resource="@xml/icon_provider_shortcut_filter" />
    <meta-data
        android:name="org.breezyweather.SUN_MOON_FILTER"
        android:resource="@xml/icon_provider_sun_moon_filter" />
    ...
</application>
```
为了让几何天气顺利地读取扩展包中的资源，图标资源和动画资源都必须按规则进行命名。如果你想要更改文件名，就必须编写相应的过滤器文件，在文件中标注更改后的文件名，并在`AndroidManifest.xml`中声明你创建的过滤器。
* `DRAWABLE_FILTER`: 天气图标、分层图标和minimal图标的图片资源。
* `ANIMATOR_FILTER`: 分层图标的动画资源。
* `SHORTCUT_FILTER`: Shortcut图标。
* `SUN_MOON_FILTER`: 用来绘制太阳和月亮的Drawable类

---

## 天气图标 (256 * 256, PNG格式)
<img width="360" src="pictures/weather_icons.jpg?raw=true"/>

对应配置文件中的`hasWeatherIcons`。
所有的天气图标都应该以`256 * 256`的PNG格式存放在`res/drawable`中，图标的命名规则如下（`weather_xxx`，`xxx`是天气类别，如`clear_day`）：
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
你也可以更改文件名。比如，假设你想将一张图片 `weather_cloudy.png`同时设置为白天和夜间的阴天图标，这时你就需要创建过滤器文件了。
比如我们在`res/xml`中创建名为`icon_provider_drawable_filter.xml`的XML格式文件并添加如下内容：
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    ...
    <item name="weather_cloudy_day" value="weather_cloudy" />
    <item name="weather_cloudy_night" value="weather_cloudy" />
    ...
</resources>
```
这样就相当于声明了`weather_cloudy_day`和`weather_cloudy_night`的真实文件名为`weather_cloudy`。注意：不要再name和value里加上`.png`。
编写好过滤器之后记得要在`AndroidManifest.xml`进行声明：
```
<application>
    ...
    <meta-data
        android:name="org.breezyweather.DRAWABLE_FILTER"
        android:resource="@xml/icon_provider_drawable_filter" />
    ...
</application>
```
如果你不提供过滤器，那么几何天气在读取资源文件时会按照标准文件名来读取，如果此时你恰好没有按照标准格式来命名，或者你压根没有提供天气图标，那么就会出现读取不到的情况，这时会显示几何天气自带的默认图标。

---

## Minimal图标 (256 * 256, PNG格式 / 24dp, XML格式)
<img width="360" src="pictures/minimal_icons.jpg?raw=true"/>

对应配置文件中的`hasMinimalIcons`。
Minimal图标被用于小部件和通知中。对于每一个天气类型，需要4张Minimal图标，例如对于白天阴天的情况，需要：
```
weather_cloudy_day_mini_light.png
weather_cloudy_day_mini_grey.png
weather_cloudy_day_mini_dark.png
weather_cloudy_day_mini_xml.xml
```
分别为亮色图标、灰色图标、深色图标和矢量图标。
为了和小部件、通知中的字体颜色保持一致，建议亮色图标的颜色为`#fafafa`，灰色为`#9e9e9e`，深色为`#424242`，且均为PNG格式，尺寸`256 * 256`。矢量图标推荐使用XML格式，尺寸`24dp`。
Minimal图标的命名规则如下：
```
weather_xxx_mini_light
weather_xxx_mini_grey
weather_xxx_mini_dark
weather_xxx_mini_xml
```
`xxx` 表示天气类型，如`cloudy_day`，全部种类参考天气图标部分。

如果想要更改文件名，和天气图标部分的做法相同，需要编写过滤器，例如在`icon_provider_drawable_filter.xml`中加入：
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
别忘了在`AndroidManifest.xml`中声明过滤器。

---

## 图标动画 (256 * 256, PNG格式 / XML格式的animator文件)
对应配置文件中的`hasWeatherAnimators`。
为了实现天气图标动画，你需要将每个天气图标分割为1-3个图层，并为每个图层创建专属的animator文件（XML格式）。

![](pictures/weather_icon_animator_layers.png?raw=true)

以白天的雷阵雨天气为例，有如下三个图层：
```
weather_thunderstorm_day_1
weather_thunderstorm_day_2
weather_thunderstorm_day_3
```
如果你想更改文件名，还是要创建过滤器文件，例如在`icon_provider_drawable_filter.xml`里添加如下内容：
```
<resources>
    ...
    <item name="weather_thunderstorm_day_1" value="weather_thunderstorm_1" />
    <item name="weather_thunderstorm_day_2" value="weather_thunderstorm_2" />
    <item name="weather_thunderstorm_day_3" value="weather_thunderstorm_3" />
    ...
</resources>
```
别忘了在`AndroidManifest.xml`中声明它。

除了图层外，天气动画文件也要按照规则进行命名：
```
weather_thunderstorm_day_1
weather_thunderstorm_day_2
weather_thunderstorm_day_3
```
所有的天气动画文件都要以XML格式存储在`res/animator`中。
与天气图标相似，如果你想更改文件名，编写过滤器文件（此处不是drawable filter了，而是animator filter）：
1. 在`res/xml`中创建animator过滤器: `icon_provider_animator_filter.xml`.
2. 在`icon_provider_animator_filter.xml`中声明如下代码:
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
3. 在`AndroidManifest.xml`中声明过滤器:
```
<application>
    ...
    <meta-data
        android:name="org.breezyweather.ANIMATOR_FILTER"
        android:resource="@xml/icon_provider_animator_filter" />
    ...
</application>
```

---

## Shortcut图标 (192 * 192, PNG格式 / 768 * 768, PNG格式)
<img width="360" src="pictures/shortcut_icons.jpg?raw=true"/>

这部分对应配置文件中的`hasShortcutIcons`。
每个天气都拥有2个图标，例如：
```
shortcuts_cloudy_day
shortcuts_cloudy_day_foreground
```
* `shortcuts_cloudy_day`：白天的阴天shortcut图标。尺寸是`192 * 192`，整个图标由一个`96 * 96`的天气图标和一个`176 * 176`的圆形背景构成。
* `shortcuts_cloudy_day_foreground`：白天的阴天shortcut自适应图标。尺寸是`768 * 768`，由一个`256 * 256`的天气图标构成，背景为全填充的矩形。

图片文件的命名按照`shortcuts_xxx`和`shortcuts_xxx_foreground`的规则，`xxx`是天气种类。
如果你想要更改文件名，需要创建shortcut过滤器，例如，在`res/xml`创建`icon_provider_shortcut_filter.xml`并在`AndroidManifest.xml`中声明该文件：
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
        android:name="org.breezyweather.SHORTCUT_FILTER"
        android:resource="@xml/icon_provider_shortcut_filter" />
    ...
</application>
```

---

## 太阳Drawable & 月亮Drawable (java类)
<img width="360" src="pictures/sun_moon_drawables.jpg?raw=true"/>

对应配置文件中的`hasSunMoonDrawables`。
如果要提供单独的太阳drawable和月亮drawable，你需要创建相对应的java类，并通过canvas API来绘制太阳和月亮的形状。这样相比直接绘制bitmap，有着更高的运行效率。
当然，这并不是必须的，如果你不擅长编码，也可以忽略这部分内容，几何天气会自动读取`weather_clear_day`和`weather_clear_night`作为太阳和月亮图标。
需要注意的是，如果你确定要编写drawable类，则一定要创建过滤器来声明drawable的位置：
1. 在`res/xml`中创建`icon_provider_sun_moon_filter.xml`。
2. 添加如下代码(value中要写你创建的drawable的位置):
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="sun" value="org.breezyweather.ui.image.SunDrawable" />
    <item name="moon" value="org.breezyweather.ui.image.MoonDrawable" />
</resources>
```
3. 在`AndroidManifest.xml`中声明sun moon过滤器：
```
<application>
    ...
    <meta-data
        android:name="org.breezyweather.SUN_MOON_FILTER"
        android:resource="@xml/icon_provider_sun_moon_filter" />
    ...
</application>
```

---

## 最佳实践

天气图标动画和Minimal图标会给开发者带来较大的工作量。因此建议重点进行天气图标、shortcut图标的制作，其他内容请量力而行。
