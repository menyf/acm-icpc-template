# sprintf

```C++
#include <stdio.h>
main()
{
    char a = 'a';
    char buf[80];
    sprintf(buf, "The ASCII code of a is %d.", a);
    printf("%s", buf);
}
// OUTPUT : The ASCII code of a is 97.
```


```C++
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
int main(void)
{
    char str[100];
    int offset =0;
    int i=0;
    srand(time(0));  // *随机种子
    for(i = 0;i<10;i++)
    {
        offset+=sprintf(str+offset,"%d,",rand()%100);  // 格式化的数据写入字符串
    }
    str[offset-1]='\n';
    printf(str);
    return 0;
}
Output: 74,43,95,95,44,90,70,23,66,84

/*
例子使用了一个新函数srand()，它能产生随机数。例子中最复杂的部分是for循环中每次调用函数sprintf()往字符数组写数据的时候，str+foffset为每次写入数据的开始地址，最终的结果是所有产生的随机数据都被以整数的形式存入数组中。
*/
```