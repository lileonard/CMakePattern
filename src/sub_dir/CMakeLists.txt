set(LIBNAME libSub_dir)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

add_library(${LIBNAME}
   BuildDiffMatrix.cpp
   ModelFinalize.cpp
   ModelInit.cpp
   PointValue2GridValue.cpp
  )

set_target_properties(${LIBNAME} PROPERTIES OUTPUT_NAME "${LIBNAME}")
SET(CMAKE_INSTALL_PREFIX ~/Soft/mylib)
INSTALL(TARGETS ${LIBNAME}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
