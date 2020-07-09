# 线性表

## 1. 什么是线性表

**线性表（Linear List）**：由同类型 **数据元素** 构成 **有序序列** 的线性结构

* 表中元素个数称为线性表的 **长度**
* 线性表没有元素时，称为 **空表**
* 表起始位置称为 **表头**，表结束位置称为 **表尾**

## 2. 线性表的抽象数据类型描述

**类型名称：**线性表（ $$List$$ ）

**数据对象集：**线性表是 $$n( \ge 0)$$ 个元素构成的有序序列 $$(a_1, a_2, \cdots , a_n)$$ 

**操作集：**线性表 $$L \in List$$ ，整数 $$i$$ 表示位置，元素 $$X \in ElementType$$ ，线性表基本操作主要有：

* `List MakeEmpty()`：初始化一个空线性表 ****$$L$$ ；
* `ElementType FindKth(int K, List L)`：根据位序$$K$$ ，返回相应元素；
* `int Find(ElementType X, List L)`：在线性表$$L$$ 中查找 $$X$$ 第一次出现的位置；
* `void Insert(ElementType X, int i, List L)`：在位序$$i$$ 前插入一个新元素 $$X$$ ；
* `void Delete(int i, List L)`：删除指定位序 ****$$i$$ 的元素；
* `int Length(List L)`：返回线性表 $$L$$ 的长度 $$n$$ 。

