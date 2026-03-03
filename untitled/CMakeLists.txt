cmake_minimum_required(VERSION 4.2.3)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(AudioPlugin VERSION 0.0.1)

set(FORMATS "Standalone" "VST3")

juce_add_plugin(AudioPlugin
        VERSION 1.0.0
        COMPANY_NAME marvelgate
        IS_SYNTH FALSE
        NEEDS_MIDI_INPUT FALSE
        NEEDS_MIDI_OUTPUT FALSE
        IS_MIDI_EFFECT FALSE
        COPY_PLUGIN_AFTER_BUILD TRUE
        FORMATS VST3 Standalone
)


target_sources(${PROJECT_NAME}
    PRIVATE
        include/PluginName/PluginEditor.h
        include/PluginName/SineWave.h
        include/PluginName/PluginProcessor.h
        source/PluginEditor.cpp
        source/PluginProcessor.cpp
        source/SineWave.cpp

)



target_include_directories(${PROJECT_NAME}
    PRIVATE
        include/PluginName
)

target_link_libraries(AudioPlugin PRIVATE
        juce::juce_audio_utils
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)


target_compile_definitions(AudioPlugin PRIVATE
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
)

if(MSVC)
    target_compile_definitions(AudioPlugin PRIVATE
            _SILENCE_CXX23_ALIGNED_STORAGE_DEPRECATION_WARNING
    )
    target_compile_options(AudioPlugin PRIVATE
            /wd4625 /wd4626 /wd4668
    )
endif()