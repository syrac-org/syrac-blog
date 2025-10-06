---
date: 2025-10-04T00:37:23+02:00
draft: true
title: The Day I Hacked XCTrack
summary: A tale of trust and public-key cryptography.
tags:
  - dev
---

In the [previous blog post](../2025/september-update/), I mentioned a new feature in task leaderboards: track signature icons.

{{< raw >}}
<div style="display: flex; justify-content: center; gap: 2rem; flex-wrap: wrap;">
    <div title="Unknown track signature">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="oklch(71.526% 0.1356 56.892)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-shield-question-mark-icon lucide-shield-question-mark"><path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/><path d="M9.1 9a3 3 0 0 1 5.82 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg>
    </div>
    <div title="Valid track signature">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="oklch(0.6231 0.188 259.8145)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-shield-check-icon lucide-shield-check"><path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/><path d="m9 12 2 2 4-4"/></svg>
    </div>
    <div title="Invalid track signature">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="oklch(0.6368 0.2078 25.3313)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-shield-x-icon lucide-shield-x"><path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/><path d="m14.5 9.5-5 5"/><path d="m9.5 9.5 5 5"/></svg>
    </div>
</div>
{{< /raw >}}

There were a few questions about what these icons meant.
In this post, I'll explain what they are and how they're related to the day I hacked XCTrack.
This is going to be a nerdy post, so buckle up.

## The IGC file format

In paragliding, the IGC file format is the current standard for storing and exchanging flight information.
This is what most flight recorders are configured to write, and what Syrac uses to score tracklogs on tasks.

IGC files are plain text files containing timestamped coordinates, along with other flight metadata.
The first character of each line indicates the type of record stored in that line:

- `H`: header records storing metadata such as date, pilot name and glider model
- `B`: fix records storing coordinates of the flight
- `G`: security record storing the digital track signature
- [a lot of other record types](https://xp-soaring.github.io/igc_file_format/igc_format_2008.html), which are not relevant to this post

You can modify a text file simply by opening it in a file editor, so what's preventing malicious users from cheating online contests by crafting arbitrary tracklogs ?

One could argue that doing so would be pointless, because paragliding remains an amateur sport where the incentives to cheat are relatively low.
Even if this is somewhat true, it's safe to assume that as soon as there is a leaderboard, [someone is going to try to rig it](https://web.archive.org/web/20250714025458/https://xcmag.com/news/comps-and-events/xcontest-bans-pilot-for-faking-tracklogs), even if the motivations to do so are not entirely clear.

This brings me to my main point:

> For Syrac to be a trusted platform, it needs promote transparency in its leaderboards.

And digital track signatures is one way to encourage this transparency.

So, how do they work ?

## Public-key cryptography

If you're already familiar with hashing and public-key cryptography concepts, you can skip to the [next section](#in-practice).
Otherwise, this section briefly explains the two core concepts required to understand the underlying mechanics of how track signatures work.

### Hashing

For the purposes of this post, you can think of a [hash](https://en.wikipedia.org/wiki/Hash_function) as a short identifier of the file contents, similar to a fingerprint.
Any change to the file contents changes the hash.

### Digital signature

A [digital signature](https://en.wikipedia.org/wiki/Digital_signature) is a way to to verify the authenticity of a piece of data.
This process requires a pair of keys: a private key, **which should be secret and only known to the signer**, and a public key, which can be shared with anyone.

First, the private key is used to sign a piece of data by encrypting it, creating a digital signature.
Then, anyone with the public key can decrypt the signature, confirming that the data comes from the signer and hasn't been tampered with.

## In practice

Once you land, your flight recorder computes a hash of the IGC file contents, and signs it with its private key.
This signature is appended to the IGC file using `G` records.

To check the validity of a signature, the flight recorder public key can be used to decrypt the encrypted hash, and checking that it matches the hash of the original file contents.

{{< image src="digital_signature.png" alt="Digital signature workflow" position="center" style="border-radius: 4px;" >}}

## The hack

## Spoofing GPS signals
