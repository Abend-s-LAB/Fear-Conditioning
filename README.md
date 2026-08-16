Markdown

# Neutral-Predictable-Unpredictable (NPU) Threat Task

An empirical psychophysiological and behavioral paradigm designed to dissociate phasic fear (cued/predictable threat) from sustained contextual anxiety (unpredictable threat) using multimodal threat cues, acoustic startle probes, continuous subjective anxiety ratings, and optional emotion-induction video modules[cite: 1, 4].

---

## 🔬 Experimental Paradigm & Design

### 1. Threat Conditions (120s Blocks)
The task alternates between three standard conditions (120 seconds each):
* **Neutral (N):** A safe baseline context where no aversive stimuli (shocks or acoustic screams) are delivered[cite: 1, 4].
* **Predictable Threat (P):** Threat is contingent and cued. Aversive stimuli can occur **only** during the 12-second presentation of the visual cue[cite: 1, 4].
* **Unpredictable Threat (U):** Threat is contextual and sustained. Aversive stimuli can occur at any random moment throughout the 120s block, regardless of cue presence[cite: 1, 4].

### 2. Block Sequences & Habituation
* **Block Orders:** Configurable sequence permutations (e.g., `PNUNUNP` or `UNPNPNU`) run across 1 or 2 experimental blocks.
* **Startle Probes:** 6 randomized acoustic startle probes (bursts of white noise via `startle_probe_low.wav`) delivered per block to assess eye-blink EMG / startle reflex potentiation.
* **Habituation Sequence:** 9 pre-task startle probes presented during initial fixation to habituate baseline acoustic reflexes prior to test blocks.
* **Aversive Stimuli:** Configurable delivery of aversive sound screams (`shock_sound_1.mp3`, `shock_sound_2.mp3`) or TTL-triggered electric shock pulses.

### 3. Continuous and Discrete Subjective Appraisals
* **Continuous Online Rating:** Real-time on-screen Visual Analogue Scale ($0\text{--}10$) tracking continuous state anxiety levels via mouse tracking throughout all blocks.
* **Multimodal VAS Batteries:** Periodic VAS rating scales measuring:
  * Baseline / Mid / Post Task: *Anxiety*, *Avoidance*, *Tiredness*, and *Mood* (Hebrew & English supported).
  * Pre-test Videos: *Happiness*, *Anger*, *Worry*, *Discomfort*, *Sadness*, *Demand for termination*, and prior familiarity.

### 4. Optional Pre/Post Emotion-Induction Videos
Integrated video playback module presenting standardized emotion-eliciting video clips (e.g., *Marriage Story* argument vs. neutral/boring clip) with pre/post fixation crosses and post-clip VAS evaluation batteries[cite: 4].

---

## 🔌 Hardware & Serial Port Trigger Synchronization

When `Record Physiology` is enabled, 8-bit TTL event markers are transmitted over serial (`COM4`, 115200 baud) directly to BioPac / BioSemi acquisition systems[cite: 4]:

| Event Trigger Code | Experimental Meaning |
| :--- | :--- |
| **`255`** | Task initialization marker[cite: 4] |
| **`80`** | Startle habituation probe delivery[cite: 4] |
| **`99`** | Baseline fixation / physiological calibration[cite: 4] |
| **`010` / `110` / `210`** | Condition Block Start (`0xx` = Neutral, `1xx` = Predictable, `2xx` = Unpredictable)[cite: 4] |
| **`020` / `120` / `220`** | Visual Cue Onset[cite: 4] |
| **`030` / `130` / `230`** | Visual Cue Offset (No-Cue context start)[cite: 4] |
| **`+1`** | Startle probe delivered during condition (e.g., `121` = Startle during Predictable Cue)[cite: 4] |
| **`+2`** | Shock / Scream delivered during condition (e.g., `232` = Shock during Unpredictable No-Cue)[cite: 4] |
| **`50` / `55` / `60` / `65`** | Video sequence markers (Fixation, Video 1 Start, Video 1 End, Mid-Fixation)[cite: 4] |

---

## 🚀 Getting Started

### Prerequisites & Dependencies
* **Runtime:** Python 3.8[cite: 1]
* **Core Libraries:** PsychoPy (2023.2.2+), Psychtoolbox, Pygame, Pandas, PySerial, PyQt6, OpenPyXL[cite: 1, 2, 4]

### Running the Experiment

```bash
# 1. Activate environment
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# 2. Launch task
python main.py

Configuration Dialog Options

Upon startup, the GUI dialog allows configuring:

    Subject & Session Number: Output file identification[cite: 4].

    Block Sequence: Condition sequence order (PNUNUNP or UNPNPNU)[cite: 4].

    Shock Type: Auditory scream (Sound) or external stimulator trigger (Shock)[cite: 4].

    Physiology & Debug Toggles: Enable/disable serial triggers, startles, instructions, calibration, and pre/post video timing[cite: 4].

📊 Data Output Architecture

Output spreadsheets are saved directly to ./data/{Subject}/[cite: 4]:

    NPU {Subject} - fullDF - {timestamp}.csv: Millisecond-accurate continuous time-series recording mouse rating positions, timestamps, condition states, and active visual stimuli (sampled at screen refresh rate ~60Hz)[cite: 4].

    NPU {Subject} - miniDF - {timestamp}.csv: Event-level summary log containing trial onsets/offsets, probe deliveries, shock timings, reaction times, and VAS response scores[cite: 4].

    NPU {Subject} Session {session} - VideosData - {timestamp}.csv: Event log and VAS rating data from the pre/post emotion-induction video modules[cite: 4].
