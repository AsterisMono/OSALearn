## 第一部分：实验预习报告
### 【实验目的】
1. 掌握模板函数和模板类的定义和使用方法。
2. 掌握标准输入输出的使用及格式控制方法。
3. 掌握磁盘文件（如二进制文件、文本文件）的输入输出的方法。
## 第二部分：实验过程记录
1. 实现栈

将栈类改造成**类模板**：
```c++
template<class T>
class IStack
{
public:
	IStack() { Top = 0; } // 栈的构造函数
	void Push(T& n); // 往栈顶增加元素
	void Push(const T& n);
	void Pop() { if (!Empty()) Top--; } // 从非空栈的栈顶删除一个元素
	T GetTop() {
		if(!Empty()) return elem[Top-1];
	}// 返回非空栈的栈顶元素
	bool Empty() {
		if (Top == 0) return true;
		else return false;
	}// 判断栈是否为空
	int Size() {
		return Top;
	}// 返回栈中元素的个数
	void ClearStack() {
		Top = 0;
	} // 将栈清空
	~IStack() {
		//Do nothing here
	}// 栈的析构函数
private:
	T elem[MaxSize]; // 保存栈中各元素的数组
	int Top; // 保存栈顶的当前位置
};
template<class T>
void IStack<T>::Push(T& n) {
	elem[Top] = n;
	Top++;
}
template<class T>
void IStack<T>::Push(const T& n) {
	elem[Top] = n;
	Top++;
}
```
在设计时，首先改动返回值类型，然后实现相关成员函数。

*在第一次测试时发现，push函数无法接收常量，于是对其进行重载，形参改为const T&。*

2. 重载运算符，输出电话号码s

首先定义一个电话号码类：
```c++
class PhoneNum {
private:
	int AreaCode;
	int Base;
}
```

然后实现输入、输出运算符重载：
```c++
friend istream& operator>>(istream& in, PhoneNum &phm) {
		char t;
		in.get(t);
		if (t != '(') { cout << "error!" << endl; return in; }
		in >> phm.AreaCode;
		in.get(t);
		if (t != ')') { cout << "error!" << endl; return in; }
		in >> phm.Base;
		return in;
	}
	friend ostream& operator<<(ostream& out, PhoneNum &phm) {
		out << '(' <<'0'<< phm.AreaCode <<')'<< phm.Base << endl;
		return out;
	}
```