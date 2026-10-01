cmake_minimum_required(VERSION 3.22)
project(TheMole VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# JUCE: use a local copy if MOLE_JUCE_DIR is given, otherwise download it.
if(DEFINED MOLE_JUCE_DIR)
    add_subdirectory(${MOLE_JUCE_DIR} juce)
else()
    include(FetchContent)
    FetchContent_Declare(juce
        GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
        GIT_TAG 8.0.15
        GIT_SHALLOW TRUE)
    FetchContent_MakeAvailable(juce)
endif()

if(APPLE)
    set(MOLE_FORMATS AU VST3 Standalone)
else()
    set(MOLE_FORMATS VST3 Standalone)
endif()

juce_add_plugin(TheMole
    COMPANY_NAME "Octaking Labs"
    BUNDLE_ID com.octakinglabs.themole
    PLUGIN_MANUFACTURER_CODE Octk
    PLUGIN_CODE Mole
    FORMATS ${MOLE_FORMATS}
    PRODUCT_NAME "The Mole"
    DESCRIPTION "Two-transistor fuzz into KOT-style drive"
    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT FALSE
    NEEDS_MIDI_OUTPUT FALSE
    IS_MIDI_EFFECT FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
    AU_MAIN_TYPE kAudioUnitType_Effect
    VST3_CATEGORIES Fx Distortion
    COPY_PLUGIN_AFTER_BUILD FALSE)

target_sources(TheMole PRIVATE
    Source/PluginProcessor.cpp
    Source/PluginEditor.cpp)

target_compile_definitions(TheMole PUBLIC
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
    JUCE_DISPLAY_SPLASH_SCREEN=0)

target_link_libraries(TheMole
    PRIVATE juce::juce_audio_utils juce::juce_dsp
    PUBLIC juce::juce_recommended_config_flags juce::juce_recommended_lto_flags juce::juce_recommended_warning_flags)
