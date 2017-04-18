# 队列Queue #
## 队列模型 ##

![model of deque](https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Data_Queue.svg/300px-Data_Queue.svg.png)

### 特点 ###

- 在表的末端(队尾rear)插入一个元素的**Enqueue**(入队)操作，删除(或返回)在表头(对头front)的元素的**Dequeue**操作。

## 队列的实现 ##

### 队列的数组实现 ###

#### 定义 ####

        typedef int ElementType;

        #ifndef _Queue_h
        #define _Queue_h

        struct QueueRecord;
        typedef struct QueueRecord *Queue;

        int IsEmpty( Queue Q );
        int IsFull( Queue Q );
        Queue CreateQueue( int MaxElements );
        void DisposeQueue( Queue Q );
        void MakeEmpty( Queue Q );
        void Enqueue( ElementType X, Queue Q );
        ElementType Front( Queue Q );
        void Dequeue( Queue Q );
        ElementType FrontAndDequeue( Queue Q );

        #endif  /* _Queue_h */


在`.c`文件中定义结构体。

        #define MinQueueSize ( 5 )

        struct QueueRecord
        {
            int Capacity;
            int Front;
            int Rear;
            int Size;
            ElementType *Array;
        };

> `Front`和`Rear`分别表示队列的两端。`Size`记录了实际存在于队列的元素个数。这些作为一个结构的部分，除了队列例程本身通常不会有其他例程访问。

#### 使用循环数组circular array ####

暂略。

#### 测试队列是否为空 ####

        int IsEmpty( Queue Q )
        {
            return Q->Size == 0;
        }

#### 构造空队列 ####

        void MakeEmpty( Queue Q )
        {
            Q->Size = 0;
            Q->Front = 1;
            Q->Rear = 0;
        }

#### 入队 ####

        static int Succ( int Value, Queue Q )
        {
            if( ++Value == Q->Capacity )
                Value = 0;
            return Value;
        }

        void Enqueue( ElementType X, Queue Q )
        {
            if( IsFull( Q ) )
                Error( "Full queue" );
            else
            {
                Q->Size++;
                Q->Rear = Succ( Q->Rear, Q );
                Q->Array[ Q->Rear ] = X;
            }
        }