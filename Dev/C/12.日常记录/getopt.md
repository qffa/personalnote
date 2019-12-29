# 解析命令行参数

相关3个函数

- getopt()
- getopt_long()
- getopt_long_only()

## getopt

**函数原型**

```
#include <unistd.h>

int getopt(int argc, char * const argv[],
           const char *optstring);

extern char *optarg;
extern int optind, opterr, optopt;
```

**功能**

解析命令行传入参数，这里是短参数，例如

```
# 选项a，a的参数10
$ ./a.out -a 100 -b 200 -c 300 -d
```

**参数**

- argc：参数个数
- argv：参数字符串指针数组
- optstring：选项字符串，即能够被解析的参数

**optstring格式**

设置程序所支持的选项列表，接冒号代表有参数。示例：

```
char *optstr = "ab:c::";

/*
 * a: 选项a没有参数
 * b: 需要为选项b提供参数
 * c: 选项c的参数可有可无
 */
```

**返回值**

`getopt()`每次解析一个参数，所以需要被循环调用

- 成功，返回返回参数字符，设置`optind`为下一个元素的`argv`下标
- 解析完毕，返回`-1`
- 选项不支持，返回`?`
- 选项缺少参数：返回`:`，需要设置`optstring`的起始位为`:`


**getopt行为**

默认情况下，`getopt`会解析完所有参数。如果`optstring`的起始位设置为`+`，`getopt`
遇到错误选项则会停止解析

**相关变量**

- optarg：解析的当前选项的参数，例如`-a 100`，`100`即为选项`a`的参数
- optind：下一个option的索引index
- opterr：用于控制是否打印错误消息，将该全局变量置`0`，`getopt()`不会打印error message
- optopt：存放最后一个未知选项


**代码示例**

```
#include <stdio.h>
#include <unistd.h>
#include <getopt.h>

int main(int argc, char *argv[])
{

    int opt;
    opterr = 0;
    //char *string = "+:a::b:c:d";
    char *string = "+a:b:c:d";
    while((opt = getopt(argc, argv, string)) != -1)
    {
        printf("opt = %c\t\t", opt);
        printf("optarg = %s\t\t", optarg);
        printf("optind = %d\t\t", optind);
        printf("argv[optind] = %s\n", argv[optind]);
    }

    return 0;
}
```


## getopt_long

命令行参数解析支持长选项，例如

```
./a.out --src=1.1.1.1 --sport 1111
```

**函数原型**

```
#include <getopt.h>

int getopt_long(int argc, char * const argv[],
           const char *optstring,
           const struct option *longopts,
           int *longindex);

/* 只支持长选项 */
int getopt_long_only(int argc, char * const argv[],
           const char *optstring,
           const struct option *longopts,
           int *longindex);
```

**option结构体**

```
struct option {
    const char *name;       /* 长选项的名称 */
    int         has_arg;    /* 是否有选项 */
    int        *flag;       /* 设置函数的返回行为 */
    int         val;        /* getopt_long的返回值 */
};
```

**参数**

- longopts：记录参数名称和属性的数组，最后一个元素需要置零
- longindex：记录当前参数所在的`longopts`数组的下标

**返回值**

- 对于短选项：返回值同`getopt()`
- 对于长选项：如果`flag`是`NULL`，返回`var`，否则返回`0`。设置`optarg`
- 出错：同`getopt()`函数

**例子**

```
#include <stdio.h>
#include <getopt.h>

int main(int argc, char *argv[])
{
    int opt;
    int digit_optind;
    int option_index;
    char *string = "a::b:c:d";

    static struct option long_options[] =
    {
        {"reqarg", required_argument, NULL, 'r'},
        {"optarg", optional_argument, NULL, 'o'},
        {"noarg", no_argument, NULL, 'n'},
        {0,0,0,0}
    };

    while((opt = getopt_long(argc, argv, string, long_options, &option_index)) != -1)
    {
        printf("opt = %c\t\t", opt);
        printf("optarg = %s\t\t", optarg);
        printf("optind = %d\t\t", optind);
        printf("argv[optind] = %s\t\t", argv[optind]);
        printf("option_index = %d\n", option_index);
    }

    return 0;
}
```
