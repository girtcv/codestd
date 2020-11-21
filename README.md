# <center>GIRT视觉组代码开发规范</center>

<p align="right"> <big>written by shing</big>

***

## 零、约定俗成

&emsp;&emsp;[强制]今后我们视觉组如果要开源代码, 为避免法律纠纷和商业侵权, 统一使用**GNU的开源许可证`[GPL]`开源协议**, **不要**使用`[MIT]`的开源协议。 当然, 如果你是以个人的名义开源代码, 还是比较推荐`[MIT]`开源许可证的, 但是一旦涉及到GIRT视觉组的代码, 必须`至少包含[GPL]`开源许可证。(*PS: 如果有赞助的话另说*)

&emsp;&emsp;为了我们在Linux和Windows操作系统上都能方便的阅读和编辑代码, 同时保证无论在哪一个平台下编辑和阅读代码都**不会乱码**。 我们的文本编码统一采用 **`UTF-8`** 的编码, 而不采用Windows系统默认的*GBK*编码, 因为**`UTF-8`**的编码兼容性、拓展性和可移植性都十分良好， 而且大多数Linux都是默认支持UTF-8, 而不支持GBK编码。

&emsp;&emsp;[强制]ISO C/C++标准规定: main函数的返回值必须为int类型，不允许返回值类型为void类型或其它类型(*严禁这种写法: void main(void)*)。  并且如果main函数执行成功则应该返回0, 失败则返回其它值。不允许颠倒黑白, 失败返回0, 成功返回其它值。 **`main函数有且仅有下面两种写法`**

```c
int main(void)//没有参数就写个void
{//返回值类型必须为int类型
    //code
    return 0;//这句不允许省略不写
}
int main(int argc, const char* argv[])
{
    //code
    return 0;//如果执行成功,返回0,失败返回其它值
}
```

***

## 一、命名风格

&emsp;&emsp;[推荐]**`一行只写一个变量`**, 尽量不要写多个变量, 既方便注释, 又清晰明了, 不容易出错。**`不要把多个类型的变量写在同一行`**

&emsp;&emsp;[强制]变量命名统一采用 **`英文命名`**, 要做到 **`见名知意`**

&emsp;&emsp;[强制]严禁使用*拼音与英文混合*的方式命名, 严禁使用*拼音缩写*命名, 更不允许直接使用*中文*命名。注意，即使纯拼音命名方式也要避免采用。

```c
//不推荐写法 稍有不注意,容易把y当成是普通类型的变量, 而不是指针变量
long x, *y, z = NULL;//不方便注释 不要把多个类型的变量写在同一行
long a = 0;     //a是什么???代表什么???
long shuzi = 0; //不推荐拼音命名, 但也允许用拼音命名
long fsgl = 0;  //禁止使用拼音缩写命名, 你能猜出这是发f射s功g率l吗?
long 数字 = 0;  //禁止使用中文命名, 不觉得这样很别扭吗?
//推荐写法  正确的英文拼写和语法可以让阅读者易于理解, 避免歧义。
long number = 0; //英文命名, 见名知意
size_t i = 0, j = 0;//for循环的i j可以写在同一行,没问题
```

&emsp;&emsp;[推荐]局部变量在声明时直接初始化, 避免引起非法内存引用错误。

&emsp;&emsp;[强制]指针类型的*要跟在类型后面, 不要跟在变量前面

```c
//推荐变量声明时初始化, 避免非法内存引用
int x = 0;
//指针变量推荐写法
int* p = NULL;
//不推荐写法
int *ptr = NULL;
```

&emsp;&emsp;[推荐]如果是个指针变量, 推荐在变量前面加个p_, 说明该变量是个指针(pointer)变量;

如果是个全局变量, 推荐在变量前面加个g_, 说明该变量是个全局(global)变量;

如果是个类的成员变量, 推荐在变量前面加个m_, 说明该变量是个成员(member)变量;

如果是个struct、~~union~~ 、enum, 推荐命名一律全部大写, 并且以下划线开头, 用typedef重命名。*struct强制, enum推荐, union不做上述要求*。

```c
//推荐写法
int g_count = 0;//前面加个g_, 说明它是全局变量

typedef struct _IMAGE_DOS_HEADER {//命名时用_开头
    short e_magic;
    int   e_lfanew;//typedef重命名时把下划线去掉
} IMAGE_DOS_HEADER, *PIMAGE_DOS_HEADER;//如果有必要顺便定义指针类型
//没必要就不必typedef重定义指针类型

typedef enum _COLOR{//命名时用_开头(enum没那么严,你不这样做也没事)
    RED_COLOR = 0,//enum成员一律大写, 有必要就赋值
    YELLOW_COLOR//没必要可以不赋值
} COLOR;//typedef重命名时把_去掉

int main(void) {
    int* p_FileBuffer` = nullptr; //前面加个p_说明这是一个指针变量
    return 0;
}
```

&emsp;&emsp;[推荐]定义类时,类名采用大驼峰命名法, 同时在类名前面加个**`大写字母C`**(Class的缩写)表示它是一个类, 写一个类时, 类的接口写在前面(public写在前面), 类的成员变量写在后面(private写后面)

&emsp;&emsp;[推荐]定义模板时, 推荐使用typename来定义泛型, 而不是class来定义泛型

&emsp;&emsp;[推荐]变量名推荐用小驼峰命名法, 函数名和类名推荐使用大驼峰命名法, 同时类名前面加个大写字母C(Class的缩写), 方便区分

```cpp
//推荐用template<typename T>
template<typename T>
//不推荐使用template<class T>
class CSmartPtr {//类名前面加个C并且每个单词首字母大写
public://类的接口写前面
    //@Constructor
    CSmartPtr(T* ptr = nullptr) :m_pObj(ptr);
    //@Destructor
    ~CSmartPtr();

    inline void AddRefcount(void) {
        ++m_refcount;
    }

private://类的实现写后面
    T* m_pObj;//m_p说明它是成员变量, 同时是指针变量
    unsigned long m_refcount;//前面加个m_, 说明它是成员变量
};
//TestSmartPrt: 大驼峰命名法: 每个单词首字母大写
void TestSmartPrt(void)//函数 类名采用大驼峰命名法
{//smartPtr: 小驼峰命名法: 第一个单词首字母小写,其余单词首字母大写
    CSmartPtr<int> smartPtr(new int);//变量采用小驼峰命名法
}
```

[GPL]:<http://www.gnu.org/licenses/gpl-3.0.html>
