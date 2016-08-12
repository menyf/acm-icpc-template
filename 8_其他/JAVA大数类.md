# JAVA大数类

## Java大数方法

```java
import java.math.*;
BigInteger add(BigInteger other);        //加
BigInteger subtract(BigInteger other);   //减
BigInteger multiply(BigInteger other);   //乘
BigInteger divide(BigInteger other);     //除
BigInteger mod(BigInteger other);        //取余
static BigInteger valueOf(long x)        //返回等于x的大数
int compareTo(BigInteger other);         //比较大小
//相等返回0，这个大数比另一个大返回正数，否则返回负数
//BigDecimal用来实现任意精度的浮点数运算，调用方法类似
BigInteger a=BigIntger.valueOf(100);  //将普通数转化为大数
BigInteger c=a.add(b);     //c=a+b;
BigInteger d=c.multiply(b.add(BigInteger.valueOf(2))); //d=c*(b+2)
```

## 使用举例
```java
//大数输入及相乘
import java.math.*;
import java.util.*;
public class Main{
    public static void main(String[] args)
    {
        Scanner input=new Scanner(System.in);
        BigInteger a=input.nextBigInteger();
        BigInteger b=input.nextBigInteger();
        System.out.println(a.multiply(b));
    }
}
```