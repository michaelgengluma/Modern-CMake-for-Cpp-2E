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
