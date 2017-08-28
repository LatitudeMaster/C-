# STL ——容器，算法，迭代器
* STL 具有高可重用性，高性能，高移植性，跨平台的优点。
## Vector
*  **构造**
```cpp
vector<T> v; //采用模板实现类实现，默认构造函数
vector(v.begin(), v.end());//将v[begin(), end())区间中的元素拷贝给本身。
vector(n, elem);//构造函数将n个elem拷贝给本身。
vector(const vector &vec);//拷贝构造函数。

//例子 使用第二个构造函数 我们可以...
int arr[] = {2,3,4,1,9};
vector<int> v1(arr, arr + sizeof(arr) / sizeof(int));

```
*  **赋值**
```cpp
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
vector& operator=(const vector  &vec);//重载等号操作符
swap(vec);// 将vec与本身的元素互换。
```
*  **大小**
```cpp
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(int num);//重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
resize(int num, elem);//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长>度的元素被删除。
capacity();//容器的容量
reserve(int len);//容器预留len个元素长度，预留位置不初始化，元素不可访问。
```
*  **存取**
```cpp
at(int idx); //返回索引idx所指的数据，如果idx越界，抛出out_of_range异常。
operator[];//返回索引idx所指的数据，越界时，运行直接报错
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素
```
*  **增删**
```cpp
insert(const_iterator pos, int count,ele);//迭代器指向位置pos插入count个元素ele.
push_back(ele); //尾部插入元素ele
pop_back();//删除最后一个元素
erase(const_iterator start, const_iterator end);//删除迭代器从start到end之间的元素
erase(const_iterator pos);//删除迭代器指向的元素
clear();//删除容器中所有元素
```
* Vector案例
### （1） 巧用swap收缩内存空间 ###
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
using namespace std;

int main(){

	vector<int> v;
	for (int i = 0; i < 100000;i ++){
		v.push_back(i);
	}

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;

	//此时 通过resize改变容器大小
	v.resize(10);

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;

	//容量没有改变
	vector<int>(v).swap(v);

	cout << "capacity:" << v.capacity() << endl;
	cout << "size:" << v.size() << endl;


	system("pause");
	return EXIT_SUCCESS;
}

```
### （2） reserve预留空间 ###
```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
using namespace std;

int main(){

	vector<int> v;

	//预先开辟空间
	v.reserve(100000);

	int* pStart = NULL;
	int count = 0;
	for (int i = 0; i < 100000;i ++){
		v.push_back(i);
		if (pStart != &v[0]){
			pStart = &v[0];
			count++;
		}
	}

	cout << "count:" << count << endl;

	system("pause");
	return EXIT_SUCCESS;
}
```
## String
*  **构造**
```cpp
string();//创建一个空的字符串 例如: string str;      
string(const string& str);//使用一个string对象初始化另一个string对象
string(const char* s);//使用字符串s初始化
string(int n, char c);//使用n个字符c初始化
```
*  **赋值**
```cpp
string& operator=(const char* s);//char*类型字符串 赋值给当前的字符串
string& operator=(const string &s);//把字符串s赋给当前的字符串
string& operator=(char c);//字符赋值给当前的字符串
string& assign(const char *s);//把字符串s赋给当前的字符串
string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);//把字符串s赋给当前字符串
string& assign(int n, char c);//用n个字符c赋给当前字符串
string& assign(const string &s, int start, int n);//将s从start开始n个字符赋值给字符串
```
*  **存取**
```cpp
char& operator[](int n);//通过[]方式取字符
char& at(int n);//通过at方法获取字符
```
*  **拼接**
```cpp
string& operator+=(const string& str);//重载+=操作符
string& operator+=(const char* str);//重载+=操作符
string& operator+=(const char c);//重载+=操作符
string& append(const char *s);//把字符串s连接到当前字符串结尾
string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾
string& append(const string &s);//同operator+=()
string& append(const string &s, int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾
string& append(int n, char c);//在当前字符串结尾添加n个字符c
```
*  **查找和替换**
```cpp
int find(const string& str, int pos = 0) const; //查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = 0) const;  //查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;  //从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;  //查找字符c第一次出现位置
int rfind(const string& str, int pos = npos) const;//查找str最后一次位置,从pos开始查找
int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找
int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const; //查找字符c最后一次出现位置
string& replace(int pos, int n, const string& str); //替换从pos开始n个字符为字符串str
string& replace(int pos, int n, const char* s); //替换从pos开始的n个字符为字符串s
```
*  **比较**
```cpp
/*
compare函数在>时返回 1，<时返回 -1，==时返回 0。
比较区分大小写，比较时参考字典顺序，排越前面的越小。
大写的A比小写的a小。
*/
int compare(const string &s) const;//与字符串s比较
int compare(const char *s) const;//与字符串s比较
```
*  **string子串**
```cpp
string substr(int pos = 0, int n = npos) const;//返回由pos开始的n个字符组成的字符串
```
*  **插入和删除**
```cpp
string& insert(int pos, const char* s); //插入字符串
string& insert(int pos, const string& str); //插入字符串
string& insert(int pos, int n, char c);//在指定位置插入n个字符c
string& erase(int pos, int n = npos);//删除从Pos开始的n个字符
```
*  **string和c-style字符串转换**
```cpp
//string 转 char*
string str = "itcast";
const char* cstr = str.c_str();
//char* 转 string
char* s = "itcast";
string sstr(s);
```
## Stack
*  **构造**
```cpp
deque<T> deqT;//默认构造形式
deque(beg, end);//构造函数将[beg, end)区间中的元素拷贝给本身。
deque(n, elem);//构造函数将n个elem拷贝给本身。
deque(const deque &deq);//拷贝构造函数。
```
*  **赋值**
```cpp
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
deque& operator=(const deque &deq); //重载等号操作符
swap(deq);// 将deq与本身的元素互换
```
*  **大小**
```cpp
deque.size();//返回容器中元素的个数
deque.empty();//判断容器是否为空
deque.resize(num);//重新指定容器的长度为num,若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
deque.resize(num, elem); //重新指定容器的长度为num,若容器变长，则以elem值填充新位置,如果容器变短，则末尾超出容器长度的元素被删除。
```
*  **增删**
```cpp
push_back(elem);//在容器尾部添加一个数据
push_front(elem);//在容器头部插入一个数据
pop_back();//删除容器最后一个数据
pop_front();//删除容器第一个数据
```
*  **存取**
```cpp
at(idx);//返回索引idx所指的数据，如果idx越界，抛出out_of_range。
operator[];//返回索引idx所指的数据，如果idx越界，不抛出异常，直接出错。
front();//返回第一个数据。
back();//返回最后一个数据
```
*  **插入**
```cpp
insert(pos,elem);//在pos位置插入一个elem元素的拷贝，返回新数据的位置。
insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
```
*  **删除**
```cpp
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
erase(pos);//删除pos位置的数据，返回下一个数据的位置。
```
## Dequeue
*  **构造**
```cpp
stack<T> stkT;//stack采用模板类实现， stack对象的默认构造形式：
stack(const stack &stk);//拷贝构造函数
```
*  **赋值**
```cpp
stack& operator=(const stack &stk);//重载等号操作符
```
*  **存取**
```cpp
push(elem);//向栈顶添加元素
pop();//从栈顶移除第一个元素
top();//返回栈顶元素
```
*  **大小**
```cpp
empty();//判断堆栈是否为空
size();//返回堆栈的大小
```
## Queue
*  **构造函数**
```cpp
queue<T> queT;//queue采用模板类实现，queue对象的默认构造形式：
queue(const queue &que);//拷贝构造函数
```
*  **存取、插入和删除操作**
```cpp
push(elem);//往队尾添加元素
pop();//从队头移除第一个元素
back();//返回最后一个元素
front();//返回第一个元素
```
*  **赋值**
```cpp
queue& operator=(const queue &que);//重载等号操作符
```
*  **大小**
```cpp
empty();//判断队列是否为空
size();//返回队列的大小
```
## List
*  **构造**
```cpp
list<T> lstT;//list采用采用模板类实现,对象的默认构造形式：
list(beg,end);//构造函数将[beg, end)区间中的元素拷贝给本身。
list(n,elem);//构造函数将n个elem拷贝给本身。
list(const list &lst);//拷贝构造函数。
```
*  **插删**
```cpp
push_back(elem);//在容器尾部加入一个元素
pop_back();//删除容器中最后一个元素
push_front(elem);//在容器开头插入一个元素
pop_front();//从容器开头移除第一个元素
insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
erase(pos);//删除pos位置的数据，返回下一个数据的位置。
remove(elem);//删除容器中所有与elem值匹配的元素。
```
*  **大小**
```cpp
size();//返回容器中元素的个数
empty();//判断容器是否为空
resize(num);//重新指定容器的长度为num，
若容器变长，则以默认值填充新位置。
如果容器变短，则末尾超出容器长度的元素被删除。
resize(num, elem);//重新指定容器的长度为num，
若容器变长，则以elem值填充新位置。
如果容器变短，则末尾超出容器长度的元素被删除。
```
*  **赋值**
```cpp
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
list& operator=(const list &lst);//重载等号操作符
swap(lst);//将lst与本身的元素互换。
```
*  **存取**
```cpp
front();//返回第一个元素。
back();//返回最后一个元素。
```
*  **反转排序**
```cpp
reverse();//反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素。
sort(); //list排序
```
## Set / multiset
*  **构造**
```cpp
mulitset<T> mst; //multiset默认构造函数:
set(const set &st);//拷贝构造函数
```
*  **赋值**
```cpp
set& operator=(const set &st);//重载等号操作符
swap(st);//交换两个集合容器
```
*  **大小**
```cpp
size();//返回容器中元素的数目
empty();//判断容器是否为空
```
*  **插入和删除**
```cpp
insert(elem);//在容器中插入元素。
clear();//清除所有元素
erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
erase(beg, end);//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
erase(elem);//删除容器中值为elem的元素。
```
*  **查找**
```cpp
find(key);//查找键key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
count(key);//查找键key的元素个数
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```
## pair
*  **创建对组**
```cpp
//第一种方法创建一个对组
pair<string, int> pair1(string("name"), 20);
cout << pair1.first << endl; //访问pair第一个值
cout << pair1.second << endl;//访问pair第二个值
//第二种
pair<string, int> pair2 = make_pair("name", 30);
cout << pair2.first << endl;
cout << pair2.second << endl;
//pair=赋值
pair<string, int> pair3 = pair2;
cout << pair3.first << endl;
cout << pair3.second << endl;
```
## Map / multimap
*  **构造函数**
```cpp
map<T1, T2> mapTT;//map默认构造函数:
map(const map &mp);//拷贝构造函数
```
*  **赋值**
```cpp
map& operator=(const map &mp);//重载等号操作符
swap(mp);//交换两个集合容器
```
*  **大小**
```cpp
size();//返回容器中元素的数目
empty();//判断容器是否为空
```
*  **插入**
```cpp
map.insert(...); //往容器插入元素，返回pair<iterator,bool>
map<int, string> mapStu;
// 第一种 通过pair的方式插入对象
mapStu.insert(pair<int, string>(3, "小张"));
// 第二种 通过pair的方式插入对象
mapStu.inset(make_pair(-1, "校长"));
// 第三种 通过value_type的方式插入对象
mapStu.insert(map<int, string>::value_type(1, "小李"));
// 第四种 通过数组的方式插入值
mapStu[3] = "小刘";
mapStu[5] = "小王";
```
*  **删除**
```cpp
clear();//删除所有元素
erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
erase(beg,end);//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
erase(keyElem);//删除容器中key为keyElem的对组。
```
*  **p查找**
```cpp
find(key);//查找键key是否存在,若存在，返回该键的元素的迭代器；/若不存在，返回map.end();
count(keyElem);//返回容器中key为keyElem的对组个数。对map来说，要么是0，要么是1。对multimap来说，值可能大于1。
lower_bound(keyElem);//返回第一个key<=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```
* multimap 案例
```cpp
//公司今天招聘了5个员工，5名员工进入公司之后，需要指派员工在那个部门工作
//人员信息有: 姓名 年龄 电话 工资等组成
//通过Multimap进行信息的插入 保存 显示
//分部门显示员工信息 显示全部员工信息

#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<map>
#include<string>
#include<vector>
using namespace std;

//multimap 案例
//公司今天招聘了 5 个员工，5 名员工进入公司之后，需要指派员工在那个部门工作
//人员信息有: 姓名 年龄 电话 工资等组成
//通过 Multimap 进行信息的插入 保存 显示
//分部门显示员工信息 显示全部员工信息


#define SALE_DEPATMENT 1 //销售部门
#define DEVELOP_DEPATMENT 2 //研发部门
#define FINACIAL_DEPATMENT 3 //财务部门
#define ALL_DEPATMENT 4 //所有部门

//员工类
class person{
public:
	string name; //员工姓名
	int age; //员工年龄
	double salary; //员工工资
	string tele; //员工电话
};

//创建5个员工
void CreatePerson(vector<person>& vlist){

	string seed = "ABCDE";
	for (int i = 0; i < 5; i++){
		person p;
		p.name = "员工";
		p.name += seed[i];
		p.age = rand() % 30 + 20;
		p.salary = rand() % 20000 + 10000;
		p.tele = "010-8888888";
		vlist.push_back(p);
	}

}

//5名员工分配到不同的部门
void PersonByGroup(vector<person>& vlist, multimap<int, person>& plist){


	int operate = -1; //用户的操作

	for (vector<person>::iterator it = vlist.begin(); it != vlist.end(); it++){

		cout << "当前员工信息:" << endl;
		cout << "姓名：" << it->name << " 年龄:" << it->age << " 工资:" << it->salary << " 电话：" << it->tele << endl;
		cout << "请对该员工进行部门分配(1 销售部门, 2 研发部门, 3 财务部门):" << endl;
		scanf("%d", &operate);

		while (true){

			if (operate == SALE_DEPATMENT){  //将该员工加入到销售部门
				plist.insert(make_pair(SALE_DEPATMENT, *it));
				break;
			}
			else if (operate == DEVELOP_DEPATMENT){
				plist.insert(make_pair(DEVELOP_DEPATMENT, *it));
				break;
			}
			else if (operate == FINACIAL_DEPATMENT){
				plist.insert(make_pair(FINACIAL_DEPATMENT, *it));
				break;
			}
			else{
				cout << "您的输入有误，请重新输入(1 销售部门, 2 研发部门, 3 财务部门):" << endl;
				scanf("%d", &operate);
			}

		}

	}
	cout << "员工部门分配完毕!" << endl;
	cout << "***********************************************************" << endl;

}

//打印员工信息
void printList(multimap<int, person>& plist, int myoperate){

	if (myoperate == ALL_DEPATMENT){
		for (multimap<int, person>::iterator it = plist.begin(); it != plist.end(); it++){
			cout << "姓名：" << it->second.name << " 年龄:" << it->second.age << " 工资:" << it->second.salary << " 电话：" << it->second.tele << endl;
		}
		return;
	}

	multimap<int, person>::iterator it = plist.find(myoperate);
	int depatCount = plist.count(myoperate);
	int num = 0;
	if (it != plist.end()){
		while (it != plist.end() && num < depatCount){
			cout << "姓名：" << it->second.name << " 年龄:" << it->second.age << " 工资:" << it->second.salary << " 电话：" << it->second.tele << endl;
			it++;
			num++;
		}
	}
}

//根据用户操作显示不同部门的人员列表
void ShowPersonList(multimap<int, person>& plist, int myoperate){

	switch (myoperate)
	{
	case SALE_DEPATMENT:
		printList(plist, SALE_DEPATMENT);
		break;
	case DEVELOP_DEPATMENT:
		printList(plist, DEVELOP_DEPATMENT);
		break;
	case FINACIAL_DEPATMENT:
		printList(plist, FINACIAL_DEPATMENT);
		break;
	case ALL_DEPATMENT:
		printList(plist, ALL_DEPATMENT);
		break;
	}
}

//用户操作菜单
void PersonMenue(multimap<int, person>& plist){

	int flag = -1;
	int isexit = 0;
	while (true){
		cout << "请输入您的操作((1 销售部门, 2 研发部门, 3 财务部门, 4 所有部门, 0退出)：" << endl;
		scanf("%d", &flag);

		switch (flag)
		{
		case SALE_DEPATMENT:
			ShowPersonList(plist, SALE_DEPATMENT);
			break;
		case DEVELOP_DEPATMENT:
			ShowPersonList(plist, DEVELOP_DEPATMENT);
			break;
		case FINACIAL_DEPATMENT:
			ShowPersonList(plist, FINACIAL_DEPATMENT);
			break;
		case ALL_DEPATMENT:
			ShowPersonList(plist, ALL_DEPATMENT);
			break;
		case 0:
			isexit = 1;
			break;
		default:
			cout << "您的输入有误，请重新输入!" << endl;
			break;
		}

		if (isexit == 1){
			break;
		}
	}

}

int main(){

	vector<person>  vlist; //创建的5个员工 未分组
	multimap<int, person> plist; //保存分组后员工信息

	//创建5个员工
	CreatePerson(vlist);
	//5名员工分配到不同的部门
	PersonByGroup(vlist, plist);
	//根据用户输入显示不同部门员工信息列表 或者 显示全部员工的信息列表
	PersonMenue(plist);

	system("pause");
	return EXIT_SUCCESS;
}
```

### STL使用场景举例
	vector的使用场景：比如软件历史操作记录的存储。
	deque的使用场景：比如排队购票系统。
	list的使用场景：比如公交车乘客的存储。
	set的使用场景：比如对手机游戏的个人得分记录的存储。
	map的使用场景：比如按ID号存储十万个用户，想要快速要通过ID查找对应的用户。


# **STL—算法篇**
* 算法主要是由头文件< algorithm > < functional > < numeric >组成。

## 常用遍历算法
```cpp
/*
    遍历算法 遍历容器元素
	@param beg 开始迭代器
	@param end 结束迭代器
	@param _callback  函数回调或者函数对象
	@return 函数对象
*/
for_each(iterator beg, iterator end, _callback);
/*
	transform算法 将指定容器区间元素搬运到另一容器中
	注意 : transform 不会给目标容器分配内存，所以需要我们提前分配好内存
	@param beg1 源容器开始迭代器
	@param end1 源容器结束迭代器
	@param beg2 目标容器开始迭代器
	@param _cakkback 回调函数或者函数对象
	@return 返回目标容器迭代器
*/
transform(iterator beg1, iterator end1, iterator beg2, _callbakc)


for_each:
/*

template<class _InIt,class _Fn1> inline
void for_each(_InIt _First, _InIt _Last, _Fn1 _Func)
{
	for (; _First != _Last; ++_First)
		_Func(*_First);
}

*/

//普通函数
void print01(int val){
	cout << val << " ";
}
//函数对象
struct print001{
	void operator()(int val){
		cout << val << " ";
	}
};

//for_each算法基本用法
void test01(){

	vector<int> v;
	for (int i = 0; i < 10;i++){
		v.push_back(i);
	}

	//遍历算法
	for_each(v.begin(), v.end(), print01);
	cout << endl;

	for_each(v.begin(), v.end(), print001());
	cout << endl;

}

struct print02{
	print02(){
		mCount = 0;
	}
	void operator()(int val){
		cout << val << " ";
		mCount++;
	}
	int mCount;
};

//for_each返回值
void test02(){

	vector<int> v;
	for (int i = 0; i < 10; i++){
		v.push_back(i);
	}

	print02& p = for_each(v.begin(), v.end(), print02());
	cout << endl;
	cout << p.mCount << endl;
}

struct print03 : public binary_function<int, int, void>{
	void operator()(int val,int bindParam) const{
		cout << val + bindParam << " ";
	}
};

//for_each绑定参数输出
void test03(){

	vector<int> v;
	for (int i = 0; i < 10; i++){
		v.push_back(i);
	}

	for_each(v.begin(), v.end(), bind2nd(print03(),100));
}


transform:
//transform 将一个容器中的值搬运到另一个容器中
/*
	template<class _InIt, class _OutIt, class _Fn1> inline
	_OutIt _Transform(_InIt _First, _InIt _Last,_OutIt _Dest, _Fn1 _Func)
	{

		for (; _First != _Last; ++_First, ++_Dest)
			*_Dest = _Func(*_First);
		return (_Dest);
	}

	template<class _InIt1,class _InIt2,class _OutIt,class _Fn2> inline
	_OutIt _Transform(_InIt1 _First1, _InIt1 _Last1,_InIt2 _First2, _OutIt _Dest, _Fn2 _Func)
	{
		for (; _First1 != _Last1; ++_First1, ++_First2, ++_Dest)
			*_Dest = _Func(*_First1, *_First2);
		return (_Dest);
	}
*/

struct transformTest01{
	int operator()(int val){
		return val + 100;
	}
};
struct print01{
	void operator()(int val){
		cout << val << " ";
	}
};
void test01(){

	vector<int> vSource;
	for (int i = 0; i < 10;i ++){
		vSource.push_back(i + 1);
	}

	//目标容器
	vector<int> vTarget;
	//给vTarget开辟空间
	vTarget.resize(vSource.size());
	//将vSource中的元素搬运到vTarget
	vector<int>::iterator it = transform(vSource.begin(), vSource.end(), vTarget.begin(), transformTest01());
	//打印
	for_each(vTarget.begin(), vTarget.end(), print01()); cout << endl;

}

//将容器1和容器2中的元素相加放入到第三个容器中
struct transformTest02{
	int operator()(int v1,int v2){
		return v1 + v2;
	}
};
void test02(){

	vector<int> vSource1;
	vector<int> vSource2;
	for (int i = 0; i < 10; i++){
		vSource1.push_back(i + 1);
	}

	//目标容器
	vector<int> vTarget;
	//给vTarget开辟空间
	vTarget.resize(vSource1.size());
	transform(vSource1.begin(), vSource1.end(), vSource2.begin(),vTarget.begin(), transformTest02());
	//打印
	for_each(vTarget.begin(), vTarget.end(), print01()); cout << endl;
}

```
## 常用查找算法
```cpp
/*
	find算法 查找元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value 查找的元素
	@return 返回查找元素的位置
*/
find(iterator beg, iterator end, value)
/*
	adjacent_find算法 查找相邻重复元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  _callback 回调函数或者谓词(返回bool类型的函数对象)
	@return 返回相邻元素的第一个位置的迭代器
*/
adjacent_find(iterator beg, iterator end, _callback);
/*_
	binary_search算法 二分查找法
	注意: 在无序序列中不可用
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value 查找的元素
	@return bool 查找返回true 否则false
*/
bool binary_search(iterator beg, iterator end, value);
/*
	find_if算法 条件查找
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  callback 回调函数或者谓词(返回bool类型的函数对象)
	@return bool 查找返回true 否则false
*/
find_if(iterator beg, iterator end, _callback);
/*_
	count算法 统计元素出现次数
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  value回调函数或者谓词(返回bool类型的函数对象)
	@return int返回元素个数
*/
count(iterator beg, iterator end, value);
/*
	count算法 统计元素出现次数
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param  callback 回调函数或者谓词(返回bool类型的函数对象)
	@return int返回元素个数
*/
count_if(iterator beg, iterator end, _callback);

```
## 常用排序算法
```cpp
/*_
	merge算法 容器元素合并，并存储到另一容器中
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
*/
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)
/*
	sort算法 容器元素排序
	注意:两个容器必须是有序的
	@param beg 容器1开始迭代器
	@param end 容器1结束迭代器
	@param _callback 回调函数或者谓词(返回bool类型的函数对象)
*/
sort(iterator beg, iterator end, _callback)
/*_
	sort算法 对指定范围内的元素随机调整次序
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
*/
random_shuffle(iterator beg, iterator end)
/*
	reverse算法 反转指定范围的元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
*/
reverse(iterator beg, iterator end);

```
## 常用拷贝和替换算法
```cpp
/*
	copy算法 将容器内指定范围的元素拷贝到另一容器中
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param dest 目标容器结束迭代器
*/
copy(iterator beg, iterator end, iterator dest)
/*
	replace算法 将容器内指定范围的旧元素修改为新元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param oldvalue 旧元素
	@param oldvalue 新元素
*/
replace(iterator beg, iterator end, oldvalue, newvalue)
/*
	replace_if算法 将容器内指定范围满足条件的元素替换为新元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param callback函数回调或者谓词(返回Bool类型的函数对象)
	@param oldvalue 新元素
*/
replace_if(iterator beg, iterator end, _callback, newvalue)
/*
	swap算法 互换两个容器的元素
	@param c1容器1
	@param c2容器2
*/
swap(container c1, container c2);

```
## 常用算数生成算法
```cpp
/*
	accumulate算法 计算容器元素累计总和
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value累加值
*/
accumulate(iterator beg, iterator end, value);
/*
	fill算法 向容器中添加元素
	@param beg 容器开始迭代器
	@param end 容器结束迭代器
	@param value t填充元素
*/
fill(iterator beg, iterator end, value);

```
## 常用集合算法
```cpp
/*
	set_intersection算法 求两个set集合的交集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
/*
	set_union算法 求两个set集合的并集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
/*
	set_difference算法 求两个set集合的差集
	注意:两个集合必须是有序序列
	@param beg1 容器1开始迭代器
	@param end1 容器1结束迭代器
	@param beg2 容器2开始迭代器
	@param end2 容器2结束迭代器
	@param dest  目标容器开始迭代器
	@return 目标容器的最后一个元素的迭代器地址
*/
set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);

```
