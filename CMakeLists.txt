cmake_minimum_required(VERSION 3.16)
project(NotTetris CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# --- Pliki źródłowe ---
file(GLOB SOURCES "${CMAKE_SOURCE_DIR}/src/*.cpp")
add_executable(NotTetris ${SOURCES})

# --- SFML ---
# Opcja A (domyślna dla tego repo): lokalna kopia SFML w external/SFML/
# (musisz ją tam samodzielnie pobrać/rozpakować — patrz README.md).
# Opcja B: SFML zainstalowane systemowo lub przez vcpkg/conan — używana
# automatycznie, jeśli external/SFML nie istnieje.
if(EXISTS "${CMAKE_SOURCE_DIR}/external/SFML/include")
    message(STATUS "Używam lokalnej kopii SFML z external/SFML/")
    target_include_directories(NotTetris PRIVATE "${CMAKE_SOURCE_DIR}/external/SFML/include")
    target_link_directories(NotTetris PRIVATE "${CMAKE_SOURCE_DIR}/external/SFML/lib")
    target_link_libraries(NotTetris PRIVATE
        sfml-graphics sfml-window sfml-system sfml-audio sfml-network
    )
else()
    message(STATUS "external/SFML nie znalezione — szukam SFML zainstalowanego w systemie/vcpkg")
    find_package(SFML 2.5 COMPONENTS graphics window system audio network REQUIRED)
    target_link_libraries(NotTetris PRIVATE
        sfml-graphics sfml-window sfml-system sfml-audio sfml-network
    )
endif()

# --- Kopiowanie zasobów (assets/) obok pliku wykonywalnego ---
# Gra wczytuje pliki relatywnie do katalogu roboczego (np. "assets/textures/coin.png"),
# więc assets/ muszą leżeć obok .exe/binarki po zbudowaniu.
add_custom_command(TARGET NotTetris POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${CMAKE_SOURCE_DIR}/assets" "$<TARGET_FILE_DIR:NotTetris>/assets"
    COMMENT "Kopiowanie assets/ do katalogu wynikowego"
)

# --- Kopiowanie DLL-i SFML na Windows (z lokalnej kopii, jeśli jest) ---
if(WIN32 AND EXISTS "${CMAKE_SOURCE_DIR}/external/SFML/bin")
    file(GLOB SFML_DLLS "${CMAKE_SOURCE_DIR}/external/SFML/bin/*.dll")
    add_custom_command(TARGET NotTetris POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different ${SFML_DLLS} "$<TARGET_FILE_DIR:NotTetris>"
        COMMENT "Kopiowanie DLL-i SFML do katalogu wynikowego"
    )
endif()
