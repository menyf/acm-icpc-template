# 拓展欧几里得

## 说明
关于linear\_equation()的一些说明:
通过linear\_equation()返回值为该线性方程是否有整数解。参数中返回的x,y是ax+by=c的一组解<Br/>

```
根据方程可知 ==> a(X+x)+b(Y+y)=c
           ==> aX+bY=0
           ==> X/b=-Y/a
           ==> 设参数t使 X=t*b/gcd(a,b)
                        Y=-t*a/gcd(a,b)
               (t为整数)
由此可以构建线性方程ax+by=c的所有解
``` 

## 模版
```C++
//拓展欧几里德求解ax+by=gcd(a,b)的一组解
ll ex_gcd(ll a,ll b,ll &x,ll &y){
    if (b==0) {
        x=1,y=0;
        return a;
    }
    else{
        ll r=ex_gcd(b,a%b,y,x);
        y-=x*(a/b);
        return r;
    }
}
//返回的bool值表示该方程是否有解
bool linear_equation(int a,int b,int c,int &x,int &y){
    int d=ex_gcd(a,b,x,y);
    if (c%d) return false;
    int k=c/d;
    x*=k;y*=k;
    return true;
}

```