## MIDI Fundamentals

### What is MIDI?

Musical Instrument Digital Interface (MIDI) is a technical standard that describes a communication protocol, digital interface, and electrical connectors that connect a wide variety of electronic musical instruments, computers, and related audio devices.

**Key Point**: MIDI is NOT audio. MIDI data can be transferred via MIDI or USB cable, or recorded to a sequencer or digital audio workstation to be edited or played back. MIDI contains instructions (like sheet music) while audio files (MP3, WAV) contain actual sound recordings.

### MIDI Note Numbers

MIDI note numbers specify pitches from 0 to 127 (C-2 - G8), with middle C (C3) being note number 60.

**Formula for Frequency**:
```
Frequency (Hz) = 440 × 2^((note_number - 69) / 12)
```
Where note 69 = A4 = 440 Hz

**Common Ranges**:
- The standard 5 octave synthesizer keyboard range is 36 - 96
- The 88-note piano keyboard range is 21 - 108

**Note Number to Pitch Conversion**:
```
MIDI 60 = C4 (Middle C) = 261.63 Hz
MIDI 61 = C#4 = 277.18 Hz
MIDI 62 = D4 = 293.66 Hz
...
MIDI 69 = A4 = 440 Hz (tuning standard)
MIDI 72 = C5 = 523.25 Hz
```

Each increase of 12 = one octave higher
Each decrease of 12 = one octave lower

### Velocity

Velocity is the "value" that determines how a controller effect is applied. For notes, velocity represents how hard a key is pressed.

**Range**: 0-127
- 0 = Silent (note off)
- 1-31 = Very soft
- 32-63 = Soft
- 64-95 = Medium
- 96-127 = Loud
- 127 = Maximum loudness

**Usage**: Velocity affects volume and can also affect timbre (tone quality) on some instruments.

### MIDI Channels

A single MIDI cable can carry up to sixteen channels of MIDI data, each of which can be routed to a separate device.

**Channel Numbering**:
- Technical: Channels 0-15
- Display to users: Channels 1-16

**Purpose**: Channels allow multiple instruments to receive different instructions on the same MIDI connection.
- Example: Channel 1 = Piano, Channel 2 = Guitar, Channel 10 = Drums (by convention)

---

## MIDI Messages

### MIDI Message Structure

The MIDI protocol is made up of messages. A message consists of a string (series) of 8-bit bytes.

**Message Format**:
```
[Status Byte] [Data Byte 1] [Data Byte 2] ...
```

### Status Bytes

The status byte of all Channel Voice messages is nibblised, with the top nibble (the top 4 bits) indicating the command (0x8 to 0xE), and the lower nibble indicating the MIDI channel.

**Format**:
```
Status Byte = [Command (4 bits)][Channel (4 bits)]
```

**Example**:
- 0x92 = Note On, Channel 2
  - High nibble: 9 = Note On
  - Low nibble: 2 = Channel 2

### Common MIDI Message Types

**Note On** (0x9n, where n = channel):
- Data byte 1: Note number (0-127)
- Data byte 2: Velocity (1-127)
- Purpose: Start playing a note

**Note Off** (0x8n):
- Data byte 1: Note number (0-127)
- Data byte 2: Release velocity (0-127)
- Purpose: Stop playing a note
- Note: Often represented as Note On with velocity 0

**Program Change** (0xCn):
- Data byte 1: Program number (0-127)
- Purpose: Change instrument/sound
- Example: 0 = Acoustic Grand Piano, 24 = Acoustic Guitar

**Control Change** (0xBn):
- Data byte 1: Controller number (0-127)
- Data byte 2: Value (0-127)
- Purpose: Modify parameters (volume, pan, modulation, etc.)

**Pitch Bend** (0xEn):
- Data bytes 1-2: Combined 14-bit value
- Purpose: Bend pitch up or down
- Using hex, 00 40 is the central (no bend) setting. 00 00 gives the maximum downwards bend, and 7F 7F the maximum upwards bend

---

## MIDI Files

### MIDI File Structure

A MIDI File always starts with a header chunk, and is followed by one or more track chunks.

**File Layout**:
```
MThd [Header Chunk]
  - Format type
  - Number of tracks
  - Time division
MTrk [Track Chunk 1]
  - Track events with delta times
MTrk [Track Chunk 2]
  - Track events with delta times
...
```

### Header Chunk (MThd)

The header chunk specifies basic information about the data in the file.

**Components**:
1. **Chunk type**: "MThd" (4 ASCII characters)
2. **Length**: Always 6 bytes
3. **Format**: 0, 1, or 2
4. **Number of tracks**: How many MTrk chunks follow
5. **Division**: Time resolution

### MIDI File Formats

Only three values of format are specified:

**Format 0**:
- The file contains a single multi-channel track
- All data in one track
- Can contain multiple MIDI channels

**Format 1**:
- The file contains one or more simultaneous tracks (or MIDI outputs) of a sequence
- First track: Tempo and time signature (no notes)
- Subsequent tracks: Note data for different instruments
- All tracks play simultaneously

**Format 2**:
- The file contains one or more sequentially independent single-track patterns
- Each track is separate/independent
- Used for song patterns or loops

### Time Division (Ticks)

The division specifies the meaning of the delta-times.

**Two Formats**:

**1. Metrical Time (most common)**:
- Bits 14 thru 0 represent the number of delta time "ticks" which make up a quarter-note
- Example: Division = 480 means 480 ticks per quarter note
- Higher numbers = better time resolution

**2. SMPTE Time**:
- Used for film/video synchronization
- Specifies frames per second and ticks per frame

### Delta Time

**Definition**: The amount of time elapsed since the previous event in the track.

**Units**: Measured in "ticks" as defined by the file's division value.

**Example Calculation**:
```
Given:
- Division = 480 ticks per quarter note
- Tempo = 120 BPM (quarter note = 0.5 seconds)

If delta_time = 240 ticks:
  240 ticks ÷ 480 ticks/quarter = 0.5 quarter notes
  0.5 quarter notes × 0.5 seconds/quarter = 0.25 seconds
```

### Track Chunks (MTrk)

Track chunks contain a sequence of time-ordered events (MIDI and/or sequencer-specific data), each of which has a delta time value associated with it.

**Event Structure**:
```
[Delta Time] [Event Type] [Event Data]
```

**Event Types in Tracks**:
1. **MIDI Events**: Note On, Note Off, Control Change, etc.
2. **System Exclusive (SysEx)**: Manufacturer-specific data
3. **Meta Events**: Tempo, time signature, lyrics, markers

### Important Meta Events

**Set Tempo** (0xFF 0x51):
- Changes the tempo
- Value in microseconds per quarter note
- Formula: BPM = 60,000,000 / tempo_value

**Time Signature** (0xFF 0x58):
- Defines beats per measure and beat duration
- Example: 4/4 time

**End of Track** (0xFF 0x2F):
- Marks the end of a track
- Required at the end of every track

### Default Values

All MIDI Files should specify tempo and time signature. If they don't, the time signature is assumed to be 4/4, and the tempo 120 beats per minute.

---

## Working with MIDI in Code

### Reading MIDI Files (Python with mido)

```python
import mido

# Open MIDI file
midi_file = mido.MidiFile('song.mid')

# File properties
print(f"Type: {midi_file.type}")  # Format 0, 1, or 2
print(f"Ticks per beat: {midi_file.ticks_per_beat}")
print(f"Length: {midi_file.length} seconds")
print(f"Number of tracks: {len(midi_file.tracks)}")
```

### Extracting Notes

```python
import mido

midi_file = mido.MidiFile('song.mid')
notes = []
current_tick = 0

# Default tempo (120 BPM = 500,000 microseconds per quarter)
tempo = 500000

for track in midi_file.tracks:
    for msg in track:
        current_tick += msg.time
        
        # Find tempo changes
        if msg.type == 'set_tempo':
            tempo = msg.tempo
        
        # Extract note events
        elif msg.type == 'note_on' and msg.velocity > 0:
            notes.append({
                'pitch': msg.note,
                'velocity': msg.velocity,
                'time_ticks': current_tick,
                'channel': msg.channel
            })
```

### Converting Ticks to Seconds

```python
def ticks_to_seconds(ticks, ticks_per_beat, tempo_microseconds):
    """
    Convert MIDI ticks to seconds
    
    ticks: Position in MIDI ticks
    ticks_per_beat: From MIDI file header
    tempo_microseconds: Current tempo (from set_tempo message)
    """
    beats = ticks / ticks_per_beat
    seconds = (beats * tempo_microseconds) / 1_000_000
    return seconds

# Example
time_in_seconds = ticks_to_seconds(960, 480, 500000)
# 960 ticks ÷ 480 = 2 beats
# 2 beats × 500000 microseconds = 1 second
```

### MIDI Note to Frequency

```python
def midi_to_frequency(note_number):
    """
    Convert MIDI note number to frequency in Hz
    A4 (MIDI 69) = 440 Hz
    """
    return 440 * (2 ** ((note_number - 69) / 12))

# Examples
print(midi_to_frequency(60))  # C4 = 261.63 Hz
print(midi_to_frequency(69))  # A4 = 440.00 Hz
print(midi_to_frequency(72))  # C5 = 523.25 Hz
```

### Creating MIDI Files

```python
from midiutil import MIDIFile

# Create file with 1 track
midi = MIDIFile(1)

track = 0
channel = 0
time = 0  # Start time in beats
tempo = 120  # BPM

# Add tempo
midi.addTempo(track, time, tempo)

# Add notes: (track, channel, pitch, time, duration, volume)
midi.addNote(track, channel, 60, 0, 1, 100)  # C4
midi.addNote(track, channel, 64, 1, 1, 100)  # E4
midi.addNote(track, channel, 67, 2, 1, 100)  # G4

# Save file
with open("output.mid", "wb") as output_file:
    midi.writeFile(output_file)
```

---

## Quick Reference Tables

### MIDI Note Numbers (Selected)

| Note | MIDI # | Frequency (Hz) |
|------|--------|----------------|
| C4 (Middle C) | 60 | 261.63 |
| C#4/D♭4 | 61 | 277.18 |
| D4 | 62 | 293.66 |
| D#4/E♭4 | 63 | 311.13 |
| E4 | 64 | 329.63 |
| F4 | 65 | 349.23 |
| F#4/G♭4 | 66 | 369.99 |
| G4 | 67 | 392.00 |
| G#4/A♭4 | 68 | 415.30 |
| A4 (Tuning) | 69 | 440.00 |
| A#4/B♭4 | 70 | 466.16 |
| B4 | 71 | 493.88 |
| C5 | 72 | 523.25 |

### Common Tempo Markings

| Italian Term | Meaning | BPM Range |
|-------------|---------|-----------|
| Largo | Very slow | 40-60 |
| Adagio | Slow | 66-76 |
| Andante | Walking pace | 76-108 |
| Moderato | Moderate | 108-120 |
| Allegro | Fast | 120-168 |
| Presto | Very fast | 168-200 |
| Prestissimo | Extremely fast | 200+ |

### MIDI Status Bytes (Channel Messages)

| Message Type | Status Byte | Data Bytes | Purpose |
|-------------|-------------|------------|---------|
| Note Off | 0x8n | Note, Velocity | Stop note |
| Note On | 0x9n | Note, Velocity | Start note |
| Control Change | 0xBn | Controller, Value | Modify parameter |
| Program Change | 0xCn | Program | Change instrument |
| Pitch Bend | 0xEn | LSB, MSB | Bend pitch |

(n = channel number 0-15)

---
