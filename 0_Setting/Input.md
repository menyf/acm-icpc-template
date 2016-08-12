# 输入

## 使用方法
* `read(x);`读取整数`x`

## Tips
* 只能读取整数，包括`int`,`long long`,`unsigned int`,`unsigned long long`

## 模版
```C++
template <class T>inline bool Read(T &ret) {    char c; int sgn;    if(c=getchar(),c==EOF) return 0; //EOF
    while(c!='-'&&(c<'0'||c>'9')) c=getchar();    sgn=(c=='-') ?-1:1 ;    ret=(c=='-') ?0:(c -'0');
    while(c=getchar(),c>='0'&&c<='9')
        ret=ret*10+(c-'0');
    ret*=sgn;    return 1;}
```