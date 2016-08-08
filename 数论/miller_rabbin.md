# Miller_Rabbin

 Miller_Rabbin 算法进行素数测试

说明：

 速度快，而且可以判断 <2^63的数

 复杂度：O(logN) * 测试次数 由于要处理long long，所以略慢

 是素数返回true 合数返回false;
 
使用方法：
 
 * bool isPrime =  Miller_Rabbin(x)

Tips:
 
 * 需要stdlib.h头文件
 * 可能是伪素数，但概率极小
 

```
const int S = 20; //随机算法判定次数，S越大，判错概率越小
ll mult_mod(ll a,ll b,ll mod){
    return (a*b-(ll)(a/(long double)mod*b+1e-3)*mod+mod)%mod;
}

//计算  x^n %c
ll pow_mod(ll x, ll n, ll mod) { //x^n%c
    if(n == 1)return x % mod;
    x %= mod;
    ll tmp = x;
    ll ret = 1;
    while(n) {
        if(n & 1) ret = mult_mod(ret, tmp, mod);
        tmp = mult_mod(tmp, tmp, mod);
        n >>= 1;
    }
    return ret;
}

//以a为基,n-1=x*2^t      a^(n-1)=1(mod n)  验证n是不是合数
//一定是合数返回true,不一定返回false
bool check(ll a, ll n, ll x, ll t) {
    ll ret = pow_mod(a, x, n);
    ll last = ret;
    for(int i = 1; i <= t; i++) {
        ret = mult_mod(ret, ret, n);
        if(ret == 1 && last != 1 && last != n - 1) return true; //合数
        last = ret;
    }
    if(ret != 1) return true;
    return false;
}

// Miller_Rabin()算法素数判定
//
bool Miller_Rabbin(ll n) {
    if(n < 2)return false;
    if(n == 2)return true;
    if((n & 1) == 0) return false; //偶数
    ll x = n - 1;
    ll t = 0;
    while((x & 1) == 0) {
        x >>= 1;
        t++;
    }
    for(int i = 0; i < S; i++) {
        ll a = rand() % (n - 1) + 1; //rand()需要stdlib.h头文件
        if(check(a, n, x, t))
            return false;//合数
    }
    return true;
}
```