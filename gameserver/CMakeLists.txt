cmake_minimum_required(VERSION 3.5)

project(game_server)

set(CMAKE_CXX_STANDARD 11)

# 设置编译选项
if(debug)
    set(CMAKE_CXX_FLAGS "-I ../include/molnet -O0 -g -Wall -ldl -lstdc++ -lpthread -L ../libs -lmolnet -lmysqlclient -ljsoncpp")
else()
    set(CMAKE_CXX_FLAGS "-I ../include/molnet -O2 -ldl -lstdc++ -lpthread -L ../libs -lmolnet -lmysqlclient -ljsoncpp")
endif()

# 设置源文件目录
set(SRCS ./)

# 设置输出目录
set(OBJDIR objs)
set(BIN ../bin)

# 添加源文件
file(GLOB OBJS ${OBJDIR}/*.o)

# 生成可执行文件
add_executable(game_server ${OBJS})

# 设置目标文件
set(TARGET ${BIN}/game_server)

# 链接库
target_link_libraries(game_server dl stdc++ pthread molnet mysqlclient jsoncpp)

# 创建目录
file(MAKE_DIRECTORY ${OBJDIR})
file(MAKE_DIRECTORY ${BIN})

# 编译源文件
add_custom_command(OUTPUT ${OBJDIR}/GameFrameManager.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/GameFrameManager.cpp -o ${OBJDIR}/GameFrameManager.o
        DEPENDS ${SRCS}/GameFrameManager.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/gameparamconfig.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/gameparamconfig.cpp -o ${OBJDIR}/gameparamconfig.o
        DEPENDS ${SRCS}/gameparamconfig.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/game_server.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/game_server.cpp -o ${OBJDIR}/game_server.o
        DEPENDS ${SRCS}/game_server.cpp
        )

# 编译gameframe目录下的文件
add_custom_command(OUTPUT ${OBJDIR}/DBOperator.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/gameframe/DBOperator.cpp -o ${OBJDIR}/DBOperator.o
        DEPENDS ${SRCS}/gameframe/DBOperator.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/CPlayer.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/gameframe/CPlayer.cpp -o ${OBJDIR}/CPlayer.o
        DEPENDS ${SRCS}/gameframe/CPlayer.cpp
        )

# 编译videoplayback目录下的文件
add_custom_command(OUTPUT ${OBJDIR}/CVideoPlaybackManager.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/videoplayback/CVideoPlaybackManager.cpp -o ${OBJDIR}/CVideoPlaybackManager.o
        DEPENDS ${SRCS}/videoplayback/CVideoPlaybackManager.cpp
        )

# 编译matching目录下的文件
add_custom_command(OUTPUT ${OBJDIR}/CBasicMatchingMode.o
        COMMAND ${CMAKE_CXX_COMPILER} -c -g ${CMAKE_CXX_FLAGS} ${SRCS}/matching/CBasicMatchingMode.cpp -o ${OBJDIR}/CBasicMatchingMode.o
        DEPENDS ${SRCS}/matching/CBasicMatchingMode.cpp
        )

# 设置生成可执行文件的命令
add_custom_command(TARGET game_server POST_BUILD
        COMMAND ${CMAKE_CXX_COMPILER} -o ${TARGET} ${OBJDIR}/*.o ${CMAKE_CXX_FLAGS}
        )

# 清理
add_custom_target(clearn
        COMMAND ${CMAKE_COMMAND} -E remove ${TARGET}
        COMMAND ${CMAKE_COMMAND} -E remove_directory ${OBJDIR}
        )

