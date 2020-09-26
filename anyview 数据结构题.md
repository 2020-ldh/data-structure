# anyview 数据结构题：

## 第1章：绪论

### 1、三数交换

```c
/**********
【题目】试写一算法，如果三个整数a，b和c的值
不是依次非递增的，则通过交换，令其为非递增。
***********/
void Descend(int &a, int &b, int &c) //这里的引用改变a的值也会改变n的值
/* 通过交换，令 a >= b >= c */
{
    /*这里的&a是说给一个变量起一个别名，a的值改变，
    原变量即实参的值也改变，但是在dev c++编译不通过*/
    int temp;
    if(a < b){
        temp = a;
        a = b;
        b = temp;
    }
    
     if(a < c)
     {
       temp = a;
        a = c;
        c = temp;
     }
    
     
    if(b < c)
    {
        temp = b;
        b = c;
        c = temp;
    }
}
```

### 2、一元多项式求值

```c
/**********
【题目】试编写算法求一元多项式
    P(x) = a0 + a1x + a2x^2 + ... + anx^n
的值P(x0)，并确定算法中每一语句的执行次数和整个算法
的时间复杂度。
**********/
float Polynomial(int n, int a[], float x)
/* 求一元多项式的值P(x)。                  */
/* 数组a的元素a[i]为i次项的系数，i=0,...,n */
{
    int i;
    float sum;
    if(n == 0){
        sum = a[0];
        return  sum;//处理只有a0的情况
    }   
    sum = a[n] * x + a[n-1];
    for(i = n-2;i >= 0 ;i --)
    {
        sum =  sum * x + a[i] ;//秦九韶算法
    }
    return sum;
}
```

### 3、k阶斐波那契序列

```c
/**********
【题目】已知k阶裴波那契序列的定义为
    f(0)=0, f(1)=0, ..., f(k-2)=0, f(k-1)=1;
    f(n)=f(n-1)+f(n-2)+...+f(n-k), n=k,k+1,...
试编写求k阶裴波那契序列的第m项值的函数算法，
k和m均以值调用的形式在函数参数表中出现。
**********/
Status Fibonacci(int k, int m, int &f) 
/* 求k阶斐波那契序列的第m项的值f，如果能求得，返回OK */
/*否则（如参数k与m不合理），返回ERROR*/
{
    //这里的序列定义跟斐波那契数列不太一样
    //K不可以为1，因为f(k-2)=0，至少大于等于2
    //k-1项总为1，第k项往后等于前面k项之和
    //如K = 3，则f(k)=f(2)+f(1)+f(0);
    if(k < 2 || m < 0){
        return ERROR;
    } 
    //前k-2项都为0
    int i,j,t[100] = {0,0},sum = 0;
    if(m < k-1) f = 0; 
    else if(m == k-1) f = 1;
    else {
        t[k-1] = 1;//k-1项为1
        for(i = k;i <= m;i ++){//给第k项~m项赋值
           for(j = i - k;j < i;j ++){
                sum += t[j];//前k项之和
           }
           t[i] = sum;//赋值给第k项~m项
           sum = 0;//记得归零
        }
        f = t[m];       
    }
    return OK;
}
```

### 4、计算i!*2^i的值

```c
/**********
【题目】试编写算法，计算i!×2^i的值并存入数组
a[0..n-1]的第i-1个分量中 (i=1,2,…,n)。假设计
算机中允许的整数最大值为MAXINT，则当对某个k
(1≤k≤n)使k!×2^k>MAXINT时，应按出错处理。注意
选择你认为较好的出错处理方法。
**********/
Status Series(int a[], int n) 
/* 求i!*2^i序列的值并依次存入长度为n的数组a；     */
/* 若所有值均不超过MAXINT，则返回OK，否则OVERFLOW */
{
     int i,j = 1;
     Status sum = 1; //记得设sum为1而不是0
     for(i = 0;i < n;i ++){
          for(j = i + 1;j > 0;j --){
                sum *= j * 2;
          }
          if(sum > MAXINT){
                return OVERFLOW;
          } else{
                a[i] = sum;
          }
          sum = 1;
     }
     return OK;
}
```

### 5、统计男女总分及团体总分

```c
/**********
【题目】假设有A、B、C、D、E五个高等院校进行田径对抗赛，
各院校的单项成绩均以存入计算机并构成一张表，表中每一行
的形式为：
        项目名称   性别   校名   成绩   得分
编写算法，处理上述表格，以统计各院校的男、女总分和团体
总分，并输出。
                               
相关数据类型定义如下：
typedef enum {female,male} Sex;
typedef struct{
  char *sport;     // 项目名称
  Sex  gender;     // 性别（女：female；男：male）
  char schoolname; // 校名为'A','B','C','D'或'E'
  char *result;    // 成绩
  int score;       // 得分（7,5,4,3,2或1）
} ResultType;

typedef struct{
  int malescore;   // 男子总分
  int femalescore; // 女子总分
  int totalscore;  // 男女团体总分
} ScoreType;

实现以下函数：
**********/
void Scores(ResultType *result, ScoreType *score)
/* 求各校的男、女总分和团体总分, 并依次存入数组score */
/* 假设比赛结果已经储存在result[ ]数组中,            */
/* 并以特殊记录 {"", male, ' ', "", 0 }（域scorce=0）*/
/* 表示结束                                          */
{
    int i = 0;
    for(i = 0;i < 5;i ++){
        score[i].malescore = 0;
        score[i].femalescore = 0;
        score[i].totalscore = 0; 
    }
    for(i = 0;;i ++){
        if(strcmp(result[i].sport,"") == 0
        && result[i].gender == male
        && result[i].schoolname == ' '
        && strcmp(result[i].result,"") == 0
        && result[i].score == 0){
            break;
        } 
        switch(result[i].schoolname){
            case 'A': {
                        if(result[i].gender == male){
                            score[0].malescore += result[i].score; 
                        }else{
                            score[0].femalescore += result[i].score;
                        } 
                        score[0].totalscore += result[i].score;
                        break; 
                      }
            case 'B': {
                        if(result[i].gender == male){
                            score[1].malescore += result[i].score;
                        }else{
                            score[1].femalescore += result[i].score;
                        } 
                        score[1].totalscore += result[i].score;
                        break;
                      }
            case 'C': {
                        if(result[i].gender == male){
                            score[2].malescore += result[i].score;
                        }else{
                            score[2].femalescore += result[i].score;
                        } 
                        score[2].totalscore += result[i].score;
                        break;
                      }
            case 'D': {
                        if(result[i].gender == male){
                            score[3].malescore += result[i].score;
                        }else{
                            score[3].femalescore += result[i].score;
                        } 
                        score[3].totalscore += result[i].score;
                        break;
                      }
            case 'E': {
                        if(result[i].gender == male){
                            score[4].malescore += result[i].score;
                        }else{
                            score[4].femalescore += result[i].score;
                        } 
                        score[4].totalscore += result[i].score;
                        break;
                      }
            default:break;
        }
    }
}
```

### 6、对序列S的第i个元素赋值e

```c
/**********
【题目】试写一算法，对序列S的第i个元素赋以值e。
序列的类型定义为：
typedef struct {
  ElemType  *elem;
  int  length;
} Sequence;
***********/
Status Assign(Sequence &S, int i, ElemType e) 
/* 对序列S的第i个元素赋以值e，并返回OK。 */
/* 若S或i不合法，则赋值失败，返回ERROR   */
{
   if(&S == null){
        return ERROR ;
   }
   if(S.elem == null){
        return ERROR;
   } 
   if(i < 0 || i > S.length){
        return ERROR;
   }
   S.elem[i] = e;
   return OK;   
}
```

### 7、由长度为n的一维数组a构建一个序列S

```c
/**********
【题目】试写一算法，由长度为n的一维数组a构建一个序列S。
序列的类型定义为：
typedef struct {
  ElemType  *elem;
  int  length;
} Sequence;
***********/
Status CreateSequence(Sequence &S, int n, ElemType *a) 
/* 由长度为n的一维数组a构建一个序列S，并返回OK。 */
/* 若构建失败，则返回ERROR                       */
{
    if(n <= 0||a == null){//这里不要忽略n等于0的情况
        return ERROR;
    }
    S.length = n;
    S.elem = a;
    return OK;
}
```

### 8、构建一个值为x的结点

```c
/**********
【题目】链表的结点和指针类型定义如下
    typedef struct LNode {
       ElemType  data;
       struct LNode *next;
    } LNode, *LinkList;
试写一函数，构建一个值为x的结点。
***********/
LinkList MakeNode(ElemType x)
/* 构建一个值为x的结点，并返回其指针。*/
/* 若构建失败，则返回NULL。           */
{    //代码一
     /*LinkList link ;
     link = (LinkList)malloc(sizeof(LNode));
     if(link == NULL) return NULL;
     link->data = x;
     return link;*/
     
     //代码二
     LNode  link;
     link.data = x;
     return &link; 
}
```

### 9、构建长度为2且两个结点的值依次为x和y的链表

```c
/**********
【题目】链表的结点和指针类型定义如下
    typedef struct LNode {
       ElemType  data;
       struct LNode *next;
    } LNode, *LinkList;
试写一函数，构建长度为2且两个结点的值依次为x和y的链表。
**********/
LinkList CreateLinkList(ElemType x, ElemType y) 
/* 构建其两个结点的值依次为x和y的链表。*/
/* 若构建失败，则返回NULL。            */
{
     LNode link;
     link.data = x;
     LinkList p;
     p = (LinkList)malloc(sizeof(LNode));
     if(p == NULL) return NULL;
     link.next = p;
     p->data = y;
     return &link;
}
```

### 10、构建长度为2的升序链表

```c
/**********
【题目】链表的结点和指针类型定义如下
    typedef struct LNode {
       ElemType  data;
       struct LNode *next;
    } LNode, *LinkList;
试写一函数，构建长度为2的升序链表，两个结点的值
分别为x和y，但应小的在前，大的在后。
**********/
LinkList CreateOrdLList(ElemType x, ElemType y)
/* 构建长度为2的升序链表。  */
/* 若构建失败，则返回NULL。 */
{
     if(x > y){
        //不开辟新内存交换两个数
        x = x+y;
        y = x-y;
        x = x-y;
     }
     LNode link;
     link.data = x;
     LinkList p;
     p = (LinkList)malloc(sizeof(LNode));
     if(p == NULL) return NULL;
     link.next = p;
     p->data = y;
     return &link;
}
```

## 第2章：线性数据结构

### 1、实现顺序栈的判空操作

```c
/**********
【题目】试写一算法，实现顺序栈的判空操作
StackEmpty_Sq(SqStack S)。
顺序栈的类型定义为：
typedef struct {
  ElemType *elem; // 存储空间的基址
  int top;        // 栈顶元素的下一个位置，简称栈顶位标
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack;        // 顺序栈
***********/
Status StackEmpty_Sq(SqStack S)
/* 对顺序栈S判空。                      */ 
/* 若S是空栈，则返回TRUE；否则返回FALSE */
{
    if(S.top != 0) return FALSE;
    else return TRUE;
}

```

### 2、实现顺序栈的取栈顶元素操作

```c
/**********
【题目】试写一算法，实现顺序栈的取栈顶元素操作
GetTop_Sq(SqStack S, ElemType &e)。
顺序栈的类型定义为：
typedef struct {
  ElemType *elem;  // 存储空间的基址
  int top;         // 栈顶元素的下一个位置，简称栈顶位标
  int size;        // 当前分配的存储容量
  int increment;   // 扩容时，增加的存储容量
} SqStack;         // 顺序栈
***********/
Status GetTop_Sq(SqStack S, ElemType &e) 
/* 取顺序栈S的栈顶元素到e，并返回OK； */ 
/* 若失败，则返回ERROR。              */
{
   if(S.top == 0) return ERROR;
   e = S.elem[S.top - 1];
   return OK;
}
```

### 3、实现顺序栈的出栈操作

```c
/**********
【题目】试写一算法，实现顺序栈的出栈操作
Pop_Sq(SqStack &S, ElemType &e)。
顺序栈的类型定义为：
typedef struct {
  ElemType *elem;  // 存储空间的基址
  int top;         // 栈顶元素的下一个位置，简称栈顶位标
  int size;        // 当前分配的存储容量
  int increment;   // 扩容时，增加的存储容量
} SqStack;         // 顺序栈
***********/
Status Pop_Sq(SqStack &S, ElemType &e) 
/* 顺序栈S的栈顶元素出栈到e，并返回OK；*/ 
/* 若失败，则返回ERROR。               */
{
    if(S.top == 0) return ERROR;
    //这里--S.top既可以找到栈顶元素
    //也可以实现栈顶位标减1
    e = S.elem[--S.top];
    return OK;
}
```

### 4、重新定义顺序栈，构建初始容量和扩容增量分别为size和inc的空顺序栈S

```c
/**********
【题目】若顺序栈的类型重新定义如下。试编写算法，
构建初始容量和扩容增量分别为size和inc的空顺序栈S。
typedef struct {
  ElemType *elem; // 存储空间的基址
  ElemType *top;  // 栈顶元素的下一个位置
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack2;
***********/
Status InitStack_Sq2(SqStack2 &S, int size, int inc)
/* 构建初始容量和扩容增量分别为size和inc的空顺序栈S。*/ 
/* 若成功，则返回OK；否则返回ERROR。                 */
{
    //考虑size、inc值不合理的情况
    if(size < 0 || inc < 0) return ERROR;
    S.elem = S.top = (ElemType *)malloc(sizeof(SqStack2)*size);
    if(S.elem == NULL) return ERROR;
    S.size = size;
    S.increment = inc;
    return OK;
}

```

### 5、重新定义顺序栈，实现顺序栈的判空操作

```c
/**********
【题目】若顺序栈的类型重新定义如下。试编写算法，
实现顺序栈的判空操作。
typedef struct {
  ElemType *elem; // 存储空间的基址
  ElemType *top;  // 栈顶元素的下一个位置
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack2;
***********/
Status StackEmpty_Sq2(SqStack2 S)
/* 对顺序栈S判空。                      */ 
/* 若S是空栈，则返回TRUE；否则返回FALSE */
{
     //同地址为空栈
     if(S.elem != S.top)return FALSE;
     else return TRUE;
}

```

### 6、重新定义顺序栈，实现顺序栈的入栈操作

```c
/**********
【题目】若顺序栈的类型重新定义如下。试编写算法，
实现顺序栈的入栈操作。
typedef struct {
  ElemType *elem; // 存储空间的基址
  ElemType *top;  // 栈顶元素的下一个位置
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack2;
***********/
Status Push_Sq2(SqStack2 &S, ElemType e)
/* 若顺序栈S是满的，则扩容，若失败则返回ERROR。*/
/* 将e压入S，返回OK。                          */
{
    int i = 0;
    ElemType *temp = S.elem;
    while(temp != S.top)/*找到两者地址相等，此时i表示有多少个元素，类似顺序栈1中top的作用*/ 
    {
        i++;
        temp ++;
    }
    temp = S.elem;
    if(i == S.size){
        /*内存重新分配，之所以用另一个变量来存储有以下原因：内存分配有几种情况(这个函数不太安全)*/
        /*
        	1、若已有内存后面还存在increment个连续地址，则把后面的increment内存纳入数组中，此时temp 			   的地址跟S.elem一致；
        	2、如果原来的内存后面没有足够的空闲空间用来分配，那么会在栈中重新找一块newSize大小的内存，
        	   然后把已有内存的数据复制到新内存中(数据已移动)，把新内存的地址返回给temp，而老块内存重新
        	   放回栈中
        	3、如果没有足够可用的内存用来完成重新分配（扩大原来的内存块或者分配新的内存块），则返回null.				而原来的内存块保持不变。
        	4、如果S.elem为null，则realloc()和malloc()类似。分配一个newsize的内存块，返回一个指向
        	   该内存块的指针。
		    5、如果newsize大小为0，那么释放S.elem指向的内存，并返回null。 
 
        	综上，这就是为什么重新分配内存时要定义一个临时指针，并最后把该指针指向NULL
        */
       temp = (ElemType *)realloc(S.elem,sizeof(ElemType)*(S.size
       +S.increment));
       if(temp == NULL)return ERROR;
       S.elem = temp;
       /*注意，之前这里如果没有下面这行代码，
        若如果原本已有六个元素(size)，
        则测试不通过，经调试发现：
        那个temp在内存重新分配后不一定是指向S.elem的值
        所以这时候S.top跟S.elem的地址差距甚大
        */
       S.top = S.elem + i;
       S.size += S.increment; 
       temp = NULL;  
    }
    *(S.top ++) = e;    
    return OK;
}
```

### 7、重新定义顺序栈，实现顺序栈的出栈操作

```c
/**********
【题目】若顺序栈的类型重新定义如下。试编写算法，
实现顺序栈的出栈操作。
typedef struct {
  ElemType *elem; // 存储空间的基址
  ElemType *top;  // 栈顶元素的下一个位置
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack2;
***********/
Status Pop_Sq2(SqStack2 &S, ElemType &e) 
/* 若顺序栈S是空的，则返回ERROR；    */ 
/* 否则将S的栈顶元素出栈到e，返回OK。*/
{
    if(S.elem == S.top) return ERROR;
    e = *(--S.top);
    return OK;
}
```

### 8、借助辅助栈，复制顺序栈S1得到S2

```c
/**********
【题目】试写一算法，借助辅助栈，复制顺序栈S1得到S2。
顺序栈的类型定义为：
typedef struct {
  ElemType *elem; // 存储空间的基址
  int top;        // 栈顶元素的下一个位置，简称栈顶位标
  int size;       // 当前分配的存储容量
  int increment;  // 扩容时，增加的存储容量
} SqStack;        // 顺序栈
可调用顺序栈接口中下列函数：
Status InitStack_Sq(SqStack &S, int size, int inc); // 初始化顺序栈S
Status DestroyStack_Sq(SqStack &S); // 销毁顺序栈S
Status StackEmpty_Sq(SqStack S);    // 栈S判空，若空则返回TRUE，否则FALSE
Status Push_Sq(SqStack &S, ElemType e); // 将元素e压入栈S
Status Pop_Sq(SqStack &S, ElemType &e); // 栈S的栈顶元素出栈到e
***********/
Status CopyStack_Sq(SqStack S1, SqStack &S2) 
/* 借助辅助栈，复制顺序栈S1得到S2。    */ 
/* 若复制成功，则返回TRUE；否则FALSE。 */
{
     //先初始化
     Status status = InitStack_Sq(S2,S1.size,S1.increment);
     if(status == OVERFLOW)return FALSE; 
     if(S1.top == 0) return TRUE;
     for(int i = 0;i < S1.top;i ++){
        Push_Sq(S2,S1.elem[i]);
     }
     DestroyStack_Sq(S1);
     return OK;
}
```

### 9、求循环队列的长度

```c
/**********
【题目】试写一算法，求循环队列的长度。
循环队列的类型定义为：
typedef struct {
  ElemType *base;  // 存储空间的基址
  int front;       // 队头位标
  int rear;        // 队尾位标，指示队尾元素的下一位置
  int maxSize;     // 最大长度
} SqQueue;
***********/
int QueueLength_Sq(SqQueue Q)
/* 返回队列Q中元素个数，即队列的长度。 */ 
{
    //保留一个内存空间不用的方案
    //有两种情况：rear在front后；rear在front前
    return (Q.rear-Q.front+Q.maxSize)%Q.maxSize;
}
```

### 10、设置一个标志域tag标记队列的空或满，编写与此结构相应的入队列和出队列的算法

```c
/**********
【题目】如果希望循环队列中的元素都能得到利用，
则可设置一个标志域tag，并以tag值为0或1来区分尾
指针和头指针值相同时的队列状态是"空"还是"满"。
试编写与此结构相应的入队列和出队列的算法。
本题的循环队列CTagQueue的类型定义如下：
typedef struct {
  ElemType elem[MAXQSIZE];
  int tag;
  int front;
  int rear;
} CTagQueue;
**********/
/*方案一：利用出队时顺便把出队的那个存储空间清0，然后在入队函数那里判断是否队列满；该方法有局限性，如遇到那种不能清零的存储空间比如非基本类型就不能用了，还是方案二简单易懂*/
    
Status EnCQueue(CTagQueue &Q, ElemType x)
/* 将元素x加入队列Q，并返回OK；*/
/* 若失败，则返回ERROR。       */
{
    if(Q.rear == Q.front && Q.tag == 1){//队列满
        return ERROR;
    }
    Q.elem[Q.rear] = x;
    Q.rear = (Q.rear + 1)%MAXQSIZE;  
    if(Q.elem[Q.rear] && Q.rear == Q.front)
    {//rear等于front且该值不为0（因为在出队时设置出队的元素为0）
     //如果该值不为0说明该地方有数据，即队列满
        Q.tag = 1;
    }else{      
        Q.tag = 0;
    }
    //入队后，如果rear等于front，那么队列满了
    if(Q.rear == Q.front) Q.tag = 1;
    return OK;
}

Status DeCQueue(CTagQueue &Q, ElemType &x)
/* 将队列Q的队头元素退队到x，并返回OK；*/
/* 若失败，则返回ERROR。               */
{
    if(Q.rear == Q.front && Q.tag == 0){//队列空
        return ERROR;
    }
    x = Q.elem[Q.front];    
    Q.elem[Q.front] = 0;//值清零，为了入队时判断是否满
    /*if(!Q.elem[(Q.front+1)%MAXQSIZE]){//队列的Q.elem[Q.rear]为0？
        Q.tag = 0;        
    }else Q.tag = 1; */
    Q.front = (Q.front+1)%MAXQSIZE;
    return OK;
}

/*方案二：入队后,如果rear == front,那么说明队列满；出队后,如果rear == front,那么该队列为空*/

Status EnCQueue(CTagQueue &Q, ElemType x)
/* 将元素x加入队列Q，并返回OK；*/
/* 若失败，则返回ERROR。       */
{
    if(Q.rear == Q.front && Q.tag == 1){//队列满
        return ERROR;
    }
    Q.elem[Q.rear] = x;
    Q.rear = (Q.rear + 1)%MAXQSIZE;  
    //入队后,如果rear == front,那么说明队列满
    if(Q.rear == Q.front) Q.tag = 1;
    return OK;
}

Status DeCQueue(CTagQueue &Q, ElemType &x)
/* 将队列Q的队头元素退队到x，并返回OK；*/
/* 若失败，则返回ERROR。               */
{
    if(Q.rear == Q.front && Q.tag == 0){//队列空
        return ERROR;
    }
    x = Q.elem[Q.front];        
    Q.front = (Q.front+1)%MAXQSIZE; 
    //出队后,如果rear == front,那么该队列为空
    if(Q.rear == Q.front) Q.tag = 0;
    return OK;
}
    
```

### 11、设一个长度域length记录个数，实现相应的入队列和出队列的算法，

```c
/**********
【题目】假设将循环队列定义为：以域变量rear
和length分别指示循环队列中队尾元素的位置和内
含元素的个数。试给出此循环队列的队满条件，并
写出相应的入队列和出队列的算法（在出队列的算
法中要返回队头元素）。
本题的循环队列CLenQueue的类型定义如下：
typedef struct {
  ElemType elem[MAXQSIZE];
  int length;
  int rear;
} CLenQueue;
**********/
Status EnCQueue(CLenQueue &Q, ElemType x)
  /* 将元素x加入队列Q，并返回OK；*/
  /* 若失败，则返回ERROR。       */
{
    if(Q.length == MAXQSIZE)return ERROR;//队列满
    Q.rear = (Q.rear+1)%MAXQSIZE;//改变rear
    /*注意，这里rear是指队尾元素，不是队尾位标*/
    Q.elem[Q.rear] = x;
    Q.length ++;//记得自增
    return OK;
}
Status DeCQueue(CLenQueue &Q, ElemType &x)
  /* 将队列Q的队头元素退队到x，并返回OK；*/
  /* 若失败，则返回ERROR。               */
{
    if(Q.length == 0) return ERROR;//队列空
    /*队头元素考虑队尾在队头后面的情况哦
      Q.rear + 1是为了得到队尾位标*/
    x = Q.elem[(Q.rear + 1 - Q.length + MAXQSIZE) % MAXQSIZE];
    Q.length --;//记得自减
    return OK;
}
```

