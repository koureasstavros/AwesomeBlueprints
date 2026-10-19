# Strava-to-Garmin activity enrichment

This agent helps preserve a run recorded on a phone when a Garmin watch was unavailable—for example, because its battery died or the watch was forgotten. It takes a FIT activity exported from Strava and creates a Garmin-compatible FIT activity with *estimates* for supported metrics the phone did not record.

The original activity remains the source of truth. The agent preserves its recorded route, timestamps, distance, speed, heart rate, and other measured data rather than replacing them with watch data from another run. It uses other activities from the same runner as references to estimate missing metrics, such as running dynamics, power, and training effect where the available data supports an estimate. **Estimated values are not measurements from the missing watch.** Some Garmin Connect features depend on device identity or private data and may be unavailable or displayed differently after import;
## Files and workflow

- Place the phone-recorded target FIT file and any suitable reference FIT activities in [`original/`](./original/). The agent never uses generated files as references or modifies the originals.
- Specify which original FIT is the target. If there is more than one and no target is specified, the agent asks rather than guessing.
- The agent validates the resulting FIT against the target's measured data and writes the final file to [`generated/`](./generated/) as `<target-stem>_generated.fit`. Experimental diagnostic FIT files also belong in `generated/`. Existing generated files are not overwritten without approval.
- Import the validated result into Garmin Connect separately; a valid FIT file does not guarantee that every metric will appear in its interface.

See [the agent instructions](./fit-activity-enrichment-agent.md) for the estimation rules, supported metrics, limitations, and validation requirements.

**Privacy:** FIT files may include precise GPS routes, heart-rate history, profile information, and device identifiers. Treat both `original/` and `generated/` as private;
