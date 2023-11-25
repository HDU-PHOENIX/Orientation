# 代码规范

首先指出，我们今天讲的代码规范只是纠正大家刚开始写代码时的一些不良习惯，不涉及到代码的风格问题，只要自己觉得舒服就行。

## 命名

### 变量命名

在看大家第一次考核提交上来的代码时，我们注意到了以下变量命名的问题：

```C
int a,b,c,d,e,f;    // 单字母变量
int a1,a2,a3,a4;    // 含糊不清的变量名
int shuzu[10],wei;  // 拼音变量名
int minwei;         // 拼音+英文变量名
```

这些都是不合适的命名。变量名应该**尽可能的清晰明了**，让人一眼就能看出这个变量的作用。比如：

```C
int temp; // 用于暂存的变量
int sum;  // 用于求和的变量
float tempurature; // 温度
```

那么这里给大家提出一点要求：在命名变量时，要求变量名的长度**不少于3个字符**，且要求变量名能够**清晰的表达出变量的作用**。

例如这样的命名也是不合适的：

```C
int aaa,bbb,ccc,ddd,eee,fff;
int qwe;
int jflskgjwe;
```

当然对于约定俗成的变量名，如循环变量`i`，`j`，`k`等，可以就用其约定俗成的变量名。

### 函数命名

函数命名的要求和变量命名的要求类似，要求**尽可能的清晰明了**，让人一眼就能看出这个函数的作用。比如：

```C
void printArray(int *array, int length); // 打印数组
void swap(int *a, int *b); // 交换两个数
void quickSort(int *array, int left, int right); // 快速排序
```

同样的，函数名的长度也要求**不少于3个字符**，且要求函数名能够**清晰的表达出函数的作用**。

## 注释

C 语言中的注释有两种：
```C
// 单行注释，从 // 开始到行末都是注释，又称行注释

/*
 * 多行注释
 * 从 / * 开始到 * / 都是注释
 * 又称块注释
 */
```

注释的作用是让人更容易理解代码，所以注释的内容应该是**对代码的解释**，而不是对代码的重复。比如：

```C
void swap(int *a, int *b) { // 交换两个数
    int temp = *a;      // 存下 a 的值
    *a = *b;            // 将 b 的值赋给 a
    *b = temp;          // 将 a 的值赋给 b
}

void quickSort(int *array, int left, int right) {   // 快速排序算法
    
    if(left >= right) { // 递归边界，如果左边界大于等于右边界，直接返回
        return;
    }

    int i = left,j = right; // i,j分别指向数组的左右边界，设定基准数为最左边的数
    while(i < j) {
        while(i < j && array[j] >= array[left]) {   // 从右往左找第一个小于基准数的数
            j--;
        }
        while(i < j && array[i] <= array[left]) {   // 从左往右找第一个大于基准数的数
            i++;
        }
        swap(&array[i], &array[j]);     // 交换两个数
    }
    swap(&array[i], &array[left]);      // 将基准数放到正确的位置
                                        // 此时 i 指向的数一定小于基准数

    quickSort(array, left, i-1);        // 递归处理左边的数
    quickSort(array, i+1, right);       // 递归处理右边的数
}
```

一些 IDE 会让注释更人性化，比如在 VSCode 中注释（需要安装 C/C++ 插件）：
```C
/**
 * @brief 快速排序算法
 * @param array 待排序数组
 * @param left 数组左边界
 * @param right 数组右边界
*/
void quickSort(int *array, int left, int right);
```
这样写的好处是，当你在写代码时，鼠标放在函数名上，就能看到函数的注释，这样就能更好的理解函数的作用。

## 缩进、空格

在 C 语言中，缩进和空格都是没有语法意义的，但是缩进和空格能够让代码更加清晰明了，所以我们要求大家在写代码时**必须缩进**，**必须使用空格**。  
> 为什么不使用 Tab 键？
> 
> 因为不同的编辑器对 Tab 键的处理方式不同，有的会将 Tab 键转换为空格，有的会将 Tab 键转换为 4 个空格，有的会将 Tab 键转换为 8 个空格。  
> 同时，Tab 有时候会导致代码在不同的编辑器中显示不一致，所以我们建议大家使用空格。  
> ps: 不是手打空格，而是建议将 Tab 设置成 4 个空格。

例如下面的代码就是不合格的：

```C
// 没有缩进
int adder(int a, int b) {
int sum;
sum = a + b;
return sum;
}
```
```C
// 缩进不统一
void adder(int *a, int *b) {
    int sum = 0;
        for(int *p = a; p <= b; p++) {
        sum += *p;
    }
return sum;
}
```

**此外，在运算符两边都要加上空格，逗号后面也要加上空格。如前面的代码。**

## 未定义行为

### 自增自减运算符

在 C 语言中，有一些行为是未定义的，也就是说，这些行为的结果是不确定的，可能在不同的编译器中结果不同，也可能在同一个编译器中结果不同。  
例如，谭浩强老师的《C程序设计》中就提到了这样一个例子：

```C
int a = 1;
printf("%d,%d\n", a++, a++);
```

这段代码的输出结果是不确定的，可能是 `1,2`，也可能是 `2,1`，也可能是 `1,1`，也可能是 `2,2`。

或者我们来看这个例子：
```C
int a = 1;
int b = a++ + ++a;
printf("%d\n", b);
```
这段代码的结果也是不确定的，可能是 `3`，也可能是 `4`，也可能是 `5`。

首先一点就是，C 语言中自增运算符 `++` 和自减运算符 `--` **每条语句中只能出现一次**。

### 函数声明

注意这段代码：

```C
#include <stdio.h>
int main(void)
{
    int a = adder(1, 2);
    printf("%d", a);
    return 0;
}

int adder(a, b) {
    return a + b;
}
```

在 `gcc` 下编译运行这段代码，会得到一个警告，但是程序仍然可以正常运行。原因是在未声明而使用函数 `adder` 时，编译器会默认函数的返回值为 `int`；未指定函数参数类型时，编译器会默认函数参数类型为 `int`。 
如下面两段代码：
```C
#include <stdio.h>
int main(void)
{
    long a = adder(1, 2);
    printf("%ld", a);
    return 0;
}

long adder(a, b) {
    return a + b;
}
// 会编译报错，函数定义与函数声明不一致
```
```C
#include <stdio.h>
float adder(a, b) {
    return a + b;
}
int main(void)
{
    float a = adder(1.1, 1.2);
    printf("%f", a);
    return 0;
}
// 编译通过，但是运行结果错误
// 预期结果 2.3
// 实际结果 -546132160.000000
```

所以我们要求大家函数必须**先声明后使用**，若函数声明与定义分开，则函数声明与函数定义必须一致，函数声明和定义**写清楚函数的返回值类型和参数类型**。