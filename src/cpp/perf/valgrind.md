# valgrind

valgrind

## 内存泄漏分析：

valgrind --tool=memcheck --leak-check=full --log-file=valgrind.out ./output

常见内存错误：Valgrind Memcheck Error Messages:http://cs.ecs.baylor.edu/~donahoo/tools/valgrind/messages.html


## massif 内存占用分析

Valgrind-massif: https://valgrind.org/docs/manual/ms-manual.html

valgrind --tool=massif --stacks=yes ./output

kill 命令终止采集，自动在当前目录输出采集文件。

ms_print ./massif.out > massif.result



## asan

CFLAGS=${CFLAG} -fPIC -DNDEBUG -DDBUG_OFF -g -ggdb3 -O0 --std=c++11 -fsanitize=address -fno-omit-frame-pointer

LDFLAG=-lasan

yum install libasan

export ASAN_OPTIONS="log_path=asanda.log"

ASAN_OPTIONS=detect_stack_use_after_return=1
export ASAN_OPTIONS="alloc_dealloc_mismatch=1"



看行号

addr2line 0xd12dd1 -e output

看行号+函数

addr2line 0xd12dd1 -e output -f -s -C
