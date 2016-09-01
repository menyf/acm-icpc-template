# 莫比乌斯反演

莫比乌斯反演（mobius）的形式

![](http://img.blog.csdn.net/20160309185255800)

![](http://img.blog.csdn.net/20160309185317738)

其中`mu`的定义如下

![](http://img.blog.csdn.net/20160309190249569)

性质：

1.

![](http://img.blog.csdn.net/20160309190929213)

2.

![](http://img.blog.csdn.net/20160309190941197)

代码求`mu`：

```C++
int normal[maxn];
int mu[maxn];
int prime[maxn];
int pcnt;
void Init()
{
    memset(normal,0,sizeof(normal));
    mu[1] = 1;
    pcnt = 0;
    for(int i=2; i<maxn; i++)
    {
        if(!normal[i])
        {
            prime[pcnt++] = i;
            mu[i] = -1;
        }
        for(int j=0; j<pcnt&&i*prime[j]<maxn; j++)
        {
            normal[i*prime[j]] = 1;
            if(i%prime[j]) mu[i*prime[j]] = -mu[i];
            else
            {
                mu[i*prime[j]] = 0;
                break;
            }
        }
    }
}

```