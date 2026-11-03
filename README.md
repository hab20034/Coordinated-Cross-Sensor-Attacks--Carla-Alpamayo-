# Coordinated cross-sensor attacks in CARLA and Alpamayo

This team repository contains experiments that modify simulated sensor inputs to present a false account of vehicle motion. Each scenario documents its own implementation, execution steps, and available evidence.

| False story | What it represents | Where to start |
|---|---|---|
| False speed | The vehicle appears to be travelling at a different speed. | [Speed-story notebook](Attacks/Multi-sensor%20Attacks/False%20story%20scenarios/false_speed_story.ipynb) |
| False lateral position | The vehicle appears farther to the side than it really is. | [Lateral and forward pilot results](Attacks/Multi-sensor%20Attacks/False%20story%20scenarios/lateral-forward-pilots/README.md) |
| False forward position | The vehicle appears farther ahead than it really is. | [Lateral and forward pilot results](Attacks/Multi-sensor%20Attacks/False%20story%20scenarios/lateral-forward-pilots/README.md) |

For the lateral and forward pilots, the [lab setup guide](Attacks/Multi-sensor%20Attacks/False%20story%20scenarios/lateral-forward-pilots/SETUP.md) explains how to start CARLA, check the existing Alpamayo environment, and run each condition. The pilot results include a shared clean baseline, GPS-only runs, and uncoordinated and coordinated runs with sensor logs and front-camera videos.

## Single-sensor experiments

The [GPS experiments](Attacks/Single-sensor%20Attacks/GPS/README.md) cover the clean baseline, fixed offset, gradual drift, and dropout in CARLA. Each entry links to its script and recorded results.
