## C语言输出各种三角形

> 注：共n行n列（行列号从0行开始）；i为行号，j为列号

![](F:\各种三角形.jpg)

```c
//①主对角线下三角
for(i=0;i<n;i++){
	for(j=0;j<i;j++)
		printf("*");
	printf("\n");
}

//②主对角线上三角
for(i=0;i<n;i++){
	for(j=0;j<i;j++)
		printf(" ");//先填空格，腾出位置
	for(j=i;j<n;j++)
		printf("*");
	printf("\n");
}

//③副对角线下三角
for(i=0;i<n;i++){
	for(j=0;j<n-i-1;j++)
		printf(" ");
	for(j=n-i-1;j<n;j++)
		printf("*");
	printf("\n");
}

//④副对角线上三角
for(i=0;i<n;i++){
	for(j=0;j<n-i;j++)
		printf("*");
	printf("\n");
}

//⑤金字塔
    for(i=0;i<n;i++)
    {
        for(j=0;j<n-i-1;j++)
            printf(" ");
        for(j=n-i-1;j<n+i;j++)
            printf("*");
        printf("\n");
    }
或
    for(i=0;i<n;i++)
    {
        for(j=0;j<n-i-1;j++)
            printf(" ");
        for(j=0;j<=2*i;j++)  //如果是2*i-1,则 j 是从1开始
            printf("*");
        printf("\n");
    }

//⑥倒金字塔
    for(i=0;i<n;i++)
    {
        for(j=0;j<i;j++)
            printf(" ");
        for(j=0;j<2*n-2*i-1;j++)
            printf("*");
        printf("\n");
    }
或
    for(i=0;i<n;i++)
    {
        for(j=0;j<i;j++)  printf(" ");
        for(j=i;j<2*n-i-1;j++)  printf("*");
        printf("\n");
    }
```





## 数字三角形

> 注：数字三角需要考虑到不同位数的数字所占的字符位不同。

输入一个整数n（0<n<10），显示n行数字副对角线下三角。如输入5，显示如下：

![](F:\数字副对角下三角.jpg)

```c
#include<stdio.h>

int main()
{
    int i,j,n,num=1;//从1开始每次增加1顺序输出
    scanf("%d",&n);
    for(i=0;i<n;i++){
        for(j=0;j<n-i-1;j++)
            printf("   ");  //以三个字符位为一个单元
        for(j=n-i-1;j<n;j++){
            if(num >= 10)   //对于两位数要扩充到一个单元需要加一个字符位即一个空格
                printf(" %d",num++);
            else
                printf("  %d",num++);//对于一位数需要加两个空格扩充为一个单元
        }
        printf("\n");
    }
    return 0;
}
```



输入一个整数n（0<n<10），显示n行数字金字塔。如输入5，显示如下。

![](F:\数字金字塔.jpg)

```c
#include<stdio.h>
int main(){
    int i,j,n=5;
    for(i=0;i<n;i++)
    {
        for(j=0;j<n-i-1;j++)
            printf(" ");
        for(j=0;j<=2*i;j++)  //如果是2*i-1,则 j 是从1开始
            printf("%d",i+1);
        printf("\n");
    }
}

```

注意：这是没有考虑数字空格的，也就是说，一个单元占一个字符。所以显得很紧凑。如果要数字间距大一些，可以考虑一个单元占2个或3个字符，不过这样就需要改变内循环中第一个循环中输出2或3个空格，以及第二个循环中要将一位数和两位数甚至n位数分类讨论。补满一个单元就行，空格可以加在前面也可以加在后面，不过要统一！要么都加在前要么都加在后，第二个循环的范围不变，即整体的框架不变。 下面为考虑数字空格的情况。

**ps：可以不用讨论，直接用 “%nd” 的形式输出，n表示一个单元所占的字符数。**

```c
#include<stdio.h>
int main(){
    int i,j,n=11;
    for(i=0;i<n;i++)
    {
		printf("     ");
        for(j=0;j<n-i-1;j++)
            printf("   ");
        for(j=0;j<=2*i;j++)  //如果是2*i-1,则 j 是从1开始
            if(i<9)
				printf("%d  ",i+1);
			else
				printf("%d ",i+1);
        //printf("%3d",i+1);
        printf("\n");
    }
	return 0;
}
```

![](F:\数字金字塔2.jpg)



### 杨辉三角

#### 性质

- 杨辉三角的两个腰边的数都是1，其它位置的数都是顶上两个数之和。

- 杨辉三角的任意一行都是二项式系数。

#### 两种实现方法

##### 时间和空间最优算法

```c
/* 时间和空间最优算法 */
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int s = 1, h;                    // 数值和高度
    int i, j;                        // 循环计数
    scanf("%d", &h);                 // 输入层数
    printf("1\n");                   // 输出第一个 1
    for (i = 2; i <= h; s = 1, i++)         // 行数 i 从 2 到层高
    {
        printf("1 ");                // 第一个 1
        for (j = 1; j <= i - 2; j++) // 列位置 j 绕过第一个直接开始循环
            //printf("%d ", (s = (i - j) / j * s));
            printf("%d ", (s = (i - j) * s / j));
        printf("1\n");               // 最后一个 1，换行
    }
    getchar();                       // 暂停等待
    return 0;
}
```



##### 二维数组迭代

默认求直接三角形， 可通过注释的开关或使用编译器的-D定义开关调节等腰三角形和菱形输出。如果觉得复杂，可按照define使用的情况剔除因不符合ifdef条件从而未启用的代码之后阅读。

#define PYRAMID——金字塔

#define PYRAMID + #define REVERSE——菱形

#define REVERSE——对称大三角

![](F:\杨辉三角.jpg)

```c
/* 二维数组迭代 */
#include<stdio.h>
#define M 10       // 行数
// #define PYRAMID // 金字塔，会额外填充空格
// #define REVERSE // 反向再来一次，得到菱形
int main(void)
{
  int a[M][M], i, j;   // 二维数组和循环变量，a[行][列]
  for (i = 0; i<M; i++) // 每一行
  {
#ifdef PYRAMID
    for (j = 0;j <= M-i; j++) printf ("  ");
#endif // 填充结束
    for (j = 0; j <= i; j++) // 赋值和打印集中在一个循环中
      printf("%4d", (a[i][j] = (i == j || j == 0) ? 1 : // 首尾置 1
        a[i - 1][j] + a[i - 1][j - 1] )); // 使用上一行计算
    printf("\n");
  }
#ifdef REVERSE
  for(i = M-2; i >= 0; i--)
  {
#ifdef PYRAMID
    for (j = 0;j <= M - i; j++) printf("  ");
#endif // 填充结束
    for (j = 0;j <= i; j++) printf("%4d",a[i][j]); // 直接使用以前求得的值
    printf("\n");
  }
#endif // 菱形结束
  getchar(); // 暂停等待
  return 0;
}
```



### 显示倒的杨辉三角

![](F:\倒杨辉三角.jpg)

基本思想：先将正的杨辉三角存入二维数组中，然后按照反顺序输出。

ps：可以将这三种封装成三个函数，因为都是在正的杨辉三角二维数组之上进行操作，传入的参数为二维数组a[M]\[M]和行数M。

##### 主对角线上三角

```c
/* 二维数组迭代 */
#include<stdio.h>
#define M 10       // 行数
int main(void)
{
  int a[M][M], i, j;   // 二维数组和循环变量，a[行][列]
  for (i = 0; i<M; i++) // 每一行
  {
    for (j = 0; j <= i; j++) // 赋值和打印集中在一个循环中
      	a[i][j] = (i == j || j == 0) ? 1 : // 首尾置 1
        a[i - 1][j] + a[i - 1][j - 1]; // 使用上一行计算
  }
  for(i = M-1; i >= 0; i--){
      for(j = 0; j < M-i-1; j++)
          printf("    ");//注意：这里是4个空格，一个单元为4个字符，如果是2个空格就是倒金字塔。
      for(j = 0; j <= i; j++)
          printf("%4d",a[i][j]);
  }
  getchar(); // 暂停等待
  return 0;
}
```

##### 副对角线下三角

```c
/* 二维数组迭代 */
#include<stdio.h>
#define M 10       // 行数
int main(void)
{
  int a[M][M], i, j;   // 二维数组和循环变量，a[行][列]
  for (i = 0; i<M; i++) // 每一行
  {
    for (j = 0; j <= i; j++) // 赋值和打印集中在一个循环中
      	a[i][j] = (i == j || j == 0) ? 1 : // 首尾置 1
        a[i - 1][j] + a[i - 1][j - 1]; // 使用上一行计算
  }
  for(i = 0; i < M; i++){
      for(j = 0; j < M-i-1; j++)
          printf("    ");//注意：这里同样是4个空格，一个单元为4个字符，如果是2个空格就是倒金字塔。
      for(j = i; j >= 0; j--)
          printf("%4d",a[i][j]);
      printf("\n");
  }
  getchar(); // 暂停等待
  return 0;
}
```

##### 副对角线上三角

```c
/* 二维数组迭代 */
#include<stdio.h>
#define M 10       // 行数
int main(void)
{
  int a[M][M], i, j;   // 二维数组和循环变量，a[行][列]
  for (i = 0; i<M; i++) // 每一行
  {
    for (j = 0; j <= i; j++) // 赋值和打印集中在一个循环中
      	a[i][j] = (i == j || j == 0) ? 1 : // 首尾置 1
        a[i - 1][j] + a[i - 1][j - 1]; // 使用上一行计算
  }
  for(i = M-1; i >= 0; i--){//不打空格
      for(j = 0; j <= i; j++)
          printf("%4d",a[i][j]);
      printf("\n");
  }
  getchar(); // 暂停等待
  return 0;
}
```



## define_CRT_SECURE_NO_WARNINGS的用法

在编译老的C语言开源项目，如lua源包时，可能会因为一些老的C源文件使用了strcpy，scanf等不安全的函数，而报警告和错误，导致无法编译通过。

此时有两种解决方法：

- 在指定的源文件开头定义：#define_CRT_SECURE_NO_WARNINGS（只会在该文件里生效）

- 在项目属性里设置，这会在整个项目中生效，依次选择：属性->配置属性->C/C++ ->预处理器->预处理器定义->编辑

  最下面加上一行：_CRT_SECURE_NO_WARNINGS （注意不需要#define）



### EOF的意义及用法——while(scanf("%d",&n) != EOF)

EOF为End OF File的缩写，通常在文本的最后存在此字符表示文本结束。

在C语言中，表示文件结束符。

```c
while(Scanf("%d",&n) != EOF){
	....
}
```

注意：在终端手动输入时，系统不知道何时为“文件末尾”，需要使用Ctrl+Z组合键然后回车来告诉系统已经到了EOF，这样系统才会结束while。

在C++中不存在这种用法，但相同作用的是while((cin>>a) != 0)

cin是C++的输入流对象，">>"是重载的运算符，cin>>的返回值是cin对象。用这个当条件的话，通过检测其流的状态来判断结束；
（1）若流是有效的，即流未遇到错误，那么检测成功；
（2）若遇到文件结束符，或遇到一个无效的输入时（例如本题输入的值不是一个整数），istream对象的状态会变为无效，条件就为假；读取失败的时候，就不能继续读取了，那么读取操作结束，while(cin>>a)就返回false，跳出循环！
**C++中的while (cin>>n,n)：**
他的作用是：输入一个数，这数不为0时进入循环，为0时跳出循环。

输入（cin）缓冲是行缓冲。当从键盘上输入一串字符并按回车后，这些字符会首先被送到输入缓冲区中存储。每当按下回车键后，cin 就会检测输入缓冲区中是否有了可读的数据，这种情况下cin对键盘上是否有作为流结束标志CTRL+Z或者CTRL+D，其检查的方式有两种：阻塞式以及非阻塞式。

阻塞式检查方式指的是只有在回车键按下之后才对此前是否有 Ctrl+Z 组合键按下进行检查，非阻塞式样指的是按下 Ctrl+D 之后立即响应的方式。如果在按 Ctrl+D 之前已经从键盘输入了字符，则 Ctrl+D的作用就相当于回车，即把这些字符送到输入缓冲区供读取使用，此时Ctrl+D不再起流结束符的作用。如果按 Ctrl+D 之前没有任何键盘输入，则 Ctrl+D 就是流结束的信号。
阻塞式的方式有一个特点：只有按下回车之后才有可能检测在此之前是否有Ctrl+Z按下。

