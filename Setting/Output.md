# 输出

## 使用方法
* `Out(x);`输出`x`

## Tips
* `Out(x);` 等价于`printf("%d",x);` 
* 需要自己手动换行`puts("");`
* 仅限于`int`类型变量

## 模版
```
void Out(int a){    //输出外挂    if(a < 0)    {        putchar('-');        a = -a;    }    if(a >= 10)       Out(a / 10);    putchar(a % 10 + '0');}
```