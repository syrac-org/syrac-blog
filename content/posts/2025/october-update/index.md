---
date: 2025-10-30T16:00:00+01:00
draft: true
title: 2025 October Update
summary: Track preview, editor improvements, and more.
tags:
  - updates
---

## Track preview

Preview user tracks by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-eye-icon lucide-eye"><path d="M2.062 12.348a1 1 0 0 1 0-.696 10.75 10.75 0 0 1 19.876 0 1 1 0 0 1 0 .696 10.75 10.75 0 0 1-19.876 0"/><circle cx="12" cy="12" r="3"/></svg>
{{</ raw >}}
icon in the leaderboard.
This allows you to quickly see the different routes used to complete a task.
This feature will be improved in the coming weeks, so that you can replay the tracks, with automatic synchronization at the Start of Speed Section (SSS).

## XContest integration

The task dialog now contains the numeric code which can be used to download tasks in XCTrack from the cloud.
You can also use the link next to the numeric code to open a task in [XContest tools](https://tools.xcontest.org/xctsk).

TODO QR code example image.

## Track deletion

You can now delete your own tracks, either from your profile page or directly from the leaderboard by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-trash2-icon lucide-trash-2"><path d="M10 11v6"/><path d="M14 11v6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>
{{</ raw >}}
icon.

TODO delete track button.

## Task page QR code

Want to easily share a task leaderboard page with your friends ?
You can now copy the URL directly from the app, or display a printable QR code leading to the page.

TODO leaderboard QR code image.

Use it as a sticker on your local site information board if you like!

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

Placing turnpoints precisely in the task editor used to be tricky, because there was no indicators showing the location of their center.
There is now a 5m-wide hexagon to help you drag their center exactly where you want, which is particularly useful at high zoom levels.

## Skyways

M. von Känel agreed for Syrac to use its nice [thermal maps](https://thermal.kk7.ch/).
Only the skyways have been added to the task editor for now, but the thermal layer is on the way!

## Geolocation

Quickly center the explore map and the task editor map on your current location by clicking on the
{{< raw >}}
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-locate-fixed-icon lucide-locate-fixed"><line x1="2" x2="5" y1="12" y2="12"/><line x1="19" x2="22" y1="12" y2="12"/><line x1="12" x2="12" y1="2" y2="5"/><line x1="12" x2="12" y1="19" y2="22"/><circle cx="12" cy="12" r="7"/><circle cx="12" cy="12" r="3"/></svg>
{{</ raw >}}
icon.

This was originally done automatically but resulted in browsers asking for user geolocation consent without users requesting this feature, which was not a nice experience.

## Landing page

We've finally got a [landing page](http://syrac.org/)!
Its purpose is to showcase Syrac features publicly, without requiring users to set up an account just to check them out.
