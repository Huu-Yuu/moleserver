cmake_minimum_required(VERSION 3.5)

project(majiang)

set(CMAKE_CXX_STANDARD 11)

# 设置编译选项
if(debug)
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O0 -shared -g -Wall -fexceptions -fPIC -DLINUX -lpthread")
else()
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O2 -shared -fexceptions -fPIC -DLINUX -lpthread")
endif()

# 设置源文件目录
set(SRCS ./)

# 设置输出目录
set(OBJDIR objs)
set(BIN ../../libs)

# 添加源文件
file(GLOB OBJS ${OBJDIR}/*.o)

# 生成静态库
add_library(majiang STATIC ${OBJS})

# 设置目标文件
set(TARGET ${BIN}/libmajiang.a)

# 链接库
target_link_libraries(majiang pthread)

# 创建目录
file(MAKE_DIRECTORY ${OBJDIR})
file(MAKE_DIRECTORY ${BIN})

# 编译命令
add_custom_command(TARGET majiang POST_BUILD
        COMMAND ar -cr ${TARGET} ${OBJDIR}/*.o
        )

# 编译源文件
add_custom_command(OUTPUT ${OBJDIR}/MahJongAlg.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/MahJongAlg.cpp -o ${OBJDIR}/MahJongAlg.o
        DEPENDS ${SRCS}/MahJongAlg.cpp
        )

add_custom_command(OUTPUT ${OBJDIR}/MahJongLogic.o
        COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${SRCS}/MahJongLogic.cpp -o ${OBJDIR}/MahJongLogic.o
        DEPENDS ${SRCS}/MahJongLogic.cpp
        )

# 清理
add_custom_target(clearn
        COMMAND ${CMAKE_COMMAND} -E remove ${TARGET}
        COMMAND ${CMAKE_COMMAND} -E remove_directory ${OBJDIR}
        )

