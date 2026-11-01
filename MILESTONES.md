# StruckSynth milestones

## Tests
- all: cmake --build build -j && ctest --test-dir build --output-on-failure

## M1: Core struck-resonator synth
- [x] Strike exciter: Mallet / Noise burst, hardness, velocity → level / hardness, contact mode
- [x] ModalBank: up to 64 modes, material presets, stretch, T60 decay, damp tilt+bell, strike position, brightness, stereo spread
- [x] PitchConverter + Slew (glide); poly/mono with legato note stack
- [x] Expression panel: Pressure→Decay, Timbre→Damp Tilt, Timbre→Strike Position
- [x] Factory presets baked in; DPF/NanoVG UI

## M2: MIDI 2.0 and MPE port
- [x] Raw CLAP entry point (DPF as GUI only, MIDI 2.0 dialect available)
- [x] Per-note UMP decode; note matching on note + channel + group; RC 7 bend range; Pitch 7.25
- [x] CLAP note ids; note expressions (tuning as Pitch 7.25; pressure/brightness decoded)
- [x] `CLAP_EVENT_NOTE_END` after ring-out; voice-info extension
- [x] MPE lower zone: RPN 0 bend range, released-note control freeze, MPE on/off switch
- [x] Pressure and Timbre (CC 74) as per-note modulation sources
- [x] MIDI 1.0→2.0 Min-Center-Max upscale; MIDI 2.0 velocity-0 note-on treated as note-on
- [x] All Notes Off (CC 123) and All Sound Off (CC 120), per channel
- [x] Linux CI (Ubuntu gcc + clang with -Werror)

## M3: VST3 / AUv2 via clap-wrapper
- [x] VST3 (macOS + Linux) and AUv2 (macOS, `aumu StSy nLKL`) through clap-wrapper v0.16.0, wrapping the same CLAP
- [x] VST3 note expressions: Tuning + Brightness offered via `CLAP_PLUGIN_AS_VST3` (Pressure as before)
- [x] clap-wrapper AUv2 GUI patch carried in `patches/` (calls `gui->show()`, which starts the render timer)
- [x] Validated on macOS (Xcode 26.3, Ninja, 2026-10-21): ctest 12/12; clap-validator 35 pass, 0 fail;
      auval passes; Steinberg's VST3 validator 47 pass, 0 fail (lists Tuning + Brightness)
- [ ] By hand: GUI, notes, automation and state recall in Logic (AUv2), REAPER (VST3 + AUv2), Bitwig (VST3);
      Bitwig per-note Tuning in the VST3
- [ ] Linux: VST3 build in CI; not yet loaded in a Linux host

## Save and Load (2026-10-27)
- [x] Save / Load in the header write the patch as the factory presets' JSON and read it back (src/UI/PresetFile.hpp),
      through DPF's file dialog; MPE left out, loaded values clamped to each param's range.
      Ported from StruckGaussian (ReleaseUMP R2-15).
      ss-presetfile checks the round trip, every factory preset, and non-presets
- [x] AUv2: clap-wrapper gives no timers and (unpatched) never calls gui show(), so the UI's idle never ran and DPF would never
      collect the dialog's result. The GUI now runs its own main-run-loop timer when the host has none, started at
      set_parent; the home folder comes from the account, not the sandboxed $HOME; failures show in the Preset menu
      (StruckGaussian 3f0c97c)
- [x] By hand on Linux (2026-11-01): Save and Load (DPF's own dialog)
- [ ] By hand: Save and Load in Logic Pro (AUv2) and a CLAP host on macOS

## M4: Later
- [x] Pitch 7.25 as persistent per-key tuning table (CLAP tuning stays per-note)
- [ ] Move onto the DPF fork (erickampman/DPF) with own DGL_NAMESPACE

## Experiment: mode cloud (2026-10-25)
- [x] `tools/cloudlab`: offline renderer, the object's modes drawn from a Gaussian (log2 ratio ~ Normal(mean, sigma)
      in octaves, fundamental kept by default, amplitude 1/sqrt(ratio), seeded and the same on every platform),
      fed to the plugin's own ModalBank. Sweeps of sigma, mean, count and seed at C2/C4/C6, measured; decisions and
      findings in `tools/cloudlab/README.md`. No plugin, parameter or UI change.
- [x] Listened to (2026-10-25): kept, as its own instrument rather than a series here. StruckGaussian, cloned from
      this repo, is the cloud as a plugin; it carries `tools/cloudlab` on, with the generator in its
      `src/Module/ModalCloud.hpp`
