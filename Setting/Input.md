# 输入

## 使用方法
* `read(x);`读取整数`x`

## Tips
* `read(x);`相当于`scanf("%d",&x);`
* 只能读取`int`类型整数


```C++
void read(int &x){    //输入外挂      int res = 0, flag = 0;      char ch;      if((ch = getchar()) == '-')         flag = 1;      else if(ch >= '0' && ch <= '9')         res = ch - '0';      while((ch = getchar()) >= '0' && ch <= '9')          res = res * 10 + (ch - '0');      return flag ? -res : res;  }  

```