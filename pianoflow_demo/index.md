# PianoFlow renders (pilot, 2026-09-28, score-conditioned)

PianoFlow (SyMuPe, PianoFlow-base, 24.5 M parameters, flow matching over beat-relative deviations, trained on PianoCoRe-A) renders a quantised score MIDI into a performance MIDI with the same notes one-to-one, changing onsets, durations and velocities and adding sustain as lengthened durations. The public API has no guidance, temperature or text control; seed is the diversity knob besides the number of flow steps. The released package (symupe 1.1.0) also silently drops the model's score-conditioning input, so out of the box the model ignores the score's tempo marking and samples its own global tempo (renders 1.3–1.7× too slow with a wide spread); our wrapper forwards the score tokens, after which the score tempo steers the rendered tempo monotonically while score velocities still have no effect. Each piece below is the full ASAP score MIDI, three seeds (0, 1, 2; 10 flow steps, score-conditioned) and one real ASAP performance. The tempo plot shows beats per minute per score quarter for the three renders and the real performance.

| piece | notes | score MIDI dur (s) | seed 0 dur (s) | seed 1 dur (s) | seed 2 dur (s) | real dur (s) | seed vel mean / std | real vel mean / std |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bach, Prelude in C, BWV 846 (WTC I) | 549 | 70.0 | 138.0 | 128.2 | 117.4 | 139.1 | 51.1 / 6.8 | 53.6 / 12.1 |
| Beethoven, Sonata op. 2 no. 1, I | 1683 | 156.6 | 173.5 | 166.1 | 157.0 | 164.2 | 65.9 / 11.9 | 68.9 / 15.3 |
| Schubert, Impromptu op. 90 no. 3 | 2942 | 375.3 | 500.0 | 492.8 | 554.8 | 320.3 | 51.6 / 12.8 | 48.8 / 15.0 |
| Chopin, Étude op. 10 no. 4 | 2239 | 112.2 | 139.3 | 131.1 | 123.8 | 115.1 | 70.8 / 10.1 | 74.9 / 11.1 |
| Liszt, Mephisto Waltz no. 1 | 10233 | 605.1 | 731.1 | 695.4 | 685.7 | 669.3 | 75.9 / 12.7 | 68.7 / 19.3 |

Durations are the last note offset. Velocity of the seeds is the mean over the three seeds of per-file mean / std.

Schubert op. 90 no. 3 still renders 1.6× slower than the real performance even with the score tempo (110 qpm) supplied; the other four pieces are within 0.92–1.14× (seed means, last note offset).

## Bach, Prelude in C, BWV 846 (WTC I)

Folder [`bach_prelude_bwv846/`](bach_prelude_bwv846/): `score.mid`, `sample_00..02.mid` (+ `_sus`), `performance.mid` (ASAP Shi05M), `stats.txt`.

![roll](bach_prelude_bwv846/roll.png)

![tempo](bach_prelude_bwv846/tempo.png)

Tempo: beats per minute per score quarter, 60 / diff of the performed times of consecutive quarters; seeds from the render's beat times, real (Shi05M) from the ASAP beat annotations mapped to score quarters through the score MIDI's tempo map and interpolated at every quarter. No smoothing.

## Beethoven, Sonata op. 2 no. 1, I

Folder [`beethoven_op2no1_mv1/`](beethoven_op2no1_mv1/): `score.mid`, `sample_00..02.mid` (+ `_sus`), `performance.mid` (ASAP KimG01), `stats.txt`.

![roll](beethoven_op2no1_mv1/roll.png)

![tempo](beethoven_op2no1_mv1/tempo.png)

Tempo: beats per minute per score quarter, 60 / diff of the performed times of consecutive quarters; seeds from the render's beat times, real (KimG01) from the ASAP beat annotations mapped to score quarters through the score MIDI's tempo map and interpolated at every quarter. No smoothing.

## Schubert, Impromptu op. 90 no. 3

Folder [`schubert_op90no3/`](schubert_op90no3/): `score.mid`, `sample_00..02.mid` (+ `_sus`), `performance.mid` (ASAP Hou06M), `stats.txt`.

![roll](schubert_op90no3/roll.png)

![tempo](schubert_op90no3/tempo.png)

Tempo: beats per minute per score quarter, 60 / diff of the performed times of consecutive quarters; seeds from the render's beat times, real (Hou06M) from the ASAP beat annotations mapped to score quarters through the score MIDI's tempo map and interpolated at every quarter. No smoothing.

## Chopin, Étude op. 10 no. 4

Folder [`chopin_op10no4/`](chopin_op10no4/): `score.mid`, `sample_00..02.mid` (+ `_sus`), `performance.mid` (ASAP ADIG02), `stats.txt`.

![roll](chopin_op10no4/roll.png)

![tempo](chopin_op10no4/tempo.png)

Tempo: beats per minute per score quarter, 60 / diff of the performed times of consecutive quarters; seeds from the render's beat times, real (ADIG02) from the ASAP beat annotations mapped to score quarters through the score MIDI's tempo map and interpolated at every quarter. No smoothing.

## Liszt, Mephisto Waltz no. 1

Folder [`liszt_mephisto/`](liszt_mephisto/): `score.mid`, `sample_00..02.mid` (+ `_sus`), `performance.mid` (ASAP Avdeeva03), `stats.txt`.

![roll](liszt_mephisto/roll.png)

![tempo](liszt_mephisto/tempo.png)

Tempo: beats per minute per score quarter, 60 / diff of the performed times of consecutive quarters; seeds from the render's beat times, real (Avdeeva03) from the ASAP beat annotations mapped to score quarters through the score MIDI's tempo map and interpolated at every quarter. No smoothing.

## Diversity on Chopin op. 10 no. 4

Eight seeds on Chopin op. 10 no. 4 versus its 22 real ASAP performances, on the common 326-beat grid. Tempo = centred log beat period, dynamics = centred mean velocity per beat window; RMS over beats, averaged over pairs. With score conditioning, renders run 1.1× the real duration with the same global-tempo spread (CV 0.06 vs 0.05); local tempo-curve diversity between seeds (0.103) matches the real inter-performer diversity (0.095); dynamics diversity is somewhat below it (7.1 vs 8.2). The real-versus-render distance (0.125 / 9.1) is above both within-group values, so renders remain separable from real performances. Fewer flow steps (4) shrink dynamics diversity; more (32) add local tempo jitter (micro-timing std 20 ms vs 9). Before the patch (unconditioned) the same seeds gave 170 s, CV 0.19, tempo RMS 0.129.

| group | n | duration mean (s) ± CV | tempo pairwise RMS | dynamics pairwise RMS | micro-timing std (ms) |
|---|---:|---:|---:|---:|---:|
| real (22 perfs) | 22 | 116.5 ± 0.049 | 0.095 | 8.24 | - |
| default | 8 | 129.2 ± 0.060 | 0.103 | 7.07 | 9.4 |
| steps32 | 4 | 136.9 ± 0.048 | 0.176 | 7.54 | 20.0 |
| steps4 | 4 | 114.8 ± 0.068 | 0.130 | 4.40 | 6.9 |

| condition | tempo RMS real↔cond | dynamics RMS real↔cond |
|---|---:|---:|
| default | 0.125 | 9.14 |
| steps32 | 0.152 | 8.81 |
| steps4 | 0.153 | 8.48 |
