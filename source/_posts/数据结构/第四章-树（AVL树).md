---
title: 第四章 树（AVL树)
tags:
 - cpp
categories: 数据结构
---

# AVL树节点声明

```cpp
static const int ALLOWED_IMBALANCE = 1;//AVL树允许的最大高度差
struct AvlNode
{
	int element;
	AvlNode* left;//左子节点
	AvlNode* right;//右子节点
	int height;//高度

	AvlNode(const int& e, AvlNode* lt, AvlNode* rt, int h = 0)
		:element(e),left(lt),right(rt),height(h){}
	AvlNode(int && e,AvlNode*lt,AvlNode*rt,int h=0)
		:element(move(e)),left(lt),right(rt),height(h){}
};
```

# 函数

## height

```cpp
int height(AvlNode* t)//返回节点的高度，如果节点没有子节点则返回-1
{
	return t == nullptr ? -1 : t->height;
}
```

## rotatedWithLeftChild

```cpp
void rotatedWithLeftChild(AvlNode*& k2)//左左(k2的左子节点向右上方单旋转)
{
	AvlNode* k3 = k2->left;
	k2->left = k3->right;
	k3->right = k2;
	k2->height = max(height(k2->left), height(k2->right)) + 1;//更新k2高度
	k3->height = max(height(k3->left), height(k3->right)) + 1;//更新k1高度
	k2 = k3;//把k2原先所在位置的节点更新为k1
}
```

## rotatedWithRightChild

```cpp
void rotatedWithRightChild(AvlNode*& k2)//右右（k2的右子节点向左上方单旋转）
{
	AvlNode* k1 = k2->right;
	k2->right = k1->left;
	k1->left = k2;
	k2->height = max(height(k2->left), height(k2->right)) + 1;//更新k2高度
	k1->height = max(height(k1->left), height(k1->right)) + 1;//更新k1高度
	k2 = k1;//把k2原先所在位置的节点更新为k1
}
```

## doubleWithLeftChild

```cpp
void doubleWithLeftChild(AvlNode*& k3)
{
	rotatedWithRightChild(k3->left);
	rotatedWithLeftChild(k3);
}
```

## doubleWithRightChild

```cpp
void doubleWithRightChild(AvlNode*& k3)
{
	rotatedWithLeftChild(k3->left);
	rotatedWithRightChild(k3);
}
```

## balance

```cpp
void balance(AvlNode*& t)//使节点平衡（即左右子树的高度差的绝对值小于等于1）
{
	if (t == nullptr)
	{
		return;
	}
	if (height(t->left) - height(t->right) > ALLOWED_IMBALANCE)
	{
		if (height(t->left->left) >= height(t->left->right))
		{
			rotatedWithLeftChild(t);
		}
		else
		{
			doubleWithLeftChild(t);
		}
	}
	else if (height(t->right) - height(t->left) > ALLOWED_IMBALANCE)
	{
		if (height(t->right->right) >= height(t->right->left))
		{
			rotatedWithRightChild(t);
		}
		else
			doubleWithRightChild(t);
	}
	t->height = max(height(t->left), height(t->right)) + 1;
}
```

## insert

```cpp
void insert(const int& x, AvlNode*& t)
{
	if (t == nullptr)
		t = new AvlNode(x, nullptr, nullptr);
	else if (x < t->element)//插入到左子树
		insert(x, t->left);
	else if (x > t->element)//插入到右子树
		insert(x, t->right);
	balance(t);//插入后检查是否平衡，若不平衡则调整
}
```

## remove

```cpp
void remove(const int& x, AvlNode*& t)
{
	if (t == nullptr)return;
	if (x < t->element)
		remove(x, t->left);
	else if (x > t->element)
		remove(x, t->right);
	else if (t->left != nullptr && t->right != nullptr)//要删除元素所在的节点有两个子节点
	{
		t->element = findMin(t->right)->element;//把x所在节点t的元素换成该节点右子树中最小的元素
		remove(t->element, t->right);//把t的右子树中最小的元素删除
	}
	else//要删除的元素所在的节点只有一个子节点或是没有子节点
	{
		AvlNode* oldNode = t;
		t = (t->left != nullptr) ? t->left : t->right;
		delete oldNode;
	}
	balance(t);
}
```

# 树的遍历

以排列顺序打印根在t处的子树的内部方法(**中序遍历**)

```cpp
void printTree(BinaryNode* t,ostream& out) const//递归
{
    if(t!=nullptr)
    {
        printTree(t->left,out);
        out<<t->element<<endl;
        printTree(t->right,out);
    }
}
```

按排列顺序打印树的内容

```cpp
void printTree(ostream& out=cout)const
{
    if(isEmpty())
        out<<"Empty tree"<<endl;
    else 
        printTree(root,out);
}
```

