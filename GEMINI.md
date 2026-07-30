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
3. Verify it compiles using the SAME config CI uses:
   `cmake -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release -j2`
   If a header is missing, `sudo apt-get install -y <package>` and retry.
   The built plugin lands in `build/SimpleDelay_artefacts/Release/VST3/`.
4. If it fails, read the error, fix it, rebuild. Repeat until clean.
5. Commit & share the change:
   - **Default (new feature / issue work):** commit to a NEW branch (not main) and open a pull request explaining the change.
   - **Exception (auto-fix task - you were handed a failed build log):** push DIRECTLY to the branch you are on (no PR), and begin the commit message with `[autofix]`. That tag stops the auto-fix loop: never omit it on a repair, never use it on ordinary work.

## Rules
No allocation, locking, or logging inside processBlock - it must stay real-time safe.

## Conventions used by the CI agents
- A commit message starting with `[autofix]` means "the auto-fix agent made this repair." Auto-Fix will NOT run again on a build whose failing commit already carries that tag, so each human commit gets exactly one repair attempt, then it stops and waits.
- The Gemini Agent only wakes when `@gemini-cli` appears in an issue body or a comment. A new issue with no mention is just labeled automatically by the Triage workflow.
- A cron "Retry Watchdog" re-runs flaky Gemini runs automatically but stops after 3 attempts - a genuinely broken workflow gets fixed by a human, not retried forever.