# Flattened performance view (J5c)

A performance and its flattened twin are the J5c self-derived positive pair for the content half: the same notes, with the parts a notation program would not write down removed. Four rules, all deterministic. (1) Onsets within 30 ms chain into one chord group, but a group never spans more than 50 ms. (2) Every note of a group moves to the group's rounded median frame, so it lands on a frame the way a score render does (consecutive groups are kept two frames apart). (3) Every note takes the segment's median velocity, which removes the dynamics and the chord-role spread. (4) A note is cut at the next onset of its own voice (same hand, within an octave), which undoes pedal and legato overlap, and then by the usual caps: never past the next onset of the same pitch, never past the end, never shorter than one frame. What is left alone is rubato: flattening the tempo would need beat tracking and an alignment map, so the twin keeps the performance's timing curve -- that is the residual domain cue. The voice-successor rule is a heuristic, so a notated long note held under a moving same-hand line gets cut short; this shows up as a low median duration ratio on pedal-heavy pieces.

One 1440-frame segment (30 s at 48 fps) per piece, cropped before flattening; the two Beethoven pairs start at frame 0 of both the performance and the DCML score render (so the opening measures line up in content, not in time), the four val performances at 25% of the piece.

| piece | corpus | notes | on-frame in | on-frame out | vel in | vel out | moved | mean shift ms | max shift ms | overlap in | overlap out | dur ratio | dur cut |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| [Beethoven__Piano_Sonatas_1-1__KimG01](asap__Beethoven__Piano_Sonatas_1-1__KimG01/) | asap | 272 | 0.004 | 1.000 | 62 | 1 | 0.996 | 6.94 | 34.50 | 0.478 | 0.000 | 0.946 | 0.268 |
| [Beethoven__Piano_Sonatas_16-1__BuiJL02M](asap__Beethoven__Piano_Sonatas_16-1__BuiJL02M/) | asap | 330 | 0.018 | 1.000 | 83 | 1 | 0.982 | 7.35 | 31.50 | 0.603 | 0.000 | 0.948 | 0.239 |
| [aria_dedup__aa__000461_0](aria_dedup__aa__000461_0/) | aria_dedup | 122 | 0.033 | 1.000 | 12 | 1 | 0.967 | 6.79 | 29.17 | 0.918 | 0.000 | 0.502 | 0.902 |
| [aria_dedup__aa__000995_0](aria_dedup__aa__000995_0/) | aria_dedup | 94 | 0.085 | 1.000 | 13 | 1 | 0.915 | 5.31 | 20.00 | 0.894 | 0.000 | 0.502 | 0.904 |
| [maestro__2006__MIDI-Unprocessed_13_R1_2006_01-06_ORIG_MID--AUDIO_13_R1_2006_04_Track04_wav](maestro__2006__MIDI-Unprocessed_13_R1_2006_01-06_ORIG_MID--AUDIO_13_R1_2006_04_Track04_wav/) | maestro | 176 | 0.006 | 1.000 | 51 | 1 | 0.994 | 6.96 | 31.33 | 0.915 | 0.000 | 0.490 | 0.875 |
| [atepp__09984](atepp__09984/) | atepp | 319 | 0.006 | 1.000 | 64 | 1 | 0.994 | 7.07 | 31.00 | 0.831 | 0.000 | 0.864 | 0.715 |

Each folder holds `original.mid`, `flattened.mid` (tpq 500 at 120 bpm, 1 tick = 1 ms), `score.mid` for the two pairs, `roll.png` (grey = sounding, black = onset) and `stats.txt`.
