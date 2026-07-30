# Simple Delay VST3 (JUCE 7.0.12)

A stereo delay effect plugin, built with CMake.

## Structure
- CMakeLists.txt — build config, pulls JUCE 7.0.12
- Source/PluginProcessor.h/.cpp — audio engine (juce::dsp::DelayLine, AudioProcessorValueTreeState)
- Source/PluginEditor.h/.cpp — GUI

## Existing parameters
delayTime (1-2000ms), feedback (0-0.95), mix (0-1)

## Workflow for every task
1. Read the relevant files first.
2. Make the change.
3. Verify it compiles: `cmake -B build && cmake --build build -j2`.
   If a header is missing, `sudo apt-get install -y <package>` and retry.
4. If it fails, read the error, fix it, rebuild. Repeat until clean.
5. Commit to a new branch (not main) and open a pull request explaining the change.

## Rules
No allocation, locking, or logging inside processBlock — it must stay real-time safe.