cmake_minimum_required(VERSION 3.0)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
        "${CMAKE_SOURCE_DIR}/cmake/Modules/")

find_package(ANN REQUIRED)
find_package(Boost 1.58 COMPONENTS program_options system)
find_package(GSL)

set(GICP_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

include_directories(
        ${GICP_INCLUDE_DIR}
        ${GSL_INCLUDE_DIRS}
        ${ANN_INCLUDE_DIR})

set(GICP_LIB_SOURCE_FILES
        src/gicp.cpp
        src/optimize.cpp
        src/bfgs_funcs.cpp
        src/transform.cpp
        src/scan.cpp)

add_library(gicp SHARED ${GICP_LIB_SOURCE_FILES})

target_link_libraries(gicp
        ${ANN_LIBRARIES}
        ${GSL_CBLAS_LIBRARY}
        ${GSL_LIBRARY}
        ${Boost_LIBRARIES})

target_include_directories(gicp PUBLIC
        ${GICP_INCLUDE_DIR}
        ${GSL_INCLUDE_DIRS}
        ${ANN_INCLUDE_DIR})

install(DIRECTORY include/ DESTINATION include/gicp
        FILES_MATCHING PATTERN "*.h")

install(TARGETS gicp DESTINATION lib)
#
#LFLAGS += `pkg-config --libs gsl`
#CXXFLAGS += `pkg-config --cflags gsl`
#
#LFLAGS += -L. -lgicp -Lann_1.1.1/lib -lANN -lgsl -lgslcblas \
#-lboost_program_options -lboost_system -lstdc++
#CXXFLAGS += -O3 -I./ann_1.1.1/include/ANN
#
#LINK = g++
#CXX = g++
#
#SOURCES = optimize.cpp gicp.cpp bfgs_funcs.cpp scan.cpp transform.c scan2ascii.cpp
#
#BINARIES = test_gicp scan2ascii
#
#TARGETS = libgicp.a test_gicp scan2ascii
#
#.SUFFIXES:
#.SUFFIXES: .o .c .cpp .a
#
## rules
#all: $(TARGETS)
#
#libgicp.a: gicp.o optimize.o bfgs_funcs.o transform.o scan.o
#ar rvs $@ $^
#
#test_gicp: test_gicp.o gicp.o optimize.o bfgs_funcs.o transform.o
#
#scan2ascii: scan.o scan2ascii.o transform.o
#
#clean:
#rm -f *.o $(TARGETS) *~ t_*.tfm *.dat mahalanobis.txt correspondence.txt iterations.txt
#
#$(BINARIES):
#$(LINK) -o $@ $^ $(LFLAGS)
#
#.cpp.o:
#$(CXX) $(CXXFLAGS) -c $< -o $@
