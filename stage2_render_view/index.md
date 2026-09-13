# Humanised score view (J5c view B for scores)

A score render and a humanised re-rendering of it are the J5b/J5c view-B positive pair on the score side: the same notes, played rather than notated. Six rules, all drawn per view from the calibrated ranges. (1) Melody lead: the top note of a chord comes in early -- or late, a third of the time, because real pianists do that too. (2) Chord asynchrony: every other note of the group is scattered around its notated onset. (3) Metrical accent: a weak velocity bonus on downbeats and beats, read straight off the 24-frames-per-quarter score grid. (4) A slow dynamics curve: velocity drifts along a random walk between knots a few seconds apart, and half the segments get a rise-fall arch on top. (5) Articulation: one mode per dynamics segment -- legato, normal, staccato, or hold, the pedal-like mode that stretches a note up to 2.5x and is what gives the real duration ratio's upper tail. (6) Residual velocity jitter on every note. Every timing change is at most 30 ms = 1.44 frames, so latent step t of the view is still step t of the original up to one step boundary -- that is the constraint that makes this a view and not a warp. The x1.5 column multiplies every deviation magnitude (ms, velocity, and the articulation ratios' distance from 1) and is the "wide" set of the calibration study, which is what <code>j5c_flat</code> trains with. What the rules do <em>not</em> produce is a tempo curve: there is no rubato here, and the view-gap gate showed that this is the remaining domain cue between a humanised score and a real performance.

One 1440-frame segment (30 s at 48 fps, 60 quarters on the 24-frames-per-quarter score grid) per piece; the two Beethoven scores start at frame 0, where the paired ASAP performance also starts, the four val scores at 25% of the piece. No score augmentation -- no transposition, no tempo factor, no velocity shift -- so the rows differ by the rule set alone. `render_view` runs on the full note list; the segment is cropped afterwards, as in training.

| piece | corpus | notes | view | on-frame out | n_vel out | mean shift ms | max shift ms | dur ratio | frac longer | chord vel std |
|---|---|---:|---|---:|---:|---:|---:|---:|---:|---:|
| [beethoven_piano_sonatas__01-1](dilemma__beethoven_piano_sonatas__01-1/) | dilemma | 143 | original | 1.000 | 1 | — | — | — | — | 0.00 |
| [beethoven_piano_sonatas__01-1](dilemma__beethoven_piano_sonatas__01-1/) | dilemma | 143 | human_1.0 | 0.000 | 42 | 8.28 | 24.63 | 0.931 | 0.336 | 5.27 |
| [beethoven_piano_sonatas__01-1](dilemma__beethoven_piano_sonatas__01-1/) | dilemma | 143 | human_1.5 | 0.000 | 34 | 13.49 | 30.00 | 0.914 | 0.287 | 8.81 |
| [beethoven_piano_sonatas__16-1](dilemma__beethoven_piano_sonatas__16-1/) | dilemma | 243 | original | 1.000 | 1 | — | — | — | — | 0.00 |
| [beethoven_piano_sonatas__16-1](dilemma__beethoven_piano_sonatas__16-1/) | dilemma | 243 | human_1.0 | 0.000 | 36 | 6.55 | 25.86 | 0.866 | 0.251 | 4.89 |
| [beethoven_piano_sonatas__16-1](dilemma__beethoven_piano_sonatas__16-1/) | dilemma | 243 | human_1.5 | 0.000 | 59 | 9.64 | 30.00 | 0.976 | 0.239 | 6.92 |
| [pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq](pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq/) | pdmx | 112 | original | 1.000 | 1 | — | — | — | — | 0.00 |
| [pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq](pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq/) | pdmx | 112 | human_1.0 | 0.000 | 38 | 4.66 | 23.84 | 0.772 | 0.000 | 5.77 |
| [pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq](pdmx__QmNM8XPBHqdXeNSdWW1azKXde62TKPGNpTkf78rLJv9iuq/) | pdmx | 112 | human_1.5 | 0.000 | 36 | 12.24 | 30.00 | 0.882 | 0.152 | 7.81 |
| [pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u](pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u/) | pdmx | 72 | original | 1.000 | 1 | — | — | — | — | 0.00 |
| [pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u](pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u/) | pdmx | 72 | human_1.0 | 0.000 | 19 | 4.80 | 24.63 | 1.002 | 0.403 | 4.61 |
| [pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u](pdmx__QmNTApqJceptZisbyajTXkUGu983gahxXouNz5GNNXkS8u/) | pdmx | 72 | human_1.5 | 0.000 | 30 | 9.49 | 30.00 | 0.945 | 0.319 | 5.72 |
| [classic__albeniz__alb_esp6_format0](classic__albeniz__alb_esp6_format0/) | classic | 312 | original | 0.894 | 39 | — | — | — | — | 5.42 |
| [classic__albeniz__alb_esp6_format0](classic__albeniz__alb_esp6_format0/) | classic | 312 | human_1.0 | 0.000 | 61 | 9.95 | 30.00 | 0.927 | 0.240 | 9.60 |
| [classic__albeniz__alb_esp6_format0](classic__albeniz__alb_esp6_format0/) | classic | 312 | human_1.5 | 0.000 | 85 | 15.38 | 30.00 | 0.920 | 0.369 | 10.66 |
| [hfclassical__albeniz-espana_op_165](hfclassical__albeniz-espana_op_165/) | hfclassical | 244 | original | 1.000 | 48 | — | — | — | — | 8.14 |
| [hfclassical__albeniz-espana_op_165](hfclassical__albeniz-espana_op_165/) | hfclassical | 244 | human_1.0 | 0.000 | 66 | 9.18 | 30.00 | 1.112 | 0.668 | 12.52 |
| [hfclassical__albeniz-espana_op_165](hfclassical__albeniz-espana_op_165/) | hfclassical | 244 | human_1.5 | 0.000 | 64 | 13.51 | 30.00 | 1.009 | 0.348 | 15.20 |

Each folder holds `original.mid`, `human_1.0.mid`, `human_1.5.mid` (tpq 500 at 120 bpm, 1 tick = 1 ms), `performance.mid` for the two Beethoven pairs, `roll.png` (grey = sounding, black = onset) and `stats.txt`.
