# Reproduce the lateral and forward pilots on the lab PC

These commands use the existing Ubuntu lab installation: CARLA 0.9.16, the Town10HD_Opt map, the Alpamayo 1.5 model and its CUDA Python environment, and the live experiment package. Adjust the three paths to match your account. For a fresh machine, obtain the matching runner package and model environment from the integration team first.

## Terminal 1

Start a dedicated CARLA session. Check for an existing server first. If it already serves your experiment on port 2000, use that session.

```bash
pgrep -af 'CarlaUE4-Linux-Shipping'
cd "$HOME/carla0916"
./CarlaUE4.sh -quality-level=Low -windowed -ResX=1280 -ResY=720
```

## Terminal 2

```bash
cd "$HOME/Downloads/carlamayo_experiments"
export CARLA_ROOT="$HOME/carla0916"
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
pilot_python="$HOME/Carlamayo/.venv/bin/python"

"$pilot_python" experiment.py preflight
sha256sum experiment.py
```

The executed runner hash was `ce6eabf02dadfcd00c6dec94d3ff89a95760eb4c55b78aeb5d32a7b8a5289a71`. Each run's summary records additional source hashes. Confirm the runner and source versions with the integration team if they differ. The preflight checks dependencies and CUDA. The live run checks the CARLA client and server connection.

## Run a condition

Keep the same shell open. Set `pilot_story` to `lateral` or `forward` and `pilot_condition` to `clean`, `gps_only`, `uncoordinated`, or `coordinated`. Start with clean. The archived pilots reused the same clean run for both stories. Each run reloads the current CARLA world and clears its actors, so use the dedicated session with other experiment clients stopped.

```bash
pilot_story=forward
pilot_condition=coordinated
pilot_output="results/pilot_$(date +%Y%m%d_%H%M%S)/$pilot_story/$pilot_condition"

"$pilot_python" -u experiment.py run \
  --condition "$pilot_condition" --story "$pilot_story" --magnitude 1 \
  --onset 2 --duration 3 --seed 42 \
  --steps 100 --spawn-index 10 --reset-world \
  --speed-policy timed_lookahead --camera-postprocess off \
  --output "$pilot_output"
```

The coordinated displacement starts at 2 simulation seconds and reaches 1 m at 5 seconds. A full run covers about 10 simulation seconds and can take several minutes on the lab GPU.

## Check the output

```bash
cat "$pilot_output/summary.json"
ls -lh "$pilot_output"
```

Check for `completed`, 100 verified predictions, CARLA client/server 0.9.16, and Town10HD_Opt. Record the actual distance and safety outcomes. Transfer the entire condition folder under the matching story on your laptop. Open `attacked_front.mp4` for the saved front-camera view and compare the frame-50 clean and attacked images for the warp. The [evidence guide](README.md#read-the-evidence) explains the logs.
