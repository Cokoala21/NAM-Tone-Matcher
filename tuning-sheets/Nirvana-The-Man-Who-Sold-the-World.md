# Tuning Sheet — Nirvana, "The Man Who Sold the World"

Companion to `nam-tone-matcher.html`. The `.RTrackTemplate` the app exports loads the
right **FX chain**; this sheet tells you what to **set inside each plugin** to get close
to the record. The `.nam` capture only supplies the amp's character — Input/Output/Gate/EQ
and the effect settings are still on you. These are **starting points**, not measured values.

---

## Track facts

| | |
|---|---|
| **Era / source** | *MTV Unplugged in New York* (1994) |
| **Documented rig (DB)** | Mesa Boogie Studio .22+ → Mesa 4×12 (V30) |
| **Effects in chain** | EHX Small Clone Chorus (subtle) |
| **Confidence** | 68 (inferred) |

> ⚠️ **Reality check.** The famous version is the *Unplugged* performance, which was
> largely **acoustic** — Cobain played a Martin D-18E acoustic-electric, not the Mesa
> electric rig. So treat this as "warm, low-gain, lightly chorused clean," not a cranked
> amp tone. If you're chasing the *original David Bowie studio* version instead, that's a
> different, brighter electric tone — see the note at the bottom.

---

## 1. Pick the capture (TONE3000)

Search the app's suggested term **`mesa boogie studio 22`**, or for the acoustic-true
approach search **`fender clean`** / **`acoustic`** and pick a low-gain, warm clean capture
sorted by downloads. Download its `.nam` plus a matching cabinet `.wav` IR
(**`mesa 4x12 v30`**, or an open-back `1x12` for a softer clean).

---

## 2. Neural Amp Modeler plugin settings

Open the **Neural Amp Modeler** instance on the track, load the `.nam`, then set:

| Control | Value | Why |
|---|---|---|
| **Input** | **−2 to 0 dB** | Keep it just below break-up — this is a clean/edge-of-clean tone, not gain. Nudge down if it dirties up. |
| **Noise Gate** | **Off** (or thresh ≈ −75 dB) | Low-gain clean has little hiss; a hard gate kills the chorus tail. |
| **Bass** | **6** | Warmth without boom. |
| **Middle** | **6** | Vocal, present mids carry the riff. |
| **Treble** | **4–5** | Roll back — the record is dark/warm, not glassy. |
| **Output** | **0 dB**, then to taste | Match level to your project after the chain. |

> If your chosen capture has no tone stack (some `.nam` files are "raw"), do the EQ in
> ReaEQ instead (step 4).

---

## 3. Chorus — EHX Small Clone (the ReaDelay "chorus mode" instance)

The Small Clone is a **shallow, slow** chorus — it should shimmer, not warble.

| ReaDelay (chorus mode) | Value |
|---|---|
| **Depth** | **0.18–0.22** (shallow) |
| **Rate** | **0.35–0.45 Hz** (slow) |
| **Wet/Dry** | **0.25** (subtle) |

---

## 4. ReaEQ (always last in the chain)

| Band | Setting | Purpose |
|---|---|---|
| **HP (high-pass)** | **80 Hz** | Tighten the low end. |
| **Low shelf** | **200 Hz, −1.5 dB** | Trim mud. |
| **Peak** | **3 kHz, +1.0 dB** (gentle) | A touch of presence for the lead. |
| **LP (low-pass)** | **~9–10 kHz** | Darken the top to match the warm record. |

---

## 5. Cabinet IR (ReaVerb)

In the **ReaVerb** instance: remove any default impulse → **Add → File** → select your
downloaded cabinet `.wav`. Keep **Wet/Dry ≈ 1.0** (the IR *is* the cab) and **Gain 0 dB**.

---

## How to dial it in (workflow)

1. **Mute effects first.** Bypass ReaDelay(chorus) and ReaEQ. Get the dry NAM + IR sounding
   right on its own — adjust **Input** until it's clean with just a hair of grit on hard strums.
2. **Match brightness** with the NAM **Treble** (or ReaEQ LP) against the record A/B.
3. **Set level** with **Output** so the track sits where you want before effects.
4. **Un-bypass chorus**, bring **Wet** up from 0 until it's *just* audible — back off if it warbles.
5. **Un-bypass ReaEQ** last; use it only to fix what the amp tone can't (low-end tightness, top-end darkness).
6. A/B against the recording often. Trust your ears over these numbers — capture and pickup
   differences mean the exact values will shift.

---

### Chasing the Bowie original instead?

The 1970 David Bowie studio version is a **brighter, jangly electric** tone (12-string +
electric, more treble, no chorus). Start from: **Input −2 dB, Treble 6–7, Bass 5, Mid 5**,
**drop the chorus entirely**, and raise the ReaEQ low-pass to ~12 kHz.
