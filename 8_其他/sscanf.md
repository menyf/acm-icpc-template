# sscanf

```C++
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
int main(){
	char str[100];
	//用法一：取指定长度的字符串
	sscanf("12345","%4s",str);
	printf("用法一\nstr = %s\n",str);

	//用法二：格式化时间
	int year,month,day,hour,minute,second;
	sscanf("2013/02/13 14:55:34","%d/%d/%d %d:%d:%d",&year,&month,&day,&hour,&minute,&second);
	printf("用法二\ntime = %d-%d-%d %d:%d:%d\n",year,month,day,hour,minute,second);

	//用法三：读入字符串
	sscanf("12345","%s",str);
	printf("用法三\nstr = %s\n",str);

	//用法四：%*d 和 %*s 加了星号 (*) 表示跳过此数据不读入. (也就是不把此数据读入参数中)
	sscanf("12345acc","%*d%s",str);
	printf("用法四\nstr = %s\n",str);

	//用法五：取到指定字符为止的字符串。如在下例中，取遇到'+'为止字符串。
	sscanf("12345+acc","%[^+]",str);
	printf("用法五\nstr = %s\n",str);

	//用法六：取到指定字符集为止的字符串。如在下例中，取遇到小写字母为止的字符串。
	sscanf("12345+acc121","%[^a-z]",str);
	printf("用法六\nstr = %s\n",str);
	return 0;
}
```

Output

```
用法一
str = 1234
用法二
time = 2013-2-13 14:55:34
用法三
str = 12345
用法四
str = acc
用法五
str = 12345
用法六
str = 12345+
```