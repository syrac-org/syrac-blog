---
date: 2025-10-30T16:00:00+01:00
title: 2025 October Update
summary: Track preview, editor improvements, and more.
tags:
  - updates
cover: terrain.png
---

## Track preview

Preview user tracks by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-eye-icon lucide-eye"><path d="M2.062 12.348a1 1 0 0 1 0-.696 10.75 10.75 0 0 1 19.876 0 1 1 0 0 1 0 .696 10.75 10.75 0 0 1-19.876 0"/><circle cx="12" cy="12" r="3"/></svg>
{{</ raw >}}
icon in a task leaderboard.
This allows you to quickly see the different routes pilots have taken to complete a task.
This feature will be improved in the coming weeks, so that you can replay tracks, with automatic synchronization at the Start of Speed Section (SSS).

{{< image src="track_preview.png" alt="Track preview" position="center" style="border-radius: 4px;" >}}

## Track deletion

Again on the leaderboard page, you can now delete your own tracks by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-trash2-icon lucide-trash-2"><path d="M10 11v6"/><path d="M14 11v6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>
{{</ raw >}}
icon (next to the track preview icon).
You can also delete your tracks from your activity feed in your profile page.

## XContest integration

When displaying a task QR code, the dialog now contains the numeric code which can be used to download tasks in XCTrack from the cloud.
You can also use the link next to the numeric code to open the task in [XContest tools](https://tools.xcontest.org/xctsk).

{{< image src="numeric_code.png" alt="XContest integraton" position="center" style="border-radius: 4px;" >}}

## Custom terrain maps

Have you noticed a lot of changes in the map styles these last few weeks ?
Thanks to the awesome work of [wipfli](https://github.com/wipfli) on [Mapterhorn](https://mapterhorn.com/) and [hyperknot](https://hyperknot.com/) on [OpenFreeMap](https://openfreemap.org/), we've now got nice-looking terrain maps using open data displayed by open-source software. This is the future.

{{< image src="terrain.png" alt="Custom terrain map styles" position="center" style="border-radius: 4px;" >}}

## Edit history

You can now use the `Undo` (
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-undo-icon lucide-undo"><path d="M3 7v6h6"/><path d="M21 17a9 9 0 0 0-9-9 9 9 0 0 0-6 2.3L3 13"/></svg>
{{</ raw >}}
) and `Redo` (
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-redo-icon lucide-redo"><path d="M21 7v6h-6"/><path d="M3 17a9 9 0 0 1 9-9 9 9 0 0 1 6 2.3l3 2.7"/></svg>
{{</ raw >}}
) buttons in the task editor to navigate your editing history.

## Turnpoint handles

Placing turnpoints precisely in the task editor used to be tricky, because there was no indicators showing the exact location of their center.
There is now a 5m-wide hexagon to help you drag their center exactly where you want, which is particularly useful at high zoom levels.

## Skyways

M. von Känel agreed for Syrac to use its nice [thermal maps](https://thermal.kk7.ch/).
Only the skyways have been added to the task editor for now, but the thermal layer is on the way!
You can adjust the layer opacity by using the slider in the map settings.

{{< image src="skyways.png" alt="Skyways" position="center" style="border-radius: 4px;" >}}

## Geolocation

Quickly center the explore map and the task editor map on your current location by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-locate-fixed-icon lucide-locate-fixed"><line x1="2" x2="5" y1="12" y2="12"/><line x1="19" x2="22" y1="12" y2="12"/><line x1="12" x2="12" y1="2" y2="5"/><line x1="12" x2="12" y1="19" y2="22"/><circle cx="12" cy="12" r="7"/><circle cx="12" cy="12" r="3"/></svg>
{{</ raw >}}
icon.

This was originally done automatically but resulted in browsers asking for user geolocation consent unconditionally, even for users who did not whish to use this feature, which was not a nice experience.

## Leaderboard sharing

Want to easily share a task leaderboard page with your friends ?
You can now copy the URL directly from the app, or display a printable QR code leading to the page.
Being able to copy the URL is particularly useful if you installed Syrac as an app, because unlike in a native browser, installed apps hide the URL bar.

{{< image src="leaderboard_sharing.png" alt="Leaderboard sharing" position="center" style="border-radius: 4px;" >}}

Use it as a sticker on your local site information board if you like!

## Landing page

We've finally got a [landing page](http://syrac.org/)!
Its purpose is to showcase Syrac features broadly, without requiring users to set up an account just to check them out.
