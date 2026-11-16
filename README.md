<h1 align="center">Namaz Vakti</h1>
<p align="center">A small prayer-times widget for macOS and iOS that shows the current prayer and the time left.</p>

<p align="center">
<img src="https://img.shields.io/badge/ScriptWidget-JSX-555?style=flat" alt="ScriptWidget">
<img src="https://img.shields.io/badge/method-Diyanet-555?style=flat" alt="Diyanet method">
<img src="https://img.shields.io/badge/API%20key-none-555?style=flat" alt="no API key">
<a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-555?style=flat" alt="MIT license"></a>
</p>

<p align="center"><b><a href="https://raw.githubusercontent.com/AlparslanSemiz/prayer-times-widget/main/namaz-vakti-mac.jsx">Download namaz-vakti-mac.jsx</a></b> and paste it into <a href="https://apps.apple.com/app/scriptwidget/id1555600758">ScriptWidget</a></p>

## Why

Prayer-time apps are heavy for a question you ask many times a day: which prayer is it and how long until the next one? This is one script file for [ScriptWidget](https://apps.apple.com/app/scriptwidget/id1555600758). It finds your location, uses the Diyanet calculation method and goes online once a day at most.

## Features

- **Current prayer and time left**, for example `İkindi · 2sa 41dk`. The widget text is Turkish.
- **Diyanet method:** times come from the [Aladhan API](https://aladhan.com/prayer-times-api) with `method=13` (Diyanet İşleri Başkanlığı).
- **Automatic location:** `$location` (CoreLocation) on iOS. If that is unavailable, an approximate location from the IP address.
- **Few network calls:** the day's times are cached in `$storage` and refreshed when the day changes or you move more than about 20 km.
- **Works offline:** if the network fails, the last cached times are shown; with no cache yet, the widget shows `--:--` placeholders and a warning instead of crashing, and recovers once you are online.
- **Readable everywhere:** blue accents and grey text work in light and dark mode, on iPhone and on the Mac desktop.
- **Kerahat warning:** a `⚠️` mark during the kerahat times.

## Install

1. Install **ScriptWidget** from the App Store.
2. Create a new script in the app and paste the contents of [`namaz-vakti-mac.jsx`](namaz-vakti-mac.jsx).
3. iOS: run the script once inside ScriptWidget and allow location access ("While Using the App").
4. Add a ScriptWidget widget to the Home Screen or desktop and pick this script.

On macOS, ScriptWidget does not support location services ("Location services are not available on this platform"), so the widget uses the IP location there. Pin a city with `SABIT_KONUM` if you prefer.

Without location permission the widget uses the IP location. If that fails too, it uses the `FALLBACK` coordinates in the file.

## Settings

Constants at the top of the file:

- `SABIT_KONUM`: pin the location, for example `{ lat: 41.0082, lon: 28.9784, city: "İstanbul" }`. When set, neither location permission nor IP location is used. Default `null`.
- `FALLBACK`: coordinates used when no location source works. Default: Karlsruhe.
- `METHOD`: calculation method. `13` is Diyanet; see the Aladhan docs for others.

<details>
<summary>How it works</summary>

1. Coordinates come from `$location` (city-level accuracy is enough). If that fails: approximate IP location from [ipwho.is](https://ipwho.is), then the last cached location, then `FALLBACK`.
2. [BigDataCloud](https://www.bigdatacloud.com/) reverse geocoding turns the coordinates into a city name, shown in the title only.
3. Today's times are fetched from the Aladhan API.
4. The result is stored in `$storage` with the date and location. On the next run with the same day and roughly the same place, nothing is fetched and only the time left is recalculated locally.

</details>

## Status

The time shown is the moment the widget last refreshed. iOS and macOS refresh widgets on their own schedule, so a delay of a few minutes is normal. None of the APIs needs a key, and all are free.

## Tech

JSX for ScriptWidget, Aladhan API, ipwho.is, BigDataCloud.

## License

[MIT](LICENSE)
