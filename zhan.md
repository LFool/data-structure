# 栈

## 1. 栈的定义

**栈：**具有一定操作约束的线性表

* 只在一端（栈顶，Top）做 **插入**、**删除**
* 插入数据：**入栈（Push）**
* 删除数据：**出栈（Pop）**
* **先入后出**

## 2. 栈的抽象数据类型描述

**类型名称：**栈（ $$Stack$$ ）

**数据对象集：**一个有 0 个或多个元素的有穷线性表

**操作集：**长度为 $$MaxSize$$ 的栈 $$S \in Stack$$ ，栈元素 $$item \in ElementType$$ 

* `Stack CreateStack(int MaxSize)`：生成空栈，其最大长度为 ****$$MaxSize$$ ；
* `int IsFull(Stack S, int MaxSize)`：判断堆栈 $$S$$ 是否已满；
* `void Push(Stack S, ElementType item)`：将元素 $$item$$ 压入栈；
* `int IsEmpty(Stack S)`：判断栈 ****$$S$$ 是否为空；
* `ElementType Pop(Stack S)`：删除并返回栈顶元素。

