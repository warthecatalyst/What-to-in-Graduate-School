# C++
## 常见面试题
1、**C++ STL的内存分配策略**
C++ STL中内存分配是通过allocator实现的。allocator是一个由两级分配器构成的内存管理器，当申请的内存大小大于128byte时，就启动第一级分配器通过malloc直接向系统的堆空间分配，如果申请的内存大小小于128byte时，就启动第二级分配器，从一个预先分配好的内存池中取一块内存交付给用户，这个内存池由16个不同大小（8的倍数，8~128byte）的空闲列表组成，allocator会根据申请内存的大小（将这个大小round up成8的倍数）从对应的空闲块列表取表头块给用户。

2、**const关键字作用**
修饰变量，说明该变量不可以被改变；
修饰指针，分为指向常量的指针（pointer to const）和自身是常量的指针（常量指针，const pointer）；
修饰引用，指向常量的引用（reference to const），用于形参类型，即避免了拷贝，又避免了函数对值的修改；
修饰成员函数，说明该成员函数内不能修改成员变量。

3、**explicit（显式）关键字**
explicit 修饰构造函数时，可以防止隐式转换和复制初始化
explicit 修饰转换函数时，可以防止隐式转换，但按语境转换除外

4、**介绍一下C++的多态类型**
多态，即多种状态（形态）。简单来说，我们可以将多态定义为消息以多种形式显示的能力。
多态是以封装和继承为基础的。
C++ 多态分类及实现：
重载多态（Ad-hoc Polymorphism，编译期）：函数重载、运算符重载
子类型多态（Subtype Polymorphism，运行期）：虚函数
参数多态性（Parametric Polymorphism，编译期）：类模板、函数模板
强制多态（Coercion Polymorphism，编译期/运行期）：基本类型转换、自定义类型转换

5、**C++的智能指针**
shared_ptr
unique_ptr
weak_ptr
auto_ptr（被 C++11 弃用）
Class shared_ptr 实现共享式拥有（shared ownership）概念。多个智能指针指向相同对象，该对象和其相关资源会在 “最后一个 reference 被销毁” 时被释放。为了在结构较复杂的情景中执行上述工作，标准库提供 weak_ptr、bad_weak_ptr 和 enable_shared_from_this 等辅助类。
Class unique_ptr 实现独占式拥有（exclusive ownership）或严格拥有（strict ownership）概念，保证同一时间内只有一个智能指针可以指向该对象。你可以移交拥有权。它对于避免内存泄漏（resource leak）——如 new 后忘记 delete ——特别有用。
shared_ptr
多个智能指针可以共享同一个对象，对象的最末一个拥有着有责任销毁对象，并清理与该对象相关的所有资源。

支持定制型删除器（custom deleter），可防范 Cross-DLL 问题（对象在动态链接库（DLL）中被 new 创建，却在另一个 DLL 内被 delete 销毁）、自动解除互斥锁
weak_ptr
weak_ptr 允许你共享但不拥有某对象，一旦最末一个拥有该对象的智能指针失去了所有权，任何 weak_ptr 都会自动成空（empty）。因此，在 default 和 copy 构造函数之外，weak_ptr 只提供 “接受一shared_ptr” 的构造函数。

可打破环状引用（cycles of references，两个其实已经没有被使用的对象彼此互指，使之看似还在 “被使用” 的状态）的问题

unique_ptr
unique_ptr是C++11 才开始提供的类型，是一种在异常时可以帮助避免资源泄漏的智能指针。采用独占式拥有，意味着可以确保一个对象和其相应的资源同一时间只被一个pointer拥有。一旦拥有着被销毁或变成empty，或开始拥有另一个对象，先前拥有的那个对象就会被销毁，其任何相应资源亦会被释放。

unique_ptr 用于取代 auto_ptr
智能指针是线程安全的，但是对智能指针指向对象的操作则要开发人员自己去注意。


