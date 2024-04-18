代码题 ：

#### 1、链表逆置

~~~C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* pcur = head;
    struct ListNode* ppre = NULL;
    while(pcur)
    {
        struct ListNode* next = pcur->next;
        pcur->next = ppre;
        ppre = pcur;
        pcur = next;
    }
    return ppre;
}
~~~



#### 2、链表删除节点

~~~C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

//这是只删除一个节点的方法，如果有重复节点，就要一直遍历完链表删
struct ListNode* deleteNode(struct ListNode* head, int val){
    struct ListNode *pcur = head;
    struct ListNode *ppre = NULL;
    struct ListNode *pnext = NULL;
    //找val的位置
    while (pcur && pcur->val != val)
    {
        ppre = pcur;
        pcur = pcur->next;
    }
    //遍历到最后一个，没有找到val，直接返回
    if (NULL == pcur)
    {
        return head;
    }
    //是头的话去掉头
    if (pcur == head)
    {
        head = pcur->next;
        return head;
    }
    pnext = pcur->next;
    //free(pcur);
    //pcur = NULL;
    ppre->next = pnext;
    return head;
}


//删除重复节点的方式

struct ListNode* deleteNode(struct ListNode* head, int val){
    struct ListNode *pcur = head;
    struct ListNode *ppre = NULL;
    struct ListNode *tmp = NULL;
    while(pcur)
    {
        if (head->val == val)
        {
            head = head->next;
            pcur = head;
            continue;
        }
        if (pcur->val == val)
        {
            tmp = pcur;
            ppre->next = pcur->next;
            pcur = pcur->next;
            free(tmp);
            tmp = NULL;
        }else
        {
            ppre = pcur;
            pcur = pcur->next;
        }

    }
    return head;
}
~~~



#### 3、链表头插法和尾插法

~~~C 
//头插法
void headInsert(pNode* pphead, pNode* pptail, int value)
{
    pNode newNode = (pNode)malloc(sizeof(Node));
    memset(newNode, 0, sizeof(Node));
    newNode->value = value;

    if (NULL == *pphead)
    {
        *pphead = newNode;
        *pptail = newNode;
    }else
    {
        newNode->next = *pphead;
        *pphead = newNode;
    }
}

//尾插法
void tailInsert(pNode* pphead, pNode* pptail, int value)
{
    pNode newNode = (pNode)malloc(sizeof(Node));
    memset(newNode, 0, sizeof(Node));
    newNode->value = value;

    if (NULL == *pphead)
    {
        *pphead = newNode;
        *pptail = newNode;
    }else
    {
        (*pptail)->next = newNode;
        *pptail = newNode;
    }
}

//有序插入,升序
void orderInsert(pNode* pphead, pNode* pptail, int value)
{
    pNode newNode = (pNode)malloc(sizeof(Node));
    memset(newNode, 0, sizeof(Node));
    newNode->value = value;

    pNode pcur = NULL;
    pNode ppre = NULL;     //用ppre吧，这样就可以直接使用while(pcur)，不用考虑其他边界情况
    if(NULL == *pphead)
    {
        *pphead = newNode;
        *pptail = newNode;
    }else
    {
        if (newNode->value <= (*pphead)->value)
        {
            //头插法
            newNode->next = *pphead;
            *pphead = newNode;
        }else if (newNode->value >= (*pptail)->value)
        {
            //尾插法
            (*pptail)->next = newNode;
            *pptail = newNode;
        }else
        {
            //中间插入
            pcur = *pphead;
            while (pcur)
            {
                if (newNode->value < pcur->value)
                {
                    ppre->next = newNode;
                    newNode->next = pcur;
                    return;
                }
                ppre = pcur;
                pcur = pcur->next;
            }  
        }
    }
}
~~~



#### 4、判断单链表是个环，只给了头结点

~~~C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool hasCycle(struct ListNode *head) {
    struct ListNode* slow = head;
    struct ListNode* fast = head;
    //快慢指针移动，如果有环，快指针肯定能追上慢指针
    while(fast)
    {
        fast = fast->next;
        if (fast)
        {
            fast = fast->next;
        }
        if (fast == slow)
        {
            return true;
        }
        slow = slow->next;
    }
    return false;
}
~~~



#### 5、将两个字符串合并为一个字符串

~~~C
char * mergeString(char* str1, char* str2)
{
    int len1 = strlen(str1);
    printf("len1 = %d\n", len1);    //strlen计算不包括\0
    int len2 = strlen(str2);
    char *buf = (char*)malloc(sizeof(char)*(len1 + len2 + 1));
    memset(buf, 0, sizeof(char) * (len1 + len2 + 1));
    sprintf(buf, "%s %s", str1, str2);
    return buf;
}

//strcat源码实现，dst的空间是确保足够的
char *strcat (char * dst, const char * src)
{
assert(NULL != dst && NULL != src);   // 源码里没有断言检测
    char * cp = dst;
    while( *cp )
         cp++;       /* find end of dst */
    while( *cp++ = *src++ ) ;       /* Copy src to end of dst */
    return( dst );                  /* return dst */
}
~~~



#### 6、写一个冒泡排序，如何用指针实现冒泡排序

~~~C
void bubbleSort(int* buf, int len)
{
    for(int i = 0; i < len - 1; i++)    //外层控制比较的次数
    {
        bool flag = false;              //表示数据是否有交换，没有交换的话直接退出
        for (int j = 1; j < len - i; j++)   //内层控制比较的元素
        {
            if (buf[j] < buf[j - 1])
            {
                int tmp = buf[j - 1];
                buf[j - 1] = buf[j];
                buf[j] = tmp;
                flag = true;
            }
        }
        if (!flag)
        {
            break;      //没有数据交换，提前退出
        }
    }
}

//指针实现
void bubbleSort1(int* buf, int len)
{
    for(int i = 0; i < len - 1; i++)    //外层控制比较的次数
    {
        for (int j = 1; j < len - i; j++)   //内层控制比较的元素
        {
            if (*(buf + j) < *(buf + j - 1))
            {
                int tmp = *(buf + j - 1);
                *(buf + j - 1) = *(buf + j);
                *(buf + j) = tmp;
            }
        }
    }
}
~~~



#### 7、写一个快排(O(nlogn))

~~~C
void swap(int* buf, int a, int b)
{
    int tmp = buf[a];
    buf[a] = buf[b];
    buf[b] = tmp;
}

int partition(int* buf, int left, int right)
{
    //left的左边是小于buf[povit]的值，右边是大于buf[povit]的值
    int povit = right;      //比较值先选为最右边的值
    int tmp = 0;
    int idx = left;
    while (idx < right)
    {
        if (buf[idx] < buf[povit])
        {
            swap(buf, idx, left);
            left++;
        }
        idx++;
    }
    swap(buf, left, povit);
    return left;        //返回的是下标值
}

void quickSortPart(int* buf, int left, int right)
{
    //递归跳出条件，不要忘记写了
    if (left >= right)
    {
        return;
    }
    int povit = partition(buf, left, right);
    quickSortPart(buf, 0, povit - 1);
    quickSortPart(buf, povit + 1, right);
}

void quickSort(int* buf, int len)
{
    quickSortPart(buf, 0, len - 1);
}
~~~



#### 8、哪些排序是稳定排序；排序稳定度的定义

~~~
稳定排序：基数排序、桶排序、冒泡排序、直接插入排序、折半插入排序、归并排序
不稳定的排序：堆排序、快速排序、希尔排序、直接选择排序
排序稳定度：排序前后两个相等的数相对位置不变，则算法稳定。
~~~

![image-20240331201136150](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240331201136150.png)

#### 9、二分查找

~~~c
//有序数组，二分查找，返回查找到的下标
int binSearch(int* nums, int numsSize, int target) {
    int left = 0, right = numsSize - 1;
    int mid = 0;
    while (left <= right)   //注意这里是<=，不是<
    {
        mid = left + (right - left) / 2;
        //mid = low + ((high - low) >> 1);  //位运算更快
        if (nums[mid] < target)
        {
            left = mid + 1;
        }else if(nums[mid] > target)
        {
            right = mid - 1;
        }else
        {
            return mid;
        }
    }
    return -1;
}
~~~



#### 10、strcpy的源码

~~~C
//n通常是des的长度 - 1，因为如果strlen(src) > strlen(des),最后不会自动添加‘\0’
char* mystrncpy(char* des, const char* src, int n)
{
    int i;
    for (i = 0; i < n && src[i]!= '\0'; i++)
    {
        des[i] = src[i];
    }
    for (; i < n; i++)
    {
        des[i] = '\0';
    }
    return des;

}

//调用的
mystrncpy(buf, buf1, sizeof(buf)/sizeof(char) - 1);
buf[20] = '\0';     //手动加上结束符
~~~



#### 11、手写strcmp

~~~C
//库函数
int strcmp(const char *s1, const char *s2)
{
    for ( ; *s1 == *s2; s1++, s2++)
        if (*s1 == '\0')
            return 0;
    return ((*(unsigned char *)s1 < *(unsigned char *)s2) ? -1 : +1);
}

//自己写的
int mystrcmp(char* src, char *des)
{
    int i = 0;
    while (src[i] == '\0' && des[i] == '\0')
    {
        if (src[i] == des[i])
        {
            continue;
        }else if(src[i] < des[i])
        {
            return -1;
        }else
        {
            return 1;
        }
    }
    return 0;
}

~~~



#### 12、判断大小端的代码

大端存储：低地址存放高字节，高地址存放低字节

小端存储：低地址存放低字节，高地址存放高字节

~~~C
#include<stdio.h>
int check_sys()
{
    int i = 1;     
    return (*(char*)&i);
}
int main()
{
    int ret = check_sys();
    if (ret == 1)
    {
        printf("小端\n");
    }
    else
    {
        printf("大端\n");
    }
    return 0;
}
~~~



#### 13、有 10GB 的订单数据，我们希望按订单金额（假设金额都是正整数）进行排序，但是我们的内存有限，只有几百 MB，没办法一次性把 10GB 的数据都加载到内存中。这个时候该怎么办呢？

~~~
基本想法：使用桶排序（10G的数据，内存只有100MB，那桶的数据最少就是100个）
1、首先，找到订单数据的最大和最小数据，打开文件，每次读一行，一直读到文件尾，找到最大值和最小值
2、假设订单数据是在1-100000元之间，那就按照每1000元作为一个桶，0-999，1000-1999...，这样就可以划分为100个桶，这里的桶就是100个文件
3、理想情况下，订单数据分配均匀，那就是100个文件，每个文件大小为100M，这个时候就可以将文件读入内存，使用快排进行排序，排序完再全部输入一个文件就行了
4、但是实际上，数据不可能均匀分布，100G的数据是不可能均匀划分到100个文件中的，有可能某个文件特别大，没有办法一次读入内存，那就要另外处理
5、针对这些大文件，还可以继续划分，比如0-999的文件太大，就可以继续分为0-99,100-199...，直到数据能够读入内存
~~~







### C/C++相关的题：

#### 1、sizeof字符串，问输出是几，还有strlen和sizeof的区别，以及sizeof用在数组名和指针上的区别

- sizeof字符串输出的是指针的大小，64位是8个字节
- sizeof就是一个计算数据类型所占空间大小的单目运算符，在计算字符串的空间大小时，**包含了结束符\0的位置**；而strlen是一个计算字符串长度的函数，使用时需要引用头文件#include <string.h>，**不包含\0,即计算\0之前的字符串长度**
- sizeof(数组名)是数组空间的大小，一般计算数组的长度是sizeof(arr)/sizeof(char)



#### 2、堆和栈的区别

- 堆是低地址向高地址扩展，栈是由高地址向低地址扩展
- 堆中的内存需要手动申请和手动释放，栈中的内存是由操作系统自动申请和释放，存放着参数，局部变量等内存
- 堆中频繁的malloc/free会产生内存碎片，降低程序效率；而栈由于其先进后出的特性，不会产生内存碎片
- 堆的分配效率低，栈的分配效率高
- 栈分配效率高的原因：栈是操作系统提供的数据结构，计算机底层做了一系列的支持：分配专门的寄存器存储栈的地址，入栈和压栈有专门的指令执行；而堆是由C/C++函数库提供的，机制复杂，需要一系列分配内存、合并内存、释放内存的算法，因此效率比较低



#### 3、C++的虚函数和纯虚函数

- 虚函数：成员函数前面添加virtual关键字
- 纯虚函数：虚函数=0；比如：virtual print() =0;



#### 4、static

- 不考虑类的情况
  - 隐藏：所有不加 static 的全局变量和函数具有全局可见性，可以在其他文件中使用，加了之后只能在该文件所在的编译模块中使用
  - **默认初始化为0**，包括未初始化的全局静态变量与局部静态变量，都存在全局未初始化区
  - 静态变量在函数内定义始终存在，且只**进行一次初始化**，具有记忆性，其作用范围与局部变量相同，函数退出后仍然存在，但不能使用
- 考虑类的情况
  - static 成员变量：只与类关联，**不与类的对象关联**。定义时要分配空间，不能在类声明中初始化，**必须在类定义体外部初始化**，初始化时不需要标示为static；可以被非 static 成员函数任意访问
  - static 成员函数：不具有`this`指针，无法访问类对象的非 static 成员变量和非 static 成员函数；**不能被声明为const、虚函数和volatile**；可以被非 static 成员函数任意访问



#### 5、const

- 不考虑类的情况

  - const 常量在**定义时必须初始化**，之后无法更改

  - const 形参可以接收const和非const类型的实参，例如

    ```C++
    // i 可以是 int 型或者 const int 型
    void fun(const int& i) {}
    ```

- 考虑类的情况

  - const 成员变量：不能在类定义外部初始化，只能通过构造函数 **初始化列表** 进行初始化，并且必须有构造函数；不同类对其 const 数据成员的值可以不同，所以不能在类中声明时初始化
  - const 成员函数：实际修饰该成员函数隐含的 this 指针，**表明在该成员函数中不能对类的任何成员进行修改**



#### 6、一般对C++的类来说，memory Layout有哪些成分？C++的对象在内存上的长什么样？

~~~
通常分为四个区：
栈区：存放函数运行分配的局部变量，函数参数，返回数据，返回地址等
堆区：new或者malloc申请的空间
全局/静态存储区：全局变量，静态数据
文字常量区：常量在统一运行被创建，常量区的内存是只读的，程序结束后由系统释放。
代码区：类的成员函数(包括静态成员函数和非静态成员函数)和非成员函数代码存放在代码区

在类的定义时：
1、类的成员函数放在代码区
2、类的静态成员变量在全局数据区
3、非静态成员变量在类的对象内，对象在栈区或者堆区
4、虚函数指针和虚基指针在类的对象中，对象在栈中或者堆中
类的对象如果是定义的类局部变量，则在栈内存区，如果是new出来的类指针，则在堆内存区，同时引用会保存在栈里
	为何这样设计？其实这是从c语言发展而来的。类的成员变量相当于c的结构体，类的成员函数类似于c的函数，类的静态变量类似于c的静态或全局变量，至于虚函数，函数体还是放在代码区，但虚函数的指针和成员变量一起放在数据区，这是因为虚函数的函数体有多个，不同的子类调用同一虚函数实则调用的不同函数体，因此需要在类的数据区保持真正的虚函数的指针。
	类的静态成员函数所有对象共享
~~~



#### 7、static global local的生存周期

- **生存周期:** 变量从定义到销毁的时间范围。存放在全局数据区的变量的生存周期存在于整个程序运行期间，而存放在栈中的数据则随着函数等的作用域结束导致出栈而销毁，除了静态变量之外的局部变量都存放于栈中。取决于变量存放位置。
  - static变量：从程序开始到程序结束，程序结束过后释放
  - global变量：从程序开始到程序结束，程序结束后释放
  - local变量：从程序开始到程序结束，函数体结束后释放，除了局部静态变量



#### 8、pointer和reference的区别

- 指针是一个变量，存储的是一个地址，引用可以说是变量的别名，操作的时候就是操作变量本身
- 指针可以多级，引用只有一级
- 指针可以为空，引用一旦定义了必须初始化
- 指针可以改变指向，引用一旦初始化了就不能改变指向(被限制的指针)
  	因为引用的本质就是指针常量，int &ref = a; 实际为 int * const ref = &a;
- sizeof`指针得到的是本指针的大小，`sizeof`引用得到的是引用所指向变量的大小
- 引用本质是一个指针，同样会占8字节(或者4字节)内存



#### 9、虚函数占据内存吗？

~~~
具有虚函数的类有一个虚函数指针，虚函数指针指向虚函数表，虚函数表存放虚函数入口地址，位于程序代码区，除了虚函数指针，虚函数不占据内存
~~~



#### 10、动态多态怎么实现的

- 基类定义虚函数
- 派生类重载虚函数
- 基类指针指向派生类对象
- 基类指针调用虚函数



#### 11、C和C++的区别

- C++是面向对象的语言，而C是面向过程的结构化编程语言
- C++具有重载、继承和多态三种特性
- C++相比C语言，具有很多类型安全的功能，比如强制类型转换
- C++支持范式编程，比如类模板，函数模板等



#### 12、new和malloc的区别

- new/delete是C++关键字，需要编译器支持。malloc/free是库函数，需要头文件支持；
- 使用new操作符申请内存分配时**无须指定内存块的大小**，编译器会根据类型信息自行计算。而malloc则需要显式地指出所需内存的尺寸；
- new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，**无须进行类型转换**，故new是符合类型安全性的操作符。而malloc内存分配成功则是返回`void *`，需要通过强制类型转换将`void *`指针转换成我们需要的类型；
- new内存分配失败时，会抛出bac_alloc异常。malloc分配内存失败时返回NULL；
- new会先调用operator new函数，申请足够的内存（通常底层使用malloc实现）。然后调用类型的构造函数，初始化成员变量，最后返回自定义类型指针。delete先调用析构函数，然后调用operator delete函数释放内存（通常底层使用free实现）。malloc/free是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。

#### 13、如果我想使得申请失败后返回一个空指针，需要怎么做？

```
使用 nothrow new 来申请内存。
```



#### 14、介绍一下面向对象的特性

~~~
封装、继承、多态
~~~



#### 15、比较一下面向对象和面向过程

~~~
面向过程：
优点：效率高，因为不需要实例化对象。
缺点：耦合度高，扩展性差，不易维护（例如：每个步骤都要有，不然就不行）

面向对象：
优点：耦合低（易复用），扩展性强，易维护，由于面向对象有封装、继承、多态性的特点，可以设计出低耦合的系统，使系统更加灵活、更加易于维护。
缺点：效率比面向过程低
~~~



#### 16、函数指针和指针函数，哪里会用到他们

~~~~
1、指针函数：就是一个返回指针的函数，其本质是一个函数，而该函数的返回值是一个指针。
int *fun(int x,int y);

2、函数指针：指向函数的指针
int (*fun)(int x,int y);
函数指针是需要把一个函数的地址赋值给它
fun = &Function；
fun = Function;		//函数名表示指针地址
函数指针的调用：
x = (*fun)(参数);
x = fun();
~~~~



#### 17、C++怎么嵌入C代码

~~~C
#ifdef __cplusplus
extern "C"{
#endif
　　int Max(int nA,int nB);
#ifdef __cplusplus
};
#endif
~~~

~~~
总结：C++中调用C代码的3种具体实现方式
1.修改C代码的头文件,当其被用于C++代码时，在声明中加入exter "C" 上例中在Max.h中加入
extern "C" int Max(int nA,int nB);

2.在C++代码中(Main.cpp)重新声明一下C函数,在重新声明时添加extern "C".
#include "Max.h"
extern "C" int Max(int nA,int nB);

3.在包含C头文件(Main.cpp)时，添加extern "C".
extern "C"{
#include "Max.h"
}
注意：extern "C" 一定要加在C++的代码文件中才起作用
~~~



#### 18、C++如何避免拷贝构造

~~~
声明为private：
Point(const Point& rhs) = delete;
~~~



#### 19、hash主要用在什么场合

~~~
1、场景一：安全加密
日常用户密码加密通常使用的都是 md5、sha等哈希函数，因为不可逆，而且微小的区别加密之后的结果差距很大，所以安全性更好。
2、场景二：唯一标识
比如 URL 字段或者图片字段要求不能重复，这个时候就可以通过对相应字段值做 md5 处理，将数据统一为 32 位长度从数据库索引构建和查询角度效果更好，此外，还可以对文件之类的二进制数据做 md5 处理，作为唯一标识，这样判定重复文件的时候更快捷。
3、场景三：数据校验
比如从网上下载的很多文件（尤其是P2P站点资源），都会包含一个 MD5 值，用于校验下载数据的完整性，避免数据在中途被劫持篡改。
4、场景五：散列函数
前面已经提到，PHP 中的 md5、sha1、hash 等函数都是基于哈希算法计算散列值
5、场景五：负载均衡
对于同一个客户端上的请求，尤其是已登录用户的请求，需要将其会话请求都路由到同一台机器，以保证数据的一致性，这可以借助哈希算法来实现，通过用户 ID 尾号对总机器数取模（取多少位可以根据机器数定），将结果值作为机器编号。
6、场景六：分布式缓存
分布式缓存和其他机器或数据库的分布式不一样，因为每台机器存放的缓存数据不一致，每当缓存机器扩容时，需要对缓存存放机器进行重新索引（或者部分重新索引），这里应用到的也是哈希算法的思想。
~~~



#### 20、为什么C++可以函数重载，C不行 

~~~
因为C++中的名字改编
~~~



正则表达式

### 网络相关：

#### 1、udp处于哪一层

~~~
传输层
五层：
应用层
传输层
网络层
数据链路层
物理层
~~~



#### 2、TCP和UDP区别？

~~~
1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付
3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）
4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
5、TCP首部开销20字节;UDP的首部开销小，只有8个字节
6、TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道
~~~



#### 3、socket的惊群效应？

~~~
惊群效应（thundering herd）是指多进程（多线程）在同时阻塞等待同一个事件的时候（休眠状态），如果等待的这个事件发生，那么他就会唤醒等待的所有进程（或者线程），但是最终却只能有一个进程（线程）获得这个事件的“控制权”，对该事件进行处理，而其他进程（线程）获取“控制权”失败，只能重新进入休眠状态，这种现象和性能浪费就叫做惊群效应。
~~~



#### 4、有可读事件，read返回0是什么情况

~~~
read() == 0;	表示对端关闭了socket
read() > 0;		表示从缓冲区读取到的字节数
read() < 0;		表示出错，判断errno
如果是EINTR，表示系统中断，直接忽略
如果errno == EAGAIN或者EWOULDBLOCK，非阻塞socket直接忽略;如果是阻塞的socket,一般是读写操作超时了，还未返回，这个超时是指socket的SO_RCVTIMEO与SO_SNDTIMEO两个属性。所以在使用阻塞socket时，不要将超时时间设置的过小。不然返回了-1，你也不知道是socket连接是真的断开了，还是正常的网络抖动。一般情况下，阻塞的socket返回了-1，都需要关闭重新连接
~~~



#### 5、怎么优化close_wait问题

![image-20240402201541747](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240402201541747.png)

- **close_wait产生的原因：**
  - **CLOSE_WAIT**是被动关闭连接是形成的，根据TCP状态机，[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)端收到客户端发送的FIN，TCP协议栈会自动发送ACK，连接进入**CLOSE_WAIT**状态。但如果服务器端不执行SOCKET的**CLOSE()**操作（比如I/O线程被意外阻塞），状态就不能由**CLOSE_WAIT**迁移到**LAST_ACK**，则系统中会存在很多**CLOSE_WAIT**状态的连接.
  - 通常，**CLOSE_WAIT** 状态在服务器停留时间很短，如果你发现大量的 **CLOSE_WAIT** 状态，那么就意味着被动关闭的一方没有及时发出 ***FIN*** 包，一般有如下可能：
  - **(1) 程序问题**：如果代码层面忘记了 **CLOSE** 相应的 socket 连接，那么自然不会发出 FIN 包，从而导致 **CLOSE_WAIT** 累积；或者代码不严谨，出现死循环之类的问题，导致即便后面写了 **CLOSE** 也永远执行不到。
  - **(2) 响应太慢或者超时设置过小**：如果连接双方不和谐，一方不耐烦直接 timeout，另一方却还在忙于耗时逻辑，就会导致 close 被延后。响应太慢是首要问题，不过换个角度看，也可能是 timeout 设置过小。



#### 6、怎么优化time_wait问题

- ##### time_wait的作用：

  简单说timewait之所以等待2MSL的时长，是为了避免因为网络丢包或者网络延迟而造成的tcp传输不可靠，而这个**TIME_WAIT**状态则可以最大限度的提升网络传输的可靠性。

  同时TCP一般会禁止处于**TIME_WAIT**的连接上重建一个新的TCP连接, 这样做主要是为了避免新旧数据包出现串包的情况, 所以总结来说, TIME_WAIT的作用如下:

  - 为实现TCP全双工连接的可靠释放
  - 为使旧的数据包在网络因过期而消失

- ##### TIME_WAIT状态过多的危害
  - 在socket的TIME_WAIT状态结束之前，该socket所占用的本地端口号将一直无法释放。请注意客户端的端口总是有限的(65535), 耗尽了就会导致网络连接失败.
  - 在高并发（每秒几万qps）并且采用短连接方式进行交互的系统中运行一段时间后，系统中就会存在大量的time_wait状态，如果time_wait状态把系统所有可用端口都占完了且尚未被系统回收时，就会出现无法向服务端创建新的socket连接的情况。此时系统几乎停转，任何链接都不能建立。
  - 大量的time_wait状态也会系统一定的fd，内存和cpu资源，当然这个量一般比较小，并不是主要危害

- **优化方法：**

  - 方式一: 调整系统内核参数(地址复用)

    ~~~
    net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
    net.ipv4.tcp_tw_recycle = 1 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
    ~~~

  - 方式二：调整短链接为长链接

    ~~~
    长连接比短连接从根本上减少了关闭连接的次数，减少了TIME_WAIT状态的产生数量，在高并发的系统中，这种方式的改动非常有效果，可以明显减少系统TIME_WAIT的数量。
    ~~~

    

#### 6、socket通信,客户端和服务器的操作步骤？

**UDP：**

![image-20240402205140352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240402205140352.png)



#### 7、UDP在linux里面是咋实现



### 操作系统概念题：

#### 1、系统抖动是什么?

```
在请求分页存储管理中，从主存（DRAM）中刚刚换出（Swap Out）某一页面后（换出到Disk），根据请求马上又换入（Swap In）该页，这种反复换出换入的现象，称为系统颠簸，也叫系统抖动。产生该现象的主要原因是置换算法选择不当。
1.如果分配给进程的存储块数量小于进程所需要的最小值，进程的运行将很频繁地产生缺页中断，这种频率非常高的页面置换现象称为抖动。解决方案优化置换算法。

2.在请求分页存储管理中，可能出现这种情况，即对刚被替换出去的页，立即又要被访问。需要将它调入，因无空闲内存又要替换另一页，而后者又是即将被访问的页，于是造成了系统需花费大量的时间忙于进行这种频繁的页面交换，致使系统的实际效率很低，严重导致系统瘫痪，这种现象称为抖动现象
```



#### 2、uboot是怎么到内存中并执行的

~~~
uboot 属于bootloader的一种，是用来引导启动内核的，它的最终目的就是，从flash中读出内核，放到内存中，启动内核。
~~~



#### 3、条件变量怎么使用的

```
pthread_cond_wait等待条件满足，收到pthread_cond_signal信号过后检查条件是否满足，满足的话执行逻辑，然后解锁，比如买票和产票，票为0的时候signal通知产票线程，大于0的时候产票线程就阻塞睡眠
```



#### 4、进程的几种状态

1. 创建状态
2. 就绪状态
3. 运行状态
4. 阻塞状态
5. 终止状态

![image-20240402213531287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240402213531287.png)



#### 5、 一个可执行程序的内存分布情况

![image-20240402213639448](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240402213639448.png)



#### 6、kernel启动的第一个函数名叫什么？

~~~
start_kernel函数是所有Linux平台进入系统内核初始化的入口函数。 它的主要工作是完成剩余与硬件平台相关的初始化工作，在进行一系列与内核相关的初始化之后，调用第一个用户进程init并等待其执行。 至此，整个内核启动完成
~~~



#### 7、虚拟内存

~~~
段页式存储
~~~



#### 8、为什么内核区要放在虚拟内存的高位地址

1. 内核空间需要频繁访问硬件设备，而这些设备的寄存器通常被映射到物理内存的高地址区域，因此将内核空间放置在高地址区可以更快地访问这些设备。
2. 内核空间需要保护，以防止用户空间的程序对其进行非法访问。将内核空间放置在高地址区可以使其与用户空间的程序分离，从而更容易实现保护。
3. 内核空间需要占用大量的内存，而将其放置在高地址区可以使其与用户空间的程序分离，从而避免与用户空间的程序发生冲突。
   



#### 9、不指定的情况下优先链接静态库还是动态库

~~~
gcc test.cpp -L. -ltestlib			-L：指定库目录 -l：链接库 -I：头文件的目录，别搞错了
如果当前目录有两个库libtestlib.so libtestlib.a 则肯定是连接libtestlib.so。
如果要指定为连接静态库则使用：
gcc test.cpp -L. -static -ltestlib
~~~



#### 10、linux中遇到文件无法删除怎么解决

~~~
1、使用 lsattr 命令查看文件的附加属性。查看文件是否被赋予了 a , i 属性，如果含有这两个属性，文件是不能被删除的。
    a：让文件或目录仅供附加用途；
    b：不更新文件或目录的最后存取时间；
    c：将文件或目录压缩后存放；
    d：将文件或目录排除在倾倒操作之外；
    i：不得任意更动文件或目录；
    s：保密性删除文件或目录；
    S：即时更新文件或目录；
    u：预防意外删除。

2、使用 chattr 改变文件的附加属性，去掉 a, i 属性，文件即可被删除。
	chattr -i 文件路径

~~~


