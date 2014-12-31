include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include)
link_directories(${CMAKE_CURRENT_BINARY_DIR}/../libpud)
link_directories(${CMAKE_CURRENT_BINARY_DIR}/../libwar2)

add_definitions(-Wall)
add_definitions(-std=c99)

add_executable(ppm_cmp ppm.c ppm_cmp.c)
add_executable(tiles tiles.c ppm.c)
add_executable(tiler tiler.c)
add_executable(defaults_gen defaults_gen.c)
add_executable(tilemap tilemap.c ppm.c)
add_executable(extract extract.c)

target_link_libraries(tiles ${LIBPUD_LIBRARIES})
target_link_libraries(tiler ${LIBPUD_LIBRARIES})
target_link_libraries(defaults_gen ${LIBPUD_LIBRARIES})
target_link_libraries(tilemap ${LIBPUD_LIBRARIES})
target_link_libraries(extract ${LIBWAR2_LIBRARIES})
