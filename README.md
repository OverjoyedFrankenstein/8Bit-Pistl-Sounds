# Chiptune Sounds for Orchid + Pistil

22 8-bit / NES-style sounds for the [Telepathic Instruments](https://telepathicinstruments.com) **Orchid** synth, built for the **Pistil 2.0** companion app. Use them to play retro game soundtracks: pulse-wave leads, triangle bass, noise percussion and sound effects.

The repo includes the sounds as ready-made `.inc` files and the Python script that generates them, so you can tweak them or make your own.

## Sounds

### Treble (17)

| # | Sound | Inspired by | What it is |
|---|---|---|---|
| 1 | Chip Square 50 | General NES | 50% square wave, the hollow classic tone |
| 2 | Chip Pulse 25 | Mario | 25% pulse, the classic platformer lead |
| 3 | Chip Pulse 12 | General NES | 12.5% pulse, thin and nasal |
| 4 | Jungle Commando | Contra | Two stacked pulse waves with slight vibrato, a punchy run-and-gun lead |
| 5 | Night Castle | Castlevania | Thin pulse with an octave-up layer, vibrato and an 1/8 echo |
| 6 | Duty Sweep | General NES | Slow pulse-width sweep, for arpeggios and pads |
| 7 | Power Up Blip | Mario effects | Short blip with an upward pitch chirp, for coins and jumps |
| 8 | Hero Fanfare | Zelda | 50% square with a 25% pulse an octave below, for bold adventure themes |
| 9 | Blue Bomber Lead | Mega Man | 25% pulse with stronger vibrato and a tight 1/16 echo |
| 10 | Space Cavern | Metroid | Triangle plus a slowly sweeping thin pulse, with a soft attack and deep echo |
| 11 | Block Puzzle | Tetris | Plucky square that settles to a softer sustain |
| 12 | Arp Pluck | Most NES games | Fast-decay stab that imitates rapid NES chord arpeggios |
| 13 | Twin Pulse | DuckTales-style leads | Two slightly detuned 25% pulses for a shimmering unison lead |
| 14 | Chime Pulse | Menus and treasure jingles | Bell-like thin pulse with a triangle an octave up, plus echo |
| 15 | Laser Zap | Shooters | Falling pitch sweep for shots and lasers |
| 16 | Noise Blast | Explosions | Noise channel only, for explosions and crashes |
| 17 | Noise Tick | NES drums | Short noise hit, like a hi-hat or snare |

### Bass (5)

| # | Sound | Inspired by | What it is |
|---|---|---|---|
| 1 | Chip Triangle Bass | Mario and most NES games | Pure triangle wave, the classic NES bass channel |
| 2 | Chip Pulse Bass | Action games | Square-wave bass with an octave-down layer, for heavier lines |
| 3 | Pluck Pulse Bass | Contra and Mega Man | Decaying 25% pulse for driving bass lines |
| 4 | Triangle Kick Bass | NES drum trick | Triangle with a quick pitch drop on each note, giving a built-in kick |
| 5 | Octave Triangle Bass | General NES | Triangle with a quiet octave-up layer, rounder and easier to hear |

### Suggested pairings

| Style | Treble | Bass |
|---|---|---|
| Mario | Chip Pulse 25 | Chip Triangle Bass |
| Contra | Jungle Commando | Pluck Pulse Bass |
| Castlevania | Night Castle | Chip Pulse Bass |
| Zelda | Hero Fanfare | Octave Triangle Bass |
| Mega Man | Blue Bomber Lead | Triangle Kick Bass |
| Metroid | Space Cavern | Chip Triangle Bass |
| Tetris | Block Puzzle | Chip Triangle Bass |

## Installation

### Option A: copy the files

Copy the folders in `sounds/` into Pistil's user sound folder:

```
~/Library/Telepathic Instruments/Pistil/Pistil Sounds/Treble/
~/Library/Telepathic Instruments/Pistil/Pistil Sounds/Bass/
```

Restart Pistil, and the sounds show up in the Treble and Bass sound lists.

### Option B: drag and drop

Drag `.inc` files from `sounds/Treble` or `sounds/Bass` onto the Pistil window.

### Option C: generate and install with the script

```bash
python3 make_chiptune_sounds.py              # build into ./sounds and install into Pistil
python3 make_chiptune_sounds.py --no-install # build into ./sounds only
```

You need Python 3, and nothing else to install.

### Putting a sound on the Orchid

Load a sound in Pistil with the Orchid connected over USB-C, then use Pistil's upload/save-to-slot option to store it on the hardware.

## Making your own sounds

Each sound is a short block of parameter overrides in `make_chiptune_sounds.py`, layered on a shared chip base: raw oscillators, filter fully open, no velocity response, instant attack, a short release, and no reverb or chorus. Edit a block or add one, then re-run the script.

Example:

```python
"Chip Pulse 25": dict(O1SHAPE=PULSE, O1PW=PW_25),
```

Useful values:

| Setting | Values |
|---|---|
| Oscillator shape (`O1SHAPE` to `O4SHAPE`) | `OFF`, `SAW`, `PULSE`, `TRI` |
| Pulse width (`O1PW`, …) | `PW_50` (127, square), `PW_25` (64), `PW_12` (32) |
| Semitone (`O2SEMI`, …) | `SEMI(12)` = up an octave, `SEMI(-12)` = down an octave |
| Envelopes | `E1` = filter, `E2` = amp, `E3`/`E4` = free for modulation. Suffixes: `A` attack, `H` hold, `D` decay, `S` sustain, `R` release |
| Mod matrix (`M1S`/`M1D`/`M1A` … `M8`) | Source, destination, and amount (`AMT(pct)`, range ±50) |
| FX2 delay (`FX2TYPE=7`) | `FX2P1` time (0 = 1/4, 1 = 1/8, 2 = 1/16), then mix, feedback, spread, high-pass, low-pass |

## File format

Pistil sound files (`.inc`) are plain text with 135 lines and CRLF line endings. Each line holds one raw parameter value from 0 to 127:

```
0x03,	//0 TREBLE_VER
0x00,	//1 TREBLE_MODEL
...
0x02,	//10 TREBLE_O1SHAPE
```

Bass files use the `BASS_` prefix. The parameter order, ranges and defaults in the script match Pistil 2.0.1. These details were worked out by examining the app, because the format has no public documentation, so later versions of Pistil may change it.

## Notes

- The sounds were built for Pistil 2.0.1 on macOS.
- The vibrato depth and the pitch sweeps (Power Up Blip, Laser Zap, Triangle Kick Bass) are tuned by ear. Adjust them to taste.
- Stacked pulse-wave chords are loud. Lower `GAIN` if they clip.

## Disclaimer

This is an unofficial community project. It is not affiliated with or endorsed by Telepathic Instruments. Game titles are mentioned only to describe the musical style each sound aims for. All trademarks belong to their respective owners. No game audio or assets are included.
