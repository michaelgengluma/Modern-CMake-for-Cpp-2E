add_library(calc_obj OBJECT basic.cpp)
set_target_properties(calc_obj PROPERTIES
    POSITION_INDEPENDENT_CODE 1
)
target_sources(calc_obj 
               PUBLIC FILE_SET HEADERS
                  BASE_DIRS include
                  FILES include/calc/basic.h
)
include(Coverage)
InstrumentForCoverage(calc_obj)

add_library(calc_shared SHARED)
target_link_libraries(calc_shared calc_obj)
add_library(calc_static STATIC)
target_link_libraries(calc_static calc_obj)

CleanCoverage(calc_static)

include(Format)
Format(calc_static .)

include(CppCheck)
AddCppCheck(calc_obj)

include(Doxygen)
Doxygen(calc src/calc)

# Installation

include(GNUInstallDirs)

install(TARGETS calc_obj calc_shared calc_static
  EXPORT CalcLibrary
  ARCHIVE COMPONENT development
  LIBRARY COMPONENT runtime
  FILE_SET HEADERS COMPONENT runtime
)

if (UNIX)
  install(CODE "execute_process(COMMAND ldconfig)"
          COMPONENT runtime
  )
endif()

install(EXPORT CalcLibrary
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/calc/cmake
  NAMESPACE Calc::
  COMPONENT runtime
)

install(FILES "CalcConfig.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/calc/cmake
)
