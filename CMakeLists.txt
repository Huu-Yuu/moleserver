cmake_minimum_required(VERSION 3.5)

project(project_compile)

# 设置debug变量
set(debug 0)

# 检查是否为debug模式
if(debug)
    set(DEBUG_FLAG "debug=1")
else()
    set(DEBUG_FLAG "")
endif()

# 编译子项目
add_custom_target(all)

# 添加编译子项目的命令
add_custom_command(TARGET all
        COMMAND ${CMAKE_COMMAND} -E chdir ./srcs/molenet make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./srcs/molenet make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./srcs/MahJong make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./srcs/MahJong make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./accountserver make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./accountserver make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./gameserver make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./gameserver make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/example make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/example make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/lhd make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/lhd make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/lunpan make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/lunpan make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/sszp make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/sszp make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/bcbm make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/bcbm make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/brnn make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/brnn make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/hlddz make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/hlddz make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/xzmj make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/xzmj make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/zjh make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/zjh make ${DEBUG_FLAG}
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/dzpk make clean
        COMMAND ${CMAKE_COMMAND} -E chdir ./games/dzpk make ${DEBUG_FLAG}
        )

# 输出信息
message("项目编译成功")
