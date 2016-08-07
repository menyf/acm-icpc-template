# Lucas求组合数

说明：
 1. 数较小且mod较大时求组合数使用逆元，数较大且mod较小时求组合数用lucas
 2. 该模版只可以求对于正数的组合数，如果出现负数的情况则返回0

```
const long long mod=110119;
const int fcnt=120005;
long long fac[fcnt];
long long inv[fcnt];
ll powMod(ll a, ll b){
    ll ans =1;
    for( a%=mod; b; b>>=1, a = a * a % mod)
        if(b&1)  ans = ans * a % mod;
    return  ans;
}
void init(){
    fac[0] = 1;
    for(int i = 1; i < mod; i++)
        fac[i] = fac[i-1] * i % mod;
    inv[mod - 1]=powMod( fac[mod - 1], mod - 2);
    for(int i = mod-2; i >= 0 ; i--)
        inv[i] = inv[i+1] * (i+1) % mod;
}
ll C(ll n, ll m){
    return  m > n ? 0 : fac[n] * inv[m] % mod * inv[n-m] % mod;
}
ll Lucas(ll n, ll m){// n>m
    if(n<0||m<0||n<m) return 0;
    return  m ? (C(n%mod , m%mod) * Lucas(n/mod, m/mod)) % mod : 1;
}
```


