# The Mole (Octaking Labs) - test plugin

Audio Unit / VST3 / Standalone model of The Mole Rev 0.1:
two-transistor fuzz (Soviet MP38A / US silicon) into a King of Tone-style drive.

GitHub Actions builds a Mac installer (DMG) automatically on every upload:
Actions tab > latest run > Artifacts > The-Mole-macOS.

- `Source/MoleDSP.h` - DSP core (framework-free, 4x oversampled)
- `Source/Plugin*.cpp` - JUCE plugin wrapper and UI
- `test/render.cpp` - offline test: renders a WAV through the DSP core
