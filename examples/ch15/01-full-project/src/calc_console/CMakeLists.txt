add_library(calc_console_static STATIC tui.cpp)
target_sources(calc_console_static 
               PUBLIC FILE_SET HEADERS
                  BASE_DIRS include
                  FILES include/tui.h
)
target_precompile_headers(calc_console_static PUBLIC <string>)
target_compile_features(calc_console_static PUBLIC cxx_std_20)
set_target_properties(calc_console_static PROPERTIES CXX_EXTENSIONS OFF)

include(GetFTXUI)
target_link_libraries(calc_console_static PUBLIC calc_shared
                    ftxui::screen ftxui::dom ftxui::component)

include(Coverage)
InstrumentForCoverage(calc_console_static)
CleanCoverage(calc_console_static)

include(BuildInfo)
BuildInfo(calc_console_static)

include(Format)
Format(calc_console_static .)

include(CppCheck)
AddCppCheck(calc_console_static)

include(Doxygen)
Doxygen(calc_console src/calc_console)

add_executable(calc_console bootstrap.cpp)
target_link_libraries(calc_console calc_console_static)

# Installation

include(GNUInstallDirs)
install(TARGETS calc_console
  RUNTIME COMPONENT runtime
)
