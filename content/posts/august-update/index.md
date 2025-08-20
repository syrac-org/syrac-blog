---
date: 2025-08-20T18:30:00+02:00
draft: true
title: August update
summary: One month of Syrac.
katex: true
tags:
  - updates
cover: banner.png
---

It's been a month since Syrac was released, and there was a lot of positive feedback and [discussions](https://github.com/orgs/syrac-org/discussions?discussions_q=+).

Here is a short summary of the updates since the initial release:

## Glider categories

Glider categories are now displayed in the task leaderboard, and need to be declared when submitting tracks.

{{< image src="glider_categories.png" alt="Glider category in track submission form" position="center" style="border-radius: 4px;" width="300" >}}

## Completion speed

Another addition to task leaderboards is the completion speed, calculated as the optimized task distance between the Start of Speed Section (SSS) and the End of Speed Section (ESS) divided by the flight time between these two turnpoints.

{{< image src="leaderboard.png" alt="Leaderboard with glider category and completion speed" position="center" style="border-radius: 4px;" >}}

Since the speed section distance $d_{SS}$ can be significantly shorter than the total task distance $d_{TOT}$, the task detail page now displays both distances using $d_{TOT}$ km [$d_{SS}$ km].

## Task clusters

The explore map is now less empty by displaying clusters of tasks in view.

{{< image src="task_clusters.png" alt="Task clusters in explore map" position="center" style="border-radius: 4px;" >}}

Once the map is zoomed-in enough to resolve individual tasks, you can click on it to display a popup with a link to the leaderboard page.

{{< image src="map_popup.png" alt="Task popup in explore map" position="center" style="border-radius: 4px;" >}}

## Persistent map state

Another nice-to-have feature of the explore map is that it now remembers its position during a browsing session. This means you can navigate back and forth between the explore page and task detail pages without the map location resetting on each page load.

## Leaderboard date filter

You can now filter leaderboard results by date range. This enables pilots to run monthly, yearly, or seasonal challenges, while keeping tasks open for completion at any time.

## Task search

Want to quickly find a task you know the name of ? Use the search bar on the explore page to open the task preview.

{{< video src="task_search.mp4" width="100%" controls="true" >}}
