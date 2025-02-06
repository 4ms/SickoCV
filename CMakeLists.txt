cmake_minimum_required(VERSION 3.22)

if (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
elseif("${METAMODULE_SDK_DIR}" STREQUAL "")
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
endif()
include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(SickoCVMetaModulePlugin 
    VERSION     0.1
    DESCRIPTION "SickoCV Plugin for MetaModule"
    LANGUAGES   C CXX ASM
)

add_library(SickoCV STATIC)

# convenient variable pointing to the VCV Rack repo
set(SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR})

target_sources(SickoCV
    PRIVATE
    ${SOURCE_DIR}/src/DrumPlayer.cpp
    ${SOURCE_DIR}/src/KeySampler.cpp
    ${SOURCE_DIR}/src/SickoPlayer.cpp
    ${SOURCE_DIR}/src/SickoSampler.cpp
    ${SOURCE_DIR}/src/SickoLooper1.cpp
    ${SOURCE_DIR}/src/SickoLooper1Exp.cpp

    ${SOURCE_DIR}/src/plugin.cpp
)

target_include_directories(SickoCV PRIVATE
    ${SOURCE_DIR}
)

target_compile_options(SickoCV
    PRIVATE
    # -Wno-deprecated
    # -Wno-deprecated-enum-float-conversion
)

set_property(TARGET SickoCV PROPERTY CXX_STANDARD 20)

if("${INSTALL_DIR}" STREQUAL "")
    set(INSTALL_DIR ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins)
endif()

create_plugin(
    SOURCE_LIB      SickoCV                     # This is the cmake target name (defined in add_target)
    PLUGIN_NAME     SickoCV                     # This must match the brand "slug" used in VCV Rack
    PLUGIN_JSON     ${SOURCE_DIR}/plugin.json   # Path to the plugin.json file used by VCV Rack
    SOURCE_ASSETS   ${SOURCE_DIR}/assets        # Path to the assets/ dir containing the PNGs
    DESTINATION     ${INSTALL_DIR}              # Path to where you want the plugin file output
)
