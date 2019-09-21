## C语言基础

熟悉下面的一些基础概念和语法，使用C语言刷算法题就基本可以了。

### C语言程序执行过程

源代码——预处理器Preprcessor——扩展源代码——编译器Compiler-——汇编代码——汇编器Assembler——目标代码（simple.obj）——链接器Linker——可执行代码（simple.exe）——加载器loader——内存执行——控制台输出。



### `printf()`和`scanf()`函数

内置库函数，在`stdio.h`(头文件)中定义。

```c
printf("format string",argument_list);
```

格式字符串(`"format string"`)可以是`%d`(整数)，`%c`(字符)，`%s`(字符串)，`%f`(float)等)。

```c
scanf("format string",argument_list);
```

例如`scanf("%d",&number)`语句从控制台读取整数，并将给定值存储在数字变量中。



### 基本数据类型

C语言中有`4`种类型的数据类型。

| 类型                                  | 包含的类型                       |
| ------------------------------------- | -------------------------------- |
| 基本数据类型(*Basic Data Type*)       | int, char, float, double         |
| 派生数据类型(*Derived Data Type*)     | array, pointer, structure, union |
| 枚举数据类型(*Enumeration Data Type*) | enum                             |
| Void数据类型(*void Data Type*)        | void                             |

### 

### 转义序列

| `\n` | 新行       |
| ---- | ---------- |
| `\r` | 回车       |
| `\t` | 水平制表符 |
| `\v` | 垂直制表符 |
| `\0` | null       |



### `switch`语句

`switch`表达式必须是整数或字符类型。`case`值必须是整数或字符常量。

```c
switch(expression){    
case value1:    
 //code to be executed;    
 break;  //optional  
case value2:    
 //code to be executed;    
 break;  //optional  
......    

default:     
 code to be executed if all cases are not matched;    
}

```



### 类型转换

语法：`(type)value`; 始终建议将较低的值转换为较高值以避免数据丢失。

```c
// 无类型转换：Output: 2
int f= 9/4;  
printf("f : %d\n", f );

//使用类型转换：Output: 2.250000
float f=(float) 9/4;  
printf("f : %f\n", f );
```



### 值和引用

在通过值调用函数时，原始值不被修改。在通过引用的调用中，原始值可被修改并反映到外部调用者。

```c
void change(int num) {
    num = num + 10; 
}

void change(int *num) {
    *num = *num + 10; 
}

int main() {
    int x = 200;
 	change2(x);  // 通过值调用，变量x并没有改变
    change2(&x); // 通过引用调用，变量x已经改变了
    return 0;
}
```

| 通过值调用                             | 通过引用调用                           |
| -------------------------------------- | -------------------------------------- |
| 将值的副本传递给函数                   | 将值的地址传递给函数                   |
| 函数内的更改不会反映在函数外           | 函数内部的改变也反映在函数的外部       |
| 将在不同的内存位置创建实际和正式的参数 | 将在相同的内存位置创建实际和正式的参数 |



### 指针

```c
// 这段代码很有意思
int main(){
    int a = 10, b = 20, *p1 = &a, *p2 = &b;
    printf("Before swap: *p1=%d *p2=%d\n", *p1, *p2);
    *p1 = *p1 + *p2;
    *p2 = *p1 - *p2;
    *p1 = *p1 - *p2;
    printf("\nAfter swap: *p1=%d *p2=%d\n", *p1, *p2);
}

// Output
//Before swap: *p1=10 *p2=20
//After swap: *p1=20 *p2=1
```



### 数组

```c
int main(){
    // 数组声明初始化
    int arr[5]; // 声明大小，后赋值
    int arr2[5] = {1,2,3,4,5} // 声明赋值
    int arr3[] = {1,2,3,4,5}  

    // 二维数组
    int arr4[3][4] = {0}; // 初始化是一个好习惯 
    int arr5[3][4] = {{1,2,3,4},{2,3,4,5},{3,4,5,6}};
    int j, k;
    // 遍历
    for(j = 0; j < 3; j++) {
        for(k = 0; k < 4; k++){
            printf("%d ", arr5[j][k]);
        }
        printf("\n");
    }

    // 将数组作为参数传递给函数
    int arr[5] = {5,4,3,2,1};
    bubsort(arr,5); // 调用
}
// 函数要声明
void bubsort(int arr[], int len){
}
```



### 字符串

C语言中的字符串是由`\0`(空字符)终止的字符数组。`gets()`函数从用户读取字符串，`puts()`函数打印字符串。这两个函数都在`<stdio.h>`头文件中定义。

```c
char ch[]={'h', 'e', 'l', 'l', '0', '\0'};
char ch2[]="hello";

// 输入输出
char str[10];
printf("输入一个字符串：\n");
gets(str);
puts(str);

// 字符串函数，在"string.h"库中定义。
// strlen()；字符串长度
int len = strlen(str);
printf("字符串长度：%d\n", len);

// strcpy(destination，source)；字符串拷贝
char str2[10];
strcpy(str2，str); // 将字符串str拷贝到字符串str2

// strcat(first_string, second_string)函数连接两个字符串，结果返回到first_string
char str3[] = " world";
strcat(str, str3);

// strcmp(first_string, second_string)函数比较两个字符串，如果两个字符串相等，则返回0。
if(strcmp(str, str2) == 0);

// strrev(string)函数返回给定字符串的反转字符串。
printf("字符串反转: %s \n", strrev(str));

// strlwr(string)函数返回给定字符串的小写形式
printf("小写形式: %s \n", strlwr(str));
// strupr(string)函数返回给定字符串的大写形式
printf("大写形式: %s \n", strupr(str));

```



### 结构体

```c
// 定义一个结构体
struct employee  
{   int id;  
    char name[50];  
    float salary;  
};

// 在main中声明一个结构体变量
struct employee e1;
// 赋值调用
e1.id = 110;

// 结构体数组
// 就是将结构体以数组形式保存
struct employee e[10]; // 相当于定义了10个结构体变量

// 结构体嵌套
struct Employee
{
    int id;
    char name[20];
    struct Date
    {
        int dd;
        int mm;
        int yyyy;
    }doj;
}e1;
// 赋值
e1.id = 110;
e1.doj.yyyy = 2019;
```

**联合体**

像结构体一样，联合体(`Union`)在C语言中是一个用户定义的数据类型，用于保存不同类型的元素。但它并不占所有成员的内存总和。它只占最大成员的内存，它分享最大成员的内存。



### 文件处理

常用函数：

| 编号 | 函数名称  | 功能描述             |
| ---- | --------- | -------------------- |
| 1    | fopen()   | 打开新的或现有的文件 |
| 2    | fprintf() | 将数据写入文件       |
| 3    | fscanf()  | 从文件读取数据       |
| 4    | fclose()  | 关闭文件             |

```c
// FILE *fopen( const char * filename, const char * mode );
// int fprintf(FILE *stream, const char *format [, argument, ...])
// int fscanf(FILE *stream, const char *format [, argument, ...])

#include <stdio.h>  
main() {
    FILE *fp;
    // 写文件
    fp = fopen("file.txt", "w");  
    fprintf(fp, "Hello World\n");
    fclose(fp);   
    // 读文件
    char buff[255]; 
    fp = fopen("file.txt", "r");  
    while(fscanf(fp, "%s", buff)!=EOF){  
        printf("%s ", buff );  
    }  
    fclose(fp); 
}
```



### 数学函数

`math.h`头文件中定义和实现有各种方法。`math.h`头文件的常用函数如下。

| 序号 | 函数                 | 描述                                             |
| ---- | -------------------- | ------------------------------------------------ |
| 1    | `ceil(number)`       | 舍入给定数字。它返回大于或等于给定数字的整数值。 |
| 2    | `floor(number)`      | 舍入给定数字。它返回小于或等于给定数字的整数值。 |
| 3    | `sqrt(number)`       | 返回给定数字的平方根。                           |
| 4    | `pow(base,exponent)` | 返回给定数字的幂值。                             |
| 5    | `abs(number)`        | 返回给定数字的绝对值。                           |

