CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.19.55.tar.gz"
    SHA1 "fc4a3e21087b10708d45d503e4dc9e3aa228adf1"
)

PROJECT(SDL_mixer VERSION 2.0.1)

hunter_add_package(SDL2)
find_package(SDL2 CONFIG REQUIRED)

#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
#find_package(SDL2 REQUIRED)

set(SRC_C_FILES
    dynamic_flac.c
    dynamic_fluidsynth.c
    dynamic_mod.c
    dynamic_modplug.c
    dynamic_mp3.c
    dynamic_ogg.c
    effect_position.c
    effect_stereoreverse.c
    effects_internal.c
    fluidsynth.c
    load_aiff.c
    load_flac.c
    load_mp3.c
    load_ogg.c
    load_voc.c
    mixer.c
    music.c
    music_cmd.c
    music_flac.c
    music_mad.c
    music_mod.c
    music_modplug.c
    music_ogg.c
    wavestream.c
)

set(SRC_H_FILES
    dynamic_flac.h
    dynamic_fluidsynth.h
    dynamic_mod.h
    dynamic_modplug.h
    dynamic_mp3.h
    dynamic_ogg.h
    effects_internal.h
    fluidsynth.h
    load_aiff.h
    load_flac.h
    load_mp3.h
    load_ogg.h
    load_voc.h
    music_cmd.h
    music_flac.h
    music_mad.h
    music_mod.h
    music_modplug.h
    music_ogg.h
    SDL_mixer.h
    wavestream.h
)

set(SRC_FILES ${SRC_C_FILES} ${SRC_H_FILES})

add_library(SDL_mixer ${SRC_FILES})
target_link_libraries(SDL_mixer SDL2::SDL2)

#link_libraries(${SDL2_LIBRARY})
#include_directories(${SDL2_INCLUDE_DIR})

#######################################
#       cmake config-mode stuff       #
#######################################

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated/cmake")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

# Note: PROJECT_VERSION is used as a VERSION
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion
)

# Note: variable 'targets_export_name' used
configure_file("Config.cmake.in" "${project_config}" @ONLY)


install(
    TARGETS SDL_mixer
    EXPORT "${targets_export_name}"
    LIBRARY DESTINATION "lib"
    ARCHIVE DESTINATION "lib"
    RUNTIME DESTINATION "bin"
    INCLUDES DESTINATION "${include_install_dir}" "${include_install_dir}/SDL2"
)

install(
    FILES ${SRC_H_FILES}
    DESTINATION "${include_install_dir}/SDL2"
)


install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)


install(
    EXPORT "${targets_export_name}"
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)
