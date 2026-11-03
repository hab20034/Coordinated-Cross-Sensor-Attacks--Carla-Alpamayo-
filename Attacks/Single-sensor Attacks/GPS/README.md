# Single-sensor GPS experiments

These early CARLA 0.9.14 tests used a parked vehicle. Each folder contains the selected script and evidence from the sensor-output test.

| Experiment | What it tests |
|---|---|
| [Clean GPS baseline](01_gps_baseline/README.md) | Records unmodified GNSS coordinates. |
| [Fixed offset](02_gps_fixed_offset/README.md) | Shifts the reported position by 5 m. |
| [Gradual drift](03_gps_gradual_drift/README.md) | Increases the reported offset to 10 m. |
| [Dropout](04_gps_dropout/README.md) | Withholds the reported coordinates and then restores them. |

To rerun on the original lab setup, launch CARLA 0.9.14 and activate the `carla` Conda environment with the matching Python client. From the chosen experiment folder, run its Python script. Use a separate working directory if you want to keep new outputs separate from the recorded evidence.

The scripts manipulate software outputs. These experiments measure sensor behavior independently of the later CARLA–Alpamayo driving pilots.
