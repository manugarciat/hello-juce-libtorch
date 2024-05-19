cmake_minimum_required(VERSION 3.20)
#set(CMAKE_CXX_STANDARD 17)

project(minimal_plugin VERSION 0.0.1)



set(CMAKE_PREFIX_PATH "../../../src/libtorch")
find_package(Torch REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

# where is your JUCE folder? ../../JUCE for me
add_subdirectory(../../../JUCE ./JUCE)


juce_add_plugin(minimal_plugin
        COMPANY_NAME mnlgt
        # set to false if you want audio input
        IS_SYNTH TRUE
        NEEDS_MIDI_INPUT TRUE
        IS_MIDI_EFFECT FALSE
        NEEDS_MIDI_OUTPUT TRUE
        COPY_PLUGIN_AFTER_BUILD TRUE
        PLUGIN_MANUFACTURER_CODE mnGT
        # should change for each plugin
        PLUGIN_CODE Dem0
        FORMATS AU VST3 Standalone
        # should change for each plugin
        PRODUCT_NAME "minimal_plugin")

juce_generate_juce_header(minimal_plugin)

target_sources(minimal_plugin
        PRIVATE
        src/PluginEditor.cpp
        src/PluginProcessor.cpp)

set_property(TARGET minimal_plugin PROPERTY CXX_STANDARD 20)

target_compile_definitions(minimal_plugin
        PUBLIC #
        JUCE_ALSA=1
        JUCE_DIRECTSOUND=1
        JUCE_DISABLE_CAUTIOUS_PARAMETER_ID_CHECKING=1
        JUCE_USE_OGGVORBIS=1
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0)


target_link_libraries(minimal_plugin

        PRIVATE
        juce::juce_audio_utils
#        #### add this line ###
        ${TORCH_LIBRARIES}
        PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
