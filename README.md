# Speed Reader Pro

**Speed Reader Pro** is a scientifically grounded Rapid Serial Visual Presentation (RSVP) application designed to enhance reading speed while minimizing cognitive load and eye fatigue. Unlike traditional RSVP readers that stream text at a static velocity, Speed Reader Pro utilizes **dynamic pacing algorithms** based on psycholinguistic research to mimic the natural rhythm of human cognitive processing.

## The Science of Speed Reading

This application addresses three primary biological bottlenecks in human reading: **Saccadic Latency**, **The Optimal Viewing Position**, and **Variable Encoding Time**.

### 1. Eliminating Saccadic Cost (RSVP)

In traditional reading, the eye must make rapid ballistic movements called **saccades** to move from word to word. During a saccade (approx. 20-40ms), the brain suppresses visual input to prevent motion blur, a phenomenon known as **saccadic suppression**. Furthermore, 10-15% of reading time is often lost to **regressions** (unintentional backward glances).

* **The Solution:** By keeping the text stationary (RSVP), Speed Reader Pro eliminates the need for lateral eye movement, theoretically reclaiming the time lost to saccades and reducing the neuromuscular fatigue of the extraocular muscles.

### 2. The Optimal Recognition Point (ORP)

Research by Rayner (1979) and O'Regan (1981) demonstrated that the eye does not naturally land in the dead center of a word. Instead, the **Optimal Viewing Position (OVP)** for English readers is slightly left of center, allowing the fovea to maximize information intake from the initial letters.

* **The Solution:** This application calculates a custom **Pivot Point** for every word (at ~35% of length), creating a "red letter" anchor. This aligns the text exactly where your eye naturally wants to land, reducing the milliseconds your brain wastes "refixating" to process the word.

### 3. Dynamic Pacing (Variable Encoding)

A major flaw in standard RSVP readers is **isochrony**—displaying every word for the exact same duration. However, the human brain requires significantly more time to access the meaning of "counterintuitive" than "the". Fixed-speed presentation often leads to a crash in comprehension because the brain's working memory gets overwhelmed.

* **The Solution:** Speed Reader Pro implements a **content-aware delay algorithm**:
* **Length Compensation:** Words longer than 8 characters trigger a linear time extension (`delay *= len / 6`), granting the brain extra time for lexical access.
* **Prosodic Pausing:** The app detects punctuation (periods, commas, clauses) and inserts micro-pauses (`1.2x` - `2.0x` duration). This mimics natural speech prosody, allowing the brain to "chunk" information into meaningful segments before moving on.



---

## Key Features

### Focus Mode & Zero Distraction

* **Immersive Interface:** One click fades out all UI elements (sliders, headers), leaving only the reader HUD active to minimize peripheral visual distraction.
* **Safe-Exit:** Click anywhere outside the reader box to instantly exit Focus Mode.

### Contextual Rewind

* **The Regression Fix:** Standard RSVP makes it impossible to check a word you missed. Speed Reader Pro includes a **Rewind** feature (Left Arrow or `«` button) that jumps back 5 words, artificially restoring the ability to make "regressions" for comprehension checks without losing your place.

### Reticle & Optical Guidance

* **Foveal Framing:** A custom-designed reticle frames the word with faint upper and lower guides that fade toward the center. This frames the "foveal workspace" without cutting through the text itself.
* **Vertical Anchor:** A 15% opacity vertical line helps anchor the eye vertically, preventing vertical drift during long reading sessions.

### Drift-Corrected Timing Engine

* **The Problem:** Standard JavaScript `setTimeout` is imprecise and "drifts" over time, causing 600 WPM to slowly degrade to 550 WPM over long sessions.
* **The Fix:** The engine uses a **self-correcting delta check**. It compares the *expected* execution time against the *actual* system time (`Date.now()`) on every tick and subtracts the latency drift from the next frame's delay.

---

## 🛠 Technical Implementation

### Parsing & Memoization

To ensure 60fps performance even at 1000+ WPM, the text is not processed in real-time.

* **Pre-Calculation:** When text is pasted, it is immediately parsed into a `queue` of lightweight objects containing the `left`, `pivot`, and `right` substrings, as well as their specific `delayType` (sentence end, clause, long word).
* **Change Detection:** A hashing check (`lastTextHash`) prevents re-parsing if the user hits reset without changing the source text.

### Batched DOM Updates

The render loop uses a grouped update strategy. Rather than firing separate reflow events for the HUD, progress bar, and text, updates are batched within the single `tick()` cycle to minimize browser layout trashing.

### Accessibility & Contrast

* **Contrast Warning:** The app calculates the **YIQ luminance** of your chosen text/background colors in real-time. If the contrast ratio drops below readable standards (YIQ < 50), a warning icon appears to prevent eye strain.

---

## References

1. **Rayner, K. (1979).** Eye guidance in reading: Fixation locations within words. *Perception*, 8(1), 21-30.
2. **O'Regan, J. K. (1981).** The "Optimal Viewing Position" in words: Evidence for proper processing of the initial letters of isolated words. *Perception & Psychophysics*, 30(1), 73-85.
3. **Benedetto, S., Carbone, A., Pedrotti, M., Le Fevre, K., Bey, L. A. Y., & Baccino, T. (2015).** Rapid serial visual presentation in reading: The case of Spritz. *Computers in Human Behavior*, 45, 352-358.
4. **Castelhano, M. S., & Muter, P. (2001).** Optimizing the reading of electronic text using rapid serial visual presentation. *Behaviour & Information Technology*, 20(4), 237-247.
5. **Rayner, K. (1998).** Eye movements in reading and information processing: 20 years of research. *Psychological Bulletin*, 124(3), 372.
6. **Ibbotson, M. R., & Krekelberg, B. (2011).** Visual perception and saccadic eye movements. *Current Opinion in Neurobiology*, 21(4), 553-558.
7. **Payne, B. R., & Federmeier, K. D. (2017).** Pace yourself: Intra-individual variability in context use during reading revealed by self-paced event-related brain potentials. *Journal of Cognitive Neuroscience*, 29(5), 837-854.

---

**Author:** mattswymer

**License:** MIT License
