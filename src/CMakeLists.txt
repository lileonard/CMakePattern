OPTION(OMP_FLAG "the flag of OpenMP" ON)
OPTION(MPI_FLAG "the flag of MPI" OFF)
OPTION(FFTW_FLAG "the flag of FFTW" ON)
OPTION(BUILD_CUDA_FLAG "the flag of CUDA" OFF)

add_definitions("-Wall")
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

IF(MPI_FLAG)
  include_directories("/usr/local/mpich3/include")
  link_directories("/usr/local/mpich3/lib")
  link_libraries(" -lmpich -lmpichcxx -lmpl -lopa -lrt ")
  #Makefile Link and Includes here
ENDIF(MPI_FLAG)

if(OMP_FLAG)
  include(./cmake/findopenmp.cmake)
  if(openmp_found)
    target_link_libraries("-fopenmp")
  endif(openmp_found)
endif(OMP_FLAG)

IF(FFTW_FLAG)
  include_directories("/home/lyh/Soft/FFTW3/include")
  link_directories("/home/lyh/Soft/FFTW3/lib")
  link_libraries("-lfftw3")
ENDIF(FFTW_FLAG)


add_subdirectory("sub_dir")
# add subdirectorys of current cmake path
include_directories("path of sub_dir include")
# = -I
link_directories("${PROJECT_BINARY_DIR}/lib")
# = -L 

if(BUILD_CUDA_FLAG)
  include(./cmake/findcuda.cmake)
  cuda_add_executable(templateCUDA
    complex.cpp
    fft.cpp
    geoexp.cpp
    intercoeff.cpp
    main.cpp
    nearstpoint.cpp
    newrayangle.cpp
    raytrace.cpp
    rootcal.cpp
    gaussian.cu
    )
else(BUILD_CUDA_FLAG)
  ADD_EXECUTABLE(template
    complex.cpp
    fft.cpp
    geoexp.cpp
    intercoeff.cpp
    main.cpp
    nearstpoint.cpp
    newrayangle.cpp
    raytrace.cpp
    rootcal.cpp
    gaussian.cpp
    )
endif(BUILD_CUDA_FLAG)


target_link_libraries(template lsubdir)
# = -l belong to this project


# install information
set(cmake_install_prefix ~/aaa)
set(destination ${cmake_install_prefix}/)
add_subdirectory(src bin)
install(files copyright readme destination share/doc/cmake/t2)
install(programs runhello.sh destination bin)
# install .sh file
install(directory doc/ destination share/doc/cmake/t2)
# install the read me files
set(cmake_install_prefix /home/lyh/aaa)
set(libhello_src cmaketemplate.cpp)
add_library(cmaketemplate shared ${libhello_src})
add_library(cmaketemplate_static static ${libhello_src})
set_target_properties(cmaketemplate properties version 1.2 soversion 1)
# install .so file
install(targets cmaketemplate cmaketemplate_static
library destination ${cmake_install_prefix}/lib
archive destination ${cmake_install_prefix}/lib)
#install .a file
install(files cmaketemplate.h 
destination ${cmake_install_prefix}/include/hello)
#install .h file

