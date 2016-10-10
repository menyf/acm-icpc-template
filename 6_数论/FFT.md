# FFT多项式乘法
---
**By 饭团**

## 说明
转自 [多项式乘法运算初级版——ACdreamer](http://blog.csdn.net/acdreamers/article/details/39005227)

## 模版

```C++
//len长度为原长度的两倍
//while(len < 2*len1 || len < 2*len2) len <<= 1;  
struct Virt
{
    double r, i;//个人认为这里是R 是实部 i是虚部
    Virt(double r = 0.0,double i = 0.0)
    {
        this->r = r;
        this->i = i;
    }
    Virt operator + (const Virt &x)
    {
        return Virt(r + x.r, i + x.i);
    }
    Virt operator - (const Virt &x)
    {
        return Virt(r - x.r, i - x.i);
    }
    Virt operator * (const Virt &x)
    {
        return Virt(r * x.r - i * x.i, i * x.r + r * x.i);
    }
};
//雷德算法--倒位序
void Rader(Virt F[], int len)
{
    int j = len >> 1;
    for(int i=1; i<len-1; i++)
    {
        if(i < j) swap(F[i], F[j]);
        int k = len >> 1;
        while(j >= k)
        {
            j -= k;
            k >>= 1;
        }
        if(j < k) j += k;
    }
}
//FFT实现
void FFT(Virt F[], int len, int on)
{
    Rader(F, len);
    for(int h=2; h<=len; h<<=1) //分治后计算长度为h的DFT
    {
        Virt wn(cos(-on*2*PI/h), sin(-on*2*PI/h));  //单位复根e^(2*PI/m)用欧拉公式展开
        for(int j=0; j<len; j+=h)
        {
            Virt w(1,0);            //旋转因子
            for(int k=j; k<j+h/2; k++)
            {
                Virt u = F[k];
                Virt t = w * F[k + h / 2];
                F[k] = u + t;     //蝴蝶合并操作
                F[k + h / 2] = u - t;
                w = w * wn;      //更新旋转因子
            }
        }
    }
    if(on == -1)
        for(int i=0; i<len; i++)
            F[i].r /= len;
}
//求卷积
void Conv(Virt a[],Virt b[],int len)
{
    FFT(a,len,1);
    FFT(b,len,1);
    for(int i=0; i<len; i++)
        a[i] = a[i]*b[i];
    FFT(a,len,-1);
}
void Work()
{
    Conv(va,vb,len);
    for(int i=0; i<len; i++)
        result[i] = va[i].r+0.5;
}
```