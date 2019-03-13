add_definitions(-std=c++11)
# get geos include dir by run geos-config --cflags in shell
include_directories(/usr/local/include)
add_executable(example example.cpp)
# get geos lib dir by run geos-config --ldflags in shell
link_directories(/usr/local/lib)
target_link_libraries(example libgeos.a)
# success while make, but when run example, it can't find libgeos.so.
# target_link_libraries(example libgeos.so)

