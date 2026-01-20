# MIDI and Music Theory Reference Guide

## Table of Contents
1. [Basic Music Concepts](#basic-music-concepts)
2. [MIDI Fundamentals](#midi-fundamentals)
3. [MIDI Files](#midi-files)
4. [Working with MIDI in Code](#working-with-midi-in-code)

---

## Basic Music Concepts

### What is Pitch?

Pitch is a perceptual property of sounds that allows their ordering on a frequency-related scale, from low to high. Pitch is determined by the frequency of sound wave vibrations; higher frequency results in higher pitch.

**Frequency**: The number of vibrations per second, measured in Hertz (Hz).
- Example: A note at 440 Hz is one octave above a note at 220 Hz.

### What is a beat?

A beat is a basic unit of time in music that represents a regular pulse. Beats occur at evenly spaced intervals and provide the rhythmic framework for a piece of music. The speed of beats is measured in beats per minute (BPM), which indicates how many beats occur in one minute. For example, at 60 BPM, one beat occurs every second. Beats do not produce sound themselves; they are the underlying timing structure that musicians use to organize when sounds occur.

### What is a note?

A note is a musical sound with two essential properties: pitch and duration. Pitch refers to how high or low the note sounds, determined by its frequency and represented by letter names (A, B, C, D, E, F, G) with possible sharps or flats. Duration refers to how long the note is held, measured in beats. For example, a quarter note lasts for one beat, while a whole note lasts for four beats. In MIDI, notes are represented by numbers (0-127) for pitch and by start time plus duration for how long they play. A note is the actual audible sound you hear in music, as opposed to a beat which is only a timing reference.

### Musical Notes

In Western music, pitches within an octave are named using the first seven letters of the alphabet: A, B, C, D, E, F, and G.

**Note Pattern**:
```
C → D → E → F → G → A → B → C (repeats)
```

**Sharps and Flats**:
Between most notes are intermediate pitches:
- **Sharp (#)**: One half-step higher (C# is higher than C)
- **Flat (♭)**: One half-step lower (D♭ is lower than D)
- C# and D♭ are the same pitch (called enharmonic equivalents)

**Important**: There is no sharp or flat between E-F and B-C.

Complete 12-note sequence in one octave:
```
C → C#/D♭ → D → D#/E♭ → E → F → F#/G♭ → G → G#/A♭ → A → A#/B♭ → B → C
```

### What is an Octave?

An octave is the interval between one musical pitch and another with double or half its frequency.

**Frequency Relationship**:
- If one note has a frequency of 440 Hz, the note one octave above is at 880 Hz, and the note one octave below is at 220 Hz
- The ratio is always 2:1 (double) or 1:2 (half)

**Why "Octave"?**
The word "octave" comes from a Latin root meaning "eight". When counting from C to the next C using only the white keys on a piano (C-D-E-F-G-A-B-C), you count eight notes.

**Octave Equivalence**:
Notes an octave apart are given the same note name in the Western system of music notation. All C notes are considered the same "pitch class" regardless of octave.

### Octave Naming System (ASPN)

American Standard Pitch Notation (ASPN) provides a label for specific musical frequencies by combining a note name (such as "C") with a subscript octave designation (such as "4").

**How it works**:
- The octaves are labeled from lowest to highest, beginning with "0," and continuing in numerical order
- Each new octave designation begins on the note "C"
- Middle C is C4 (this is important to remember)

**Examples**:
- C0: Very low C (near the limit of human hearing)
- C4: Middle C (261.63 Hz)
- C5: One octave above middle C (523.25 Hz)
- A4: 440 Hz (standard tuning pitch)

**Piano Range**:
A piano keyboard primarily uses the ASPN octave designations 1 through 7, although small portions of octaves 0 and 8 are included.

### Time Signature

A time signature is a notation symbol that appears at the beginning of a piece of music. It tells performers how many beats are in each measure and what type of note represents one beat.

**Visual Representation**:
Time signatures appear as two stacked numbers at the start of sheet music, immediately after the clef symbol.

**Structure**:
```
    3    ← Top number: How many beats per measure
    —
    4    ← Bottom number: What note value gets one beat
```

**How to Read It**:
- The top number indicates how many note values of a particular type fit into each measure
- The bottom number indicates the note value that the signature is counting (always a power of 2: usually 2, 4, or 8)

**Bottom Number Reference**:
- 2 = Half note gets one beat
- 4 = Quarter note gets one beat
- 8 = Eighth note gets one beat
- 16 = Sixteenth note gets one beat

**Examples**:
- **4/4**: Four quarter notes per measure (most common)
- **3/4**: Three quarter notes per measure (waltz time)
- **6/8**: Six eighth notes per measure
- **2/2**: Two half notes per measure (cut time)

### Measures (Bars)

Music is organized into groupings called measures or bars. A measure is a segment of time containing a specific number of beats as defined by the time signature.

**Purpose**: 
Measures provide:
- A way to organize music into manageable rhythmic patterns
- Regular points of accent (the first beat of each measure is typically stressed)
- A counting framework for musicians

**Visual Notation**:
On sheet music, measures are separated by vertical lines called bar lines.

```
|  1  2  3  4  |  1  2  3  4  |  1  2  3  4  |
   Measure 1      Measure 2      Measure 3
```

### Common Time Signatures

**4/4 (Common Time)**:
- Four beats per measure, quarter note equals one beat
- Often represented as a "C" symbol, standing for "common time"
- Most widely used time signature in Western music
- Creates a steady, balanced feel
- Count: "1-2-3-4, 1-2-3-4"

**3/4 (Waltz Time)**:
- Three beats per measure, quarter note equals one beat
- Creates a distinct "oom-pah-pah" feel with emphasis on the first downbeat
- Traditional waltz rhythm
- Count: "1-2-3, 1-2-3"

**6/8**:
- Six eighth notes per measure
- Creates a lilting, flowing rhythm
- Often feels like two groups of three beats
- Count: "1-2-3-4-5-6" or "1-2-3, 1-2-3"

**2/4**:
- Two beats per measure, quarter note equals one beat
- Creates a march-like feel
- Count: "1-2, 1-2"

**2/2 (Cut Time)**:
- Two half notes per measure
- Also known as "alla breve," common in faster pieces as it allows musicians to read and play rapid passages more easily
- Often notated with a "C" with a vertical line through it
- Count: "1-2, 1-2" (but each beat is a half note)

### Simple vs. Compound Time

**Simple Time Signatures**:
- The main beat divides into two subdivisions
- Top number is 2, 3, or 4
- Examples: 2/4, 3/4, 4/4
- Each beat naturally splits into two parts

**Compound Time Signatures**:
- The main beat divides into three subdivisions
- Top number is 6, 9, or 12
- Examples: 6/8, 9/8, 12/8
- Each beat naturally splits into three parts

### Irregular (Complex) Time Signatures

Complex time signatures don't follow typical duple or triple meters and are more common in music written after the nineteenth century.

**Examples**:
- **5/4**: Five quarter notes per measure
  - Example: Dave Brubeck's "Take Five"
  - Often grouped as 3+2 or 2+3
  - Count: "1-2-3, 1-2" or "1-2, 1-2-3"

- **7/8**: Seven eighth notes per measure
  - Example: Pink Floyd's "Money"
  - Can be grouped various ways (3+2+2, 2+2+3, etc.)

### Time Signature Changes

Sometimes a composer will put a new time signature in during a piece of music. When this happens, a new time signature symbol appears in the middle of the sheet music, indicating that the rhythmic organization has changed.

**Purpose**: Allows composers to shift the rhythmic feel within a single piece.

### Default in MIDI Files

When a MIDI file does not specify a time signature, the default assumption is 4/4 time signature with a tempo of 120 beats per minute.

---

## Tempo and Timing

### What is Tempo?

In musical terminology, tempo (Italian for 'time'), measured in beats per minute, is the speed or pace of a given composition.

### Beats Per Minute (BPM)

The phrase "beats per minute" (BPM) indicates the number of beats in one minute.

**Examples**:
- A tempo notated as 60 BPM would mean that a beat sounds exactly once per second
- A 120 BPM tempo would be twice as fast, with two beats per second

**Common BPM Ranges**:
- Very Slow (Largo): 40-60 BPM
- Slow (Adagio): 66-76 BPM
- Walking pace (Andante): 76-108 BPM
- Moderate (Moderato): 108-120 BPM
- Fast (Allegro): 120-168 BPM
- Very Fast (Presto): 168-200 BPM
- Extremely Fast (Prestissimo): 200+ BPM

### Beat Duration

The actual duration of a beat depends on both tempo and time signature.

**Formula**:
```
Duration of one beat (in seconds) = 60 / BPM
```

**Examples**:
- At 60 BPM: Each beat = 1 second
- At 120 BPM: Each beat = 0.5 seconds
- At 30 BPM: Each beat = 2 seconds

**With Time Signature**:
If the unit of measure is a quarter in a song (because the time signature is 4/4) and the BPM=60, each quarter will last 1 second. If the time signature is 4/4 and the BPM=120, the quarter will last 0.5 seconds.

### Note Durations

Musical notes have relative durations:
- **Whole note**: 4 beats (in 4/4 time)
- **Half note**: 2 beats
- **Quarter note**: 1 beat
- **Eighth note**: 0.5 beats
- **Sixteenth note**: 0.25 beats

---
