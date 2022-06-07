include_directories(
	../
	./
	../inc
)

add_executable(mcm
Archive.cpp
Huffman.cpp
MCM.cpp
Memory.cpp
Util.cpp
Compressor.cpp File.cpp
LZ.cpp
Tests.cpp
)
# To link libraries from current directory on Linux.
TARGET_LINK_LIBRARIES(mcm PUBLIC Threads::Threads)
if(LINUX)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BUILD_TYPE})
endif() 