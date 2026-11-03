# Coordinated sensor pilots with CARLA and Alpamayo

3 November 2026. Two fixed-spawn false-position stories were tested in closed loop: a displacement to the right and a displacement forward. Both use the vehicle's initial heading as their reference. Seven runs share one clean baseline.

| Run | Distance travelled (m) | Peak position-estimate error (m) | Minimum camera coverage | Controller path rejections | Collisions / consecutive off-driving-lane ticks |
|---|---:|---:|---:|---:|---:|
| [Lateral clean](results/lateral/clean/summary.json) | 34.86 | 0.179 | 100.0% | 8 | 0 / 0 |
| [Lateral gps only](results/lateral/gps_only/summary.json) | 34.40 | 1.100 | 100.0% | 8 | 0 / 0 |
| [Lateral uncoordinated](results/lateral/uncoordinated/summary.json) | 35.87 | 1.134 | 81.1% | 7 | 0 / 0 |
| [Lateral coordinated](results/lateral/coordinated/summary.json) | 36.75 | 1.026 | 81.0% | 8 | 0 / 0 |
| [Forward gps only](results/forward/gps_only/summary.json) | 36.88 | 1.142 | 100.0% | 8 | 0 / 0 |
| [Forward uncoordinated](results/forward/uncoordinated/summary.json) | 35.53 | 1.216 | 80.4% | 8 | 0 / 0 |
| [Forward coordinated](results/forward/coordinated/summary.json) | 31.86 | 1.186 | 80.4% | 9 | 0 / 0 |

Each run completed 100 verified predictions. The saved controller commands match the prediction steps and simulator frames. Source hashes and the checked run settings match across conditions.

The forward coordinated run travelled 3.00 m less than clean. The lateral coordinated run travelled 1.89 m farther than clean. These are observations from one run per condition. No run recorded a collision or an off-driving-lane tick. The clean controller also rejected trajectories, so rejections alone do not establish an attack effect.

## Provenance

The pilot uses the team's experimental design and shared CARLA–Alpamayo integration. The lateral runs replicate an earlier live pilot. The forward runs extend the existing implementation with a live comparison. The evidence here comes from the seven runs executed and verified for this contribution.

## What the experiment changes

- Clean uses unmodified GPS, IMU and camera data.
- GPS-only applies a position bias after converting GNSS coordinates to map metres. IMU and camera remain clean.
- Uncoordinated uses separate signs and start times for the channels. For seed 42, GPS and camera use a negative displacement and IMU uses a positive displacement profile.
- Coordinated uses a shared displacement profile. GPS reports the displaced position, IMU acceleration is adjusted to match the profile, and each camera receives an approximate road-plane warp.

GPS and IMU feed a planar estimator. Alpamayo receives its relative motion history and the camera images. Alpamayo predicts waypoints and the controller turns them into steering, throttle and brake. The controller uses true simulated vehicle feedback. Collision and lane checks use the simulated vehicle.

## Run the experiments

Follow the [lab setup and run guide](SETUP.md) to launch CARLA, check the runner and reproduce each condition.

## Front-camera recordings

The videos show the camera view passed to the model. In the clean and GPS-only runs, that view is unwarped. Use the paired frame-50 images to inspect the camera manipulation at the same pose and the sensor logs to inspect GPS and IMU injection.

- [Lateral clean](results/lateral/clean/attacked_front.mp4)
- [Lateral gps only](results/lateral/gps_only/attacked_front.mp4)
- [Lateral uncoordinated](results/lateral/uncoordinated/attacked_front.mp4)
- [Lateral coordinated](results/lateral/coordinated/attacked_front.mp4)
- [Forward gps only](results/forward/gps_only/attacked_front.mp4)
- [Forward uncoordinated](results/forward/uncoordinated/attacked_front.mp4)
- [Forward coordinated](results/forward/coordinated/attacked_front.mp4)

## Read the evidence

- summary.json records the exact settings, versions, source hashes, completion status, distance and safety outcomes.
- sensors.jsonl records original and reported sensor values, estimated and true pose, and camera coverage. It includes startup history samples.
- predictions.jsonl records the model input checks, motion history, predicted trajectory and generated explanation. The explanation is model output.
- controls.jsonl records applied steering, throttle and brake, with path-check decisions.
- The coordinated folders contain frame 50 before and after the image warp at the same vehicle pose. These pairs show the image manipulation. The left image comes from the attacked run before the warp.
- evidence_manifest.json records SHA-256 hashes and identifies the files included in Git. All seven original videos are included. The laptop archive retains the remaining stills. In the clean condition, attacked_front.mp4 contains unmodified images because the filename is shared by the runner.

The camera uses an approximate ground-plane warp. Distorted buildings and uncovered borders are visible. The uncoordinated and coordinated conditions differ in both timing and direction, so these runs do not isolate the effect of coordination. The evaluation covers collision and driving-lane endpoints in one bounded scenario per condition. It supports input-manipulation verification and preliminary comparison. A general attack success rate requires repeated trials.
