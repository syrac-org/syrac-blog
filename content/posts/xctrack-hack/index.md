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

A [digital signature](https://en.wikipedia.org/wiki/Digital_signature) is a way to verify the authenticity of a piece of data.
This process requires a pair of keys: a private key, **which should be secret and only known to the signer**, and a public key, which can be shared with anyone.

First, the private key is used to sign a piece of data, creating a digital signature.
Then, anyone with the public key can verify the signature, confirming that the data comes from the signer and hasn't been tampered with.

## In practice

Once you land, your flight recorder computes a hash of the IGC file contents, and signs it with its private key.
This signature is appended to the IGC file using `G` records.

To check the validity of a signature, the flight recorder public key can be used to decrypt the encrypted hash, and checking that it matches the hash of the original file contents.

{{< image src="digital_signature.png" alt="Digital signature workflow" position="center" style="border-radius: 4px;" >}}

This whole process relies on the computational infeasibility for someone without access to the private key to create a valid signature.

## The hack

So, if the private key is supposed to be a secret, how is the signature of the following (doubtful) IGC file valid ?

```text
AXCT XCTrack
So long, and thanks for all the fish.
G3C9D43B54B7BB763AD8201A6C044500B4F90E2768E36DA06CEFC283F59DB8556
G0AC71E4A6F120E4CD925A4B7BFDFD06968DDBE96D0C1224A07398FC4285C5861
G30430FA72EAB9F3BCD5AFE853D7C8006FF50EDAA18DDF96E28CA81B67B7F61EB
GEB1239B4545CA16002EE843F57C6BD970702994C2FDEBC4FBD5A00D03C0B7AE7
```

You can verify its validity on your own by uploading [the IGC file](./hacked.igc) to the [FAI Open Validation Server](http://vali.fai-civl.org/validation.html).

When I started thinking about Syrac back in 2020, I wondered:

> If XCTrack is able to create valid signatures directly from my phone, that means the private key is somewhere inside the application.
> How hard would it be to find it, and reverse-engineer the signature process to craft valid tracks ?

So I decompiled the APK installed on my phone using [`apktool`](https://apktool.org/), and started looking for clues.
The app bytecode is decompiled to `.smali` files, which is an assembly-like language for Android.
It's not very readable, so I transpiled it to Java — which is not much more readable, but at least I understood what I was looking at.

From this point, it was a reverse-engineering game of figuring out how tracklogs were written, and how security records were appended to it.

Even if function and variable names were obfuscated, the Java classes were not and were pretty much self-explanatory.
As soon as I found some `public class TracklogWriter` and a `private RSAPrivateKey`, I was confident I was on the right track.

A couple hours later, I had extracted the private key and built a proof-of-concept to sign arbitrary IGC files.

I contacted XCTrack developers about this issue, and they promptly implemented a more thorough obfuscation of the private key.

{{< raw >}}
<details>
<summary>E-mail thread</summary>
{{< /raw >}}

```text
┌──────────────────────────────────────────┐
│ From: Téo Bouvard <teobouvard@gmail.com> │
│ To: XCTRACK <xctrack@xcontest.org>       │
│ Date: Mon, 16 Nov 2020 11:18:00 +0100    │
│ Subject: XCTrack security issue          │
└──────────────────────────────────────────┘

Hi,

I recently noticed the private key used for signing G records can be easily
retrieved from the XCTrack apk. This would allow someone to submit crafted
IGC tracks as being legitimate during competitions, XC contests or even
FAI-sanctioned records. Although I understand this risk is inherent to
device-based signing schemes as the private key must reside inside the
device, do you consider this an "acceptable risk" or is this a security
issue you would be willing to work on?

You can find attached a crafted track passing validation to prove this claim.

Sincerely,
Téo Bouvard

[Attachment: crafted_signed.igc (1K)]

┌────────────────────────────────────────┐
│ From: XCTRACK <xctrack@xcontest.org>   │
│ To: Téo Bouvard <teobouvard@gmail.com> │
│ Date: Mon, 16 Nov 2020 14:20:00 +0100  │
│ Subject: Re: XCTrack security issue    │
└────────────────────────────────────────┘

Hi Teo,

thanks for pointing this out. We're aware of this issue from the beginning
of the XCTrack. But you're right, nowadays it's easier to dig the key out.
It's impossible to completely hide the key from a determined expert, but
I'll try to hide the key a little bit better.

Best regards
XContest team

┌────────────────────────────────────────┐
│ From: XCTRACK <xctrack@xcontest.org>   │
│ To: Téo Bouvard <teobouvard@gmail.com> │
│ Date: Mon, 23 Nov 2020 12:31:00 +0100  │
│ Subject: Re: XCTrack security issue    │
└────────────────────────────────────────┘

Hi Téo,

could you please take a look at this apk?

Should be harder to dig the key out.

Please let me know your thoughts. Thank you!

Best regards
XContest team

[Attachment: crafted_signed.apk (13.3M)]

┌──────────────────────────────────────────┐
│ From: Téo Bouvard <teobouvard@gmail.com> │
│ To: XCTRACK <xctrack@xcontest.org>       │
│ Date: Mon, 23 Nov 2020 18:23:00 +0100    │
│ Subject: Re: XCTrack security issue      │
└──────────────────────────────────────────┘

Hi ****,

At first sight, this seems a reasonable fix. I think the effort needed to
retrieve the key in this apk can dissuade casual cheating.

Thank you for acting quickly on this.

Cheers,
Téo

┌────────────────────────────────────────┐
│ From: XCTRACK <xctrack@xcontest.org>   │
│ To: Téo Bouvard <teobouvard@gmail.com> │
│ Date: Tue, 24 Nov 2020 11:27:00 +0100  │
│ Subject: Re: XCTrack security issue    │
└────────────────────────────────────────┘

Hi Teo,

great, thanks! (And for pointing this out too.) Next versions will be
released like this.

Cheers
XContest team
```

{{< raw >}}
</details>
{{< /raw >}}

And that was it.
The key was never revoked, and I get it — it would be a mess and it's not worth the trouble, since I'm not going to publish it anywhere.

## Other manufacturers

I want to make it clear that the purpose of this post **is not** to publicly shame XCTrack, on the contrary I think what they're doing is great.
The app is useful, the stability of their [open task specification](https://xctrack.org/Competition_Interfaces.html) is a great testament to their good design choices, and the livetracking features have greatly improved the safety of our sport.
On top of that, they provided these services to pilots around the world for free, with only minimal ads.

I only hacked it because it was the flight recorder I always used.

In theory, all flight recorder manufacturers are affected by this kind of issue, they're just less easily hacked because they control the hardware, as opposed to XCTrack which only ships software to users' devices.

The [IGC specification](https://www.fai.org/sites/default/files/igc_specification_dec_2024_with_al9.pdf) describe with excruciating detail the security procedures manufacturers should follow to get their flight recorders approved by the FAI.

Some of the best extracts include:

> Unless the construction of the recorder case is permanently sealed [...], the case must have a tamper-proof physical seal across at least two joints or screws, so that the seal will be broken if the case is opened. The type of seal [...] must have markings unique to the recorder that are difficult to replicate. Seals with holographic symbols are preferred.

> It must not be possible to access the security algorithms by dis-assembly of the flight recorder, for instance through an EPROM reader.

> If a flight recorder is opened or otherwise interfered with either physically or electronically, a mechanism must exist so that any subsequent data from that flight recorder will be detected as not having the correct Digital Signature. This shall be achieved by a system that operates if the flight recorder case is interfered with and deletes the encryption key(s) required to compute a valid digital signature, such as through a microswitch, or equivalent system [...].

> The security mechanism [...] must be protected from any interference from outside, such as an attempt to prevent the mechanism from operating while the flight-recorder case is opened such as by inserting a physical probe or tool through ventilation holes, through a partially-removed case cover, or through a gap in a slightly-opened case.

I find it strange that all the mitigation techniques suggested in this specification rely on homemade booby traps, but hardware security modules — which are designed for this exact purpose — are not mentioned.

## Other hacking techniques

Even if a flight recorder implemented ideal hardware security, it would still miss the elephant in the room: the reliance on [GPS](https://ciechanow.ski/gps/) to receive positioning data, which is not authenticated.

For a couple hundred bucks, you can [buy a a software-defined radio](https://greatscottgadgets.com/hackrf/) and simulate GPS signals.
This allows you to craft entire tracklogs while being nearly undetectable, which is much easier to carry out than reverse-engineering using hardware probes.

## Does it really matter ?

No.
After all, we're here to fly.

I just feel like a lot of effort has been put by both the International Gliding Commission and flight recorder manufacturers, but that these efforts fall short of actually delivering tangible trust into the tracklogs data.
This is probably sunk-cost fallacy, but I wish the IGC specification recommended cryptographically secure hardware rather than holographic seals and microswitches.

---

I hope this post gave you a better understanding of IGC track signatures, how they're not perfect, but why they are still relevant to prevent casual cheating on online platforms.
