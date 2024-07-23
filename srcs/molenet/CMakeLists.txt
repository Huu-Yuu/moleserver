cmake_minimum_required(VERSION 3.5)

project(molnet)

set(CMAKE_CXX_STANDARD 11)

# 设置编译选项
if(debug)
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O0 -shared -g -Wall -fexceptions -fPIC -DLINUX -lpthread -lmysqlclient -lhiredis")
else()
    set(CMAKE_CXX_FLAGS "-I ../../include/molnet -O2 -shared -fexceptions -fPIC -DLINUX -lpthread -lmysqlclient -lhiredis")
endif()

# 设置源文件目录
set(SRCS ./netallocator/)
set(SRCS2 ./html5/)
set(SRCXREDIS ./xredis/)

# 设置输出目录
set(OBJDIR objs)
set(BIN ../../libs)

# 添加源文件
file(GLOB OBJS ${OBJDIR}/*.o)
file(GLOB CPPOBJS ${OBJDIR}/*.o)
file(GLOB CPPOBJS2 ${OBJDIR}/*.o)
file(GLOB CPPOBJS3 ${OBJDIR}/*.o)
file(GLOB CPPOBJXREDIS ${OBJDIR}/*.o)

# 生成静态库
add_library(molnet STATIC ${OBJS} ${CPPOBJS} ${CPPOBJS2} ${CPPOBJS3} ${CPPOBJXREDIS})

# 设置目标文件
set(TARGET ${BIN}/libmolnet.a)

# 链接库
target_link_libraries(molnet pthread mysqlclient hiredis)

# 创建目录
file(MAKE_DIRECTORY ${OBJDIR})
file(MAKE_DIRECTORY ${BIN})

# 编译命令
add_custom_command(TARGET molnet POST_BUILD
        COMMAND ar -cr ${TARGET} ${OBJDIR}/*.o
        )

# 编译源文件
foreach(source ${SRCS})
    add_custom_command(OUTPUT ${OBJDIR}/nedmalloc.o
            COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${source} -o ${OBJDIR}/nedmalloc.o
            DEPENDS ${source}
            )
endforeach()

foreach(source ${SRCS2})
    add_custom_command(OUTPUT ${OBJDIR}/base64.o
            COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${source} -o ${OBJDIR}/base64.o
            DEPENDS ${source}
            )
endforeach()

foreach(source ${SRCXREDIS})
    add_custom_command(OUTPUT ${OBJDIR}/xRedisClient.o
            COMMAND ${CMAKE_CXX_COMPILER} -c ${CMAKE_CXX_FLAGS} ${source} -o ${OBJDIR}/xRedisClient.o
            DEPENDS ${source}
            )
endforeach()

# 清理
add_custom_target(clearn
        COMMAND ${CMAKE_COMMAND} -E remove ${TARGET}
        COMMAND ${CMAKE_COMMAND} -E remove_directory ${OBJDIR}
        )

