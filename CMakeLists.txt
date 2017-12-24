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
    effects_internal.c
    effect_position.c
    effect_stereoreverse.c
    load_aiff.c
    load_voc.c
    mixer.c
    music.c
    music_cmd.c
    music_flac.c
    music_fluidsynth.c
    music_mad.c
    music_mikmod.c
    music_modplug.c
    music_mpg123.c
    music_nativemidi.c
    music_ogg.c
    music_smpeg.c
    music_timidity.c
    music_wav.c
    playmus.c
    playwave.c
)

set(SRC_H_FILES
    effects_internal.h
    load_aiff.h
    load_voc.h
    mixer.h
    music.h
    music_cmd.h
    music_flac.h
    music_fluidsynth.h
    music_mad.h
    music_mikmod.h
    music_modplug.h
    music_mpg123.h
    music_nativemidi.h
    music_ogg.h
    music_smpeg.h
    music_timidity.h
    music_wav.h
    SDL_mixer.h
)

set(SRC_FILES ${SRC_C_FILES} ${SRC_H_FILES})

add_library(SDL_mixer ${SRC_FILES})
target_link_libraries(SDL_mixer SDL2::SDL2)
target_compile_definitions(SDL_mixer PRIVATE
    MUSIC_WAV
    MUSIC_MID
)

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
