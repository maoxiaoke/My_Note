# 栈 Stack #
## 栈模型 ##

![栈模型](https://www.tutorialspoint.com/data_structures_algorithms/images/stack_representation.jpg)

### 特点 ###

- 只能在**顶部(top)**处理`push`(进栈)和`pop`(出栈)操作。
- 有时候又被称为**LIFO(先进后出)**表。

## 栈的实现 ##

有两种较为流行的方法：

- [**指针实现**](#list)
- [**数组实现**](#array)

### <a name="list">栈的链表实现</a> ###

> 第一种实现方法是单链表。在表顶部插入来实现`push`,通过删除表顶端元素实现`pop`,`top`操作只是考查表顶端元素并返回它的值。

#### 定义 ####

	#ifndef _Stack_h
	struct Node;
	typedef struct Node *PtrToNode;
	typedef PtrToNode Stack;
	
	int IsEmpty(Stack S);
	Stack CreateStack(void);
	void DisposeStack(Stack S);
	void  MakeEmpty(Stack S);
	void Push(ElementType X, Stack S);
	ElementType Top(Stack S);
	void Pop(Stack S); 
	
	#endif 

在`.c`文件中定义结构体。

	struct Node
	{
		ElementType Element;
		PtrToNode 	Next;
	};

#### 测试栈是否为空 ####

![Test the Stack ADT is empty](https://raw.githubusercontent.com/maoxiaoke/My_Note/master/Algorithm/Screenshots/Stack-IsEmpty.png)

	int IsEmpty(Stack S)
	{
		return S->Next == NULL;
	}

#### 创建一个空栈 ####

建立一个头结点，`Next`指针指向`NULL`；

	Stack CreateStack( void )
    {
		Stack S;
        S = malloc( sizeof( struct Node ) );
        if( S == NULL )
			FatalError( "Out of space!!!" );
        S->Next = NULL;
        MakeEmpty( S );
        return S;
    }
    void MakeEmpty( Stack S )
    {
		if( S == NULL )
			Error( "Must use CreateStack first" );
        else
            while( !IsEmpty( S ) )
				Pop( S );
    }

#### Push 进栈 ####

`push`向链表的前端进行插入，表的前端作为栈顶。

![Push Element to Stack ADT](https://raw.githubusercontent.com/maoxiaoke/My_Note/master/Algorithm/Screenshots/Stack-push.png)

	void Push( ElementType X, Stack S )
	{
		PtrToNode TmpCell;
		TmpCell = malloc( sizeof( struct Node ) );
        if( TmpCell == NULL )
			FatalError( "Out of space!!!" );
        else
        {
            TmpCell->Element = X;
            TmpCell->Next = S->Next;
            S->Next = TmpCell;
        }
    }

#### Pop 出栈 ####

`pop`删除表的前端的元素。

![Pop Element from Stack ADT](https://raw.githubusercontent.com/maoxiaoke/My_Note/master/Algorithm/Screenshots/Stack-pop.png)

	void Pop( Stack S )
    {
		PtrToNode FirstCell;
		if( IsEmpty( S ) )
			Error( "Empty stack" );
        else
        {
            FirstCell = S->Next;
            S->Next = S->Next->Next;
            free( FirstCell );
        }
    }

#### 返回栈顶指针 ####

`top`的实施考查表的第一个位置上的元素。

	ElementType Top( Stack S )
    {
		if( !IsEmpty( S ) )
			return S->Next->Element;
        Error( "Empty stack" );
        return 0;  /* Return value used to avoid warning */
    }

#### 总结 ####

> 这些操作所有的时间花费都是常数时间。这种方法实现的缺点在于对`malloc`和`free`的调用的开销是昂贵的。


### <a name="array">栈的数组实现</a> ###
	
另一种实现方法避免了指针，并且可能是更流行的方法。这种策略的唯一潜在危害是我们需要提前声明一个数组的大小。

#### 定义 ####

	#ifndef _Stack_h
    #define _Stack_h

    struct StackRecord;
    typedef struct StackRecord *Stack;

    int IsEmpty( Stack S );
    int IsFull( Stack S );
    Stack CreateStack( int MaxElements );
    void DisposeStack( Stack S );
    void MakeEmpty( Stack S );
    void Push( ElementType X, Stack S );
    ElementType Top( Stack S );
    void Pop( Stack S );
    ElementType TopAndPop( Stack S );

    #endif  /* _Stack_h */

在`.c`文件中定义结构体。

	#define EmptyTOS ( -1 )
    #define MinStackSize ( 5 )
	struct StackRecord
    {
		int Capacity;
        int TopOfStack;
        ElementType *Array;
	};

>栈被定义成为指向一个结构体的指针，并给定的最大值(`MinStackSize`)。每个栈都有一个`TopOfStack`，对于空栈，它的值为`-1`。 

#### 栈的创建 ####

	Stack CreateStack( int MaxElements )
    {
		Stack S;
		if( MaxElements < MinStackSize )
        	Error( "Stack size is too small" );
        S = malloc( sizeof( struct StackRecord ) );
      	if( S == NULL )
          	FatalError( "Out of space!!!" );
     	S->Array = malloc( sizeof( ElementType ) * MaxElements );
     	if( S->Array == NULL )
        	FatalError( "Out of space!!!" );
		S->Capacity = MaxElements;
     	MakeEmpty( S );
     	return S;
   	}

#### 释放栈 ####

>这个例程先释放栈数组，然后再释放栈结构体。

	void DisposeStack( Stack S )
	{
		if( S != NULL )
        {
        	free( S->Array );
            free( S );
        }
	}

#### 创建一个空栈 ####

	void MakeEmpty( Stack S )
    {
		S->TopOfStack = EmptyTOS;
    }

#### 检测一个栈是否是空栈 ####

	int IsEmpty( Stack S )
    {
		return S->TopOfStack == EmptyTOS;
    }

#### Push进栈 ####

	void Push( ElementType X, Stack S )
    {
		if( IsFull( S ) )
        	Error( "Full stack" );
        else
        	S->Array[ ++S->TopOfStack ] = X;
	}

### 将栈顶返回 ###

	ElementType Top( Stack S )
    {
		if( !IsEmpty( S ) )
        	return S->Array[ S->TopOfStack ];
        Error( "Empty stack" );
        return 0;  /* Return value used to avoid warning */
	}

#### Pop出栈 ####

	void Pop( Stack S )
	{
		if( IsEmpty( S ) )
			Error( "Empty stack" );
        else
            S->TopOfStack--;
    }

#### Pop返回弹出的元素 ####

	ElementType TopAndPop( Stack S )
	{
		if( !IsEmpty( S ) )
			return S->Array[ S->TopOfStack-- ];
		Error( "Empty stack" );
        return 0;  /* Return value used to avoid warning */
	}