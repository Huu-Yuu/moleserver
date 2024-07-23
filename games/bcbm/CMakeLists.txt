cmake_minimum_required(VERSION 3.5)

project(game_bcbm)

set(CMAKE_CXX_STANDARD 11)

# 设置编译选项
if(debug)
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O0 -shared -g -Wall -fexceptions -fPIC -DLINUX -lpthread -L ../../libs -lmolnet -lmysqlclient -ljsoncpp")
else()
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O2 -shared -fexceptions -fPIC -DLINUX -lpthread -L ../../libs -lmolnet -lmysqlclient -ljsoncpp")
endif()

# 设置源文件目录
set(SRCS ./)

# 设置输出目录
set(OBJDIR objs)
set(BIN ../../bin/games)

# 添加源文件
file(GLOB CPPOBJS ${OBJDIR}/*.o)

# 生成动态库
add_library(game_bcbm SHARED ${CPPOBJS})

# 设置目标文件
set(TARGET ${BIN}/libgame_bcbm.so)

# 链接库
target_link_libraries(game_bcbm pthread molnet mysqlclient jsoncpp)

# 创建目录
file(MAKE_DIRECTORY ${OBJDIR})
file(MAKE_DIRECTORY ${BIN})

# 编译源文件
add_custom_command(OUTPUT ${OBJDIR}/ServerLogicModule.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/ServerLogicModule.cpp -o ${OBJDIR}/ServerLogicModule.o
        DEPENDS ${SRCS}/ServerLogicModule.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/cserverlogicframe.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/cserverlogicframe.cpp -o ${OBJDIR}/cserverlogicframe.o
        DEPENDS ${SRCS}/cserverlogicframe.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/GameLogic.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/GameLogic.cpp -o ${OBJDIR}/GameLogic.o
        DEPENDS ${SRCS}/GameLogic.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/czhuangmanager.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/czhuangmanager.cpp -o ${OBJDIR}/czhuangmanager.o
        DEPENDS ${SRCS}/czhuangmanager.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/crobotlogicframe.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/crobotlogicframe.cpp -o ${OBJDIR}/crobotlogicframe.o
        DEPENDS ${SRCS}/crobotlogicframe.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/CServerServiceManager.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/CServerServiceManager.cpp -o ${OBJDIR}/CServerServiceManager.o
        DEPENDS ${SRCS}/CServerServiceManager.cpp
        )

# 设置生成动态库的命令
add_custom_command(TARGET game_bcbm POST_BUILD
        COMMAND ${CMAKE_CXX_COMPILER} -o ${TARGET} ${OBJDIR}/*.o ${CMAKE_CXX_FLAGS}
        )

# 清理
add_custom_target(clearn
        COMMAND ${CMAKE_COMMAND} -E remove ${TARGET}
        COMMAND ${CMAKE_COMMAND} -E remove_directory ${OBJDIR}
        )

