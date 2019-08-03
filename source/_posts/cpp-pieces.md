---
date: 2010-11-28T06:52:25Z
title: C++代码例子集合
slug: cpp_codes
---

## 成员变量的地址

    #include <stdio.h>
    #include <stdlib.h>
    
    struct Test
    {
        int a;int b;
    };
    
    int main()
    {
        int *i = (&((Test*)0)->b);
        printf("%d",i);
        
        system("PAUSE");
        return 0;
    }

## struct对齐一题
    #include <stdio.h>
    #include <stdlib.h>
    
    struct A
    {
        int a;
        double b;
        char c;
        short d;
    };
    
    struct B
    {
        double b;
        int a;
        char c;
        short d;
    };
    
    int main()
    {
        printf("%d\n",sizeof(A));    
        printf("%d\n",sizeof(B));
        system("PAUSE");
        return 0;
    }

对齐规则： 类型必须从类型size的整数倍开始，结构体size必须为结构体内类型最大的整数倍。
struct A: int 4 double 8 必须从8开始，故为16，char为17，short必须从2字节整数倍，从18开始，故为20，结构体必须为8的整数倍，所以必须为24。
struct B:double 8,int 4,char 1 对齐到14，从short开始2，一共为16

## 私有复制构造函数无实现仍然可以调用
    /*  
    *    Doc Name: Private copy constructor with none defination
    *    Prob Id: -
    *    Serial Id: A3
    *    Author: -
    *    Date: 10/10/28
    */
    
    
    #include "stdafx.h"
    #include <iostream>
    using namespace std;
    
    class A
    {
    public:
        A(){}
    public:
        void foo(){ A inst; A inst2(inst); return;}
    private:
        A(const A& src);
    };
    
    int _tmain(int argc, _TCHAR* argv[])
    {
        A instA;
    //    A instA2(instA);
        return 0;
    }

## 构造，复制构造，赋值函数例子
    /*  
    *    Doc Name: Basic Concepts: constructor, copy constructor,
    *                assignment operation
    *    Prob Id: -
    *    Serial Id: A.2.2
    *    Author: -
    *    Env: Microsoft Visual Studio 2008 Win32 Console
    *    Date: 10/10/27
    */
    
    #include "stdafx.h"
    #include <iostream>
    using namespace std;
    
    class A
    {
    public:
        A(){cout<<"ctor"<<endl;}
        A(int i){cout<<"ctor int"<<endl;}
        A(const A& src){cout<<"copy ctor"<<endl;}
        A& operator=(const A& rhs){cout<<"assgin op"<<endl; return (*this);}
    };
    
    void println(const int n)
    {
        cout<<"------- case "<<n<<"-------"<<endl;
    }
    
    A& getInstRef()
    {
        A* p = new A();
        return *p; 
    }
    
    A getInst()
    {
        A* p = new A();
        return *p;
    }
    
    int _tmain(int argc, _TCHAR* argv[])
    {
        println(1);
        A instA; //ctor in stack
        println(2);
        A* pint;  //nothing done
        println(3);
        pint = new A(); //ctor heap
        
        println(4);
        A instAA(instA); //copy ctor
        println(5);
        A instAAA = instA; //copy ctor
        
        println(6);
        instAA = instAAA; //assign op
        
        println(7);
        A instAAAA = getInstRef(); //temp instance - return refrence to construct
        println(8);
        instAAAA = getInstRef(); //temp instance - return refrence to copy construct
        
        println(9);
        A instA5 = getInst(); //temp instance - return instance to construct
        println(10);
        instA5 = getInst(); //temp instance - return instance to copy construct
    
        println(11);
        A instA6 = 3;
        println(12);
        A instA7;
        instA7 = 4;
    
        return 0;
    }

## 类仅有复制构造函数时，将覆盖默认构造函数和复制构造函数
    /*  
    *    Doc Name: Basic Concepts: constructor, copy constructor,
                   assignment operation
    *    Prob Id: -
    *    Serial Id: A.2.1
    *    Author: -
    *    Env: DevCpp mingw
    *    Date: 10/10/27
    */
    
    #include <iostream>
    #include <exception>
    using namespace std;
    
    class A
    {
    public:
           A(const A& src){}
    };
    
    int main()
    {
        A instA;
        system("PAUSE");
        return 0;
    }

## `c++异常`：多个catch只走一个分支

    /*  
    *    Doc Name: Multiple Catch / Function throw
    *    Prob Id: -
    *    Serial Id: A.1
    *    Author: -
    *     Env: VS2008 Win32 Console
    *    Date: 10/10/27
    */
    #include "stdafx.h"
    #include <iostream>
    using namespace std;
    
    void foo() throw ()
    {
        throw 5;
    }
    
    int _tmain(int argc, _TCHAR* argv[])
    {
    
        try{
            foo();
        }
        catch(float e){
            cout<<"caught global 1"<<endl;
        }
        catch(int e){
            cout<<"caught errno:"<<e<<endl;
        }
        catch()
        {
            //exception should not caught here      
            cout<<"caught global 2"<<endl;
        }
    
        return 0;
    }

## 虚函数，覆盖的笔试题一则
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class base
    {
        public:
            base():m(3){}
            int m;
        public:
            int GetValue(){return --m;}
    };
    
    class derived1 :public base
    {
        public:
            virtual int GetValue(){return ++m;}
    };
    
    class derived2 :public derived1
    {
        public:
            int GetValue(){return m*=2;}
    };
    
    int main()
    {
        derived2 *pd2 = new derived2();
        derived2 &rd2 = *pd2;
        pd2->GetValue();
        
        derived1 *pd = pd2;
        derived1 &rd = *pd;
        pd->GetValue();
      
        base *pb = pd;
        base &rb = *pb;
        pb->GetValue(); 
        
        printf("%d\n", rb.GetValue());
        system("PAUSE");
        return 0;
    }

## 父类对象引用子类对象实现的多态
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class base
    {
        public:
            virtual void foo(){cout<<"base foo"<<endl;}
    };
    
    class derived :public base
    {
        public:
            virtual void foo(){cout<<"derived foo"<<endl;}
    };
    
    int main()
    {
        base *pb = new derived();
        base &rb = *pb;
        rb.foo();
        system("PAUSE");
        return 0;
    }

## 虚析构函数必要性
未声明virtual：
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class base
    {
        public:
            ~base(){cout<<"base destr"<<endl;}
    };
    
    class derived :public base
    {
        public:
            ~derived(){cout<<"derived destr"<<endl;}
    };
    
    int main()
    {
        base *pb = new derived();
        delete pb;
        system("PAUSE");
        return 0;
    }

声明virtual：
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class base
    {
        public:
            virtual ~base(){cout<<"base destr"<<endl;}
    };
    
    class derived :public base
    {
        public:
            virtual ~derived(){cout<<"derived destr"<<endl;}
            
            //~derived(){cout<<"derived destr"<<endl;} //不加virtual也可以，默认会继承 
    };
    
    int main()
    {
        base *pb = new derived();
        delete pb;
        system("PAUSE");
        return 0;
    }

## `++,--`顺序对return同样适用
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    int foo(int i)
    {
        return i--;
    }
    
    int main()
    {
        cout<<foo(1)<<endl;
        system("PAUSE");
        return 0;
    }


output：
1

## 正负对模运算的影响
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    int main()
    {
        cout<<"mod 3"<<endl;
        for(int i=-10; i<10; i++)
            cout<<i<<' '<<i%3<<endl;
        
        cout<<"mod -3"<<endl;
        for(int i=-10; i<10; i++)
            cout<<i<<' '<<i%(-3)<<endl;
            
        system("PAUSE");
        return 0;
    }

结果
    mod 3
    -10 -1
    -9 0
    -8 -2
    -7 -1
    -6 0
    -5 -2
    -4 -1
    -3 0
    -2 -2
    -1 -1
    0 0
    1 1
    2 2
    3 0
    4 1
    5 2
    6 0
    7 1
    8 2
    9 0
    mod -3
    -10 -1
    -9 0
    -8 -2
    -7 -1
    -6 0
    -5 -2
    -4 -1
    -3 0
    -2 -2
    -1 -1
    0 0
    1 1
    2 2
    3 0
    4 1
    5 2
    6 0
    7 1
    8 2
    9 0

## 运算符重载回忆代码
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
        public:
            Foo(int ii=0):i(ii){}
            Foo operator +(Foo& rhs)
            {
                return Foo(i+rhs.i);
            }
            operator int*(){ int* p = new int(i); return p;}
            
        void print(){cout<<i<<endl;}
        private:
            int i;
    };
    
    int main()
    {
        Foo f(1);
        Foo ff(2);
        Foo re;
        re = f + ff;
        f.print();
        re.print();
        int* p= (int*)f;
        cout<<*p<<endl;
        system("PAUSE");
        return 0;
    }

## new malloc 区别

1. new 不但分配对象空间, 还会在分配后调用对象的构造器, 而 malloc 只是分配, 不构造:

    class A {
    public:
    A(): m_value(0)
    {
    }
    
    public:
    int m_value;
    };
    
    struct B {
    A a;
    };
    
    B *pb1 = new B;
    B *pb2 = (B *)malloc(sizeof(B));

pb1 因为被 new 调用了构造函数, 所以 `pb1->m_value == 0`,
pb2 的构造函数没有被调用, 所以 pb2-\>m_value 的值不确定(即未初始化).

2. new 分配时要指定类型, 是类型安全的; 而 malloc 返回的是 `void *`, 类型不安全; 类型不安全的东西在 `C++` 里是不被提倡的! 

## 对数组资源的申请和释放
    int *pi = new int;
    delete pi;
    char *pchar = new char;
    delete pchar;
    int *piArray = new int[10];
    delete[] piArray;
    char* pCharArray = new char[10];
    delete[] pCharArray;

## 自定义析构函数和delete &object不能共存
    /*
      Name: double delete
      Copyright: 
      Author: elprup
      Date: 08/10/10 09:18
      Description: 
    */
    
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class bar
    {
    public:
        bar():i(0){}
        ~bar(){cout<<"destrucotr"<<endl;}
        int i;
    };
    
    
    int main()
    {
        bar b;
        delete &b;
        
        
        system("PAUSE");
        return 0;
    }

## copy构造函数可以访问同类型参数的私有成员，却不能访问非同类的参数的私有成员
    /*
      Name: copy consturctor can visit private varity of its parameter
      Copyright: 
      Author: elprup
      Date: 08/10/10 09:18
      Description: 
    */
    
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class bar
    {
        private:
            int m_bi;
    };
    
    
    class foo
    {
    public:
        foo(int i=0):m_i(i){}
        foo(foo& f){m_i = f.m_i;} //visit param f.m_i;
        foo(bar& b){m_i = b.m_bi;} //visit param b.m_bi;
        void p(){cout<<m_i<<endl;}
    private:
        int m_i;
    };
    
    
    
    int main()
    {
        foo f(3);
        foo f2(f);
    //    f2.m_i; //wrong, can't access
        f2.p();  //right, get value;
        
        system("PAUSE");
        return 0;
    }

## 隐式的copy构造函数
    /*
      Name: temporary varity copy consturctor
      Copyright: 
      Author: elprup
      Date: 08/10/10 09:18
      Description: 
    */
    
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class bar;
    
    class foo
    {
    public:
        foo():pb(0){}
        // do this if not define operator bar &
        foo(bar& b){cout<<"copy constructor"<<endl;}
        foo& operator=(const foo& f){cout<<"operator equal"<<endl; return *this;}
        // if define function below, skip copy constructor and oprator= foo
        foo& operator=(const bar& b){cout<<"bar operator equal"<<endl; return *this;}
    private:
        bar *pb;
    };
    
    class bar
    {
    };
    
    int main()
    {
        foo f;
        f = b;
        
        system("PAUSE");
        return 0;
    }

## `c++沉思录` 代码集 2
    /*
      Name: Why C++ ?(2)(plus on/off funtion)
      Copyright: 
      Author: elprup
      Date: 08/10/10 09:18
      Description: 
    */
    #include <cstdio>
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    //on/off for c on/off function
    bool on = true;
    
    int print(const char* s)
    {
        extern bool on;
        if(on)
            printf("%s\n", s);
        return 0;    
    }
    
    void badguy()
    {
        extern bool on;
        on = false; //modify global varity
        return;
    }
    
    class trace_cpp
    {
    public:
        //easy to initial state of class, but c need special attention.
        trace_cpp():m_on(true){}
        int print(const char*s)const{if(m_on) printf("%s\n", s); return 0;}
        int on(){m_on = true; return 0;}
        int off(){m_on = false; return 0;}
    private:
        //add class related varity, not see out of class
        bool m_on;
    };
    
    int main()
    {
        //c style trace
        print("Hello from c.");
        
        //cpp style trace;
        trace_cpp tc;
        tc.print("Hello from c++");
        
        //turn on/off c style trace
        extern bool on;
        on = false;
        print("Wont display from c.");
        on = true;
        print("Will display from c.");
        //it looks working fine, but if foo() modify it?
        badguy();
        print("Want to display, but won't from c.");
        
        //turn on/off cpp style
        tc.off();
        tc.print("Wont display from cpp.");
        tc.on();
        tc.print("Will display from cpp.");
        //so we hardly find a function to modify state of trace cpp
        
        system("PAUSE");
        return 0;
    }

## `c++沉思录` 代码集 1
    /*
      Name: Why C++ ?(basic funtion)
      Copyright: 
      Author: elprup
      Date: 08/10/10 09:18
      Description: 
    */
    #include <cstdio>
    #include <cstdlib>
    
    #include <iostream>
    using namespace std;
    
    int print(const char* s)
    {
        printf("%s\n", s);
        return 0;    
    }
    
    class trace_cpp
    {
    public:
        int print(const char*s){printf("%s\n", s); return 0;}
    };
    
    int main()
    {
        //c style trace
        print("Hello from c.");
        
        //cpp style trace;
        trace_cpp tc;
        tc.print("Hello from c++");
        
        system("PAUSE");
        return 0;
    }

## 函数指针的取地址和解引用
    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <stdlib.h>
    using namespace std;
    
    int pdc(int n){return 1;}
    typedef int (*fp)(int n);
    
    int main()
    {
        fp f1 = &pdc;
        fp* f2 = &f1;
        (*f2)(2);
        (**f2)(2);
        (***f2)(2);
        (**********************************************f2)(2);
        system("PAUSE");
        return 0;
    }

## 模板模板参数
    #include <iostream>
    #include <vector>
    #include <algorithm>
    using namespace std;
    
    template <typename T, template<typename E> class Container>
    struct Foo
    {
        public:
            T data;
            Container<T> cont;
    };
    
    int main()
    {
        Foo<int,vector> f;
        f.data = 9;
        f.cont.push_back(9);
        return 0;
    }

## 声明模板友元函数
    #include <iostream>
    #include <vector>
    #include <stdlib.h>
    using namespace std;
    
    template<typename T> class person;
    
    template<typename T>
    ostream& operator<<(ostream & ostr, person<T>& p);
    
    template<typename T>
    class person
    {
    public:
        person(T _data):data(_data){cout<<"person constructor"<<endl;}
        person(person<T>& rhs){cout<<"person copy constructor"<<endl;}
        person<T>& operator=(person<T>& rhs){ cout<<"person operator ="<<endl; return *this;}
        ~person(){cout<<"person destructor"<<endl;}
        
        friend ostream& operator<< <T>(ostream& ostr, person<T>& p);
        
    private:
        T data;
    };
    
    template<typename T>
    ostream& operator<<(ostream& ostr, person<T>& p){
        ostr << p.data << endl;
        return ostr;
    }
    
    int main()
    {
        person<int> man(1);
        cout<<man<<endl;
        system("PAUSE");
        return 0;
    }

## 函数指针和重写的`for_each`模板函数
    #include <algorithm>
    #include <iostream>
    #include <vector>
    #include <functional>
    using namespace std;
    
    template<typename T, typename _func>
    _func formyeach(T b, T e, _func func){
        for(;b!=e;++b)
        {
            func(*b);
        }
        return func;
    }
    
    
    void print(int elem){
        cout<<elem<<endl;
    }
    
    typedef void(* second_print)(int);
    
    int main()
    {
        
        vector<int> v(30,9);
        for_each(v.begin(),v.end(),&print);
        cout<<"------------------------------"<<endl;
        formyeach(v.begin(),v.end(),&print);
        second_print fun= &print;
        fun(3);
        return 0;
    }

## 函数对象和模板结合一例
    #include <iostream>
    #include <vector>
    #include <string>
    using namespace std;
    
    template<typename T>
    class fun{
          public:
          void operator() (T param){cout<<param<<endl;}
    };
    
    template<typename T, typename _FUNC>
    void do_if(vector<T> v,  _FUNC f)
    {
         (*f)(v[0]);
         return;
    }
    
    int main()
    {
        vector<int> v(10,2);
        do_if<int, fun<int>*>(v, &fun<int>());
        system("PAUSE");
        vector<string> v2(10,"A");
        do_if<string, fun<string>*>(v2, &fun<string>());
        system("PAUSE");   
        return 0;
    }
    

## const reference 被引用对象可修改的例子
    #include <cstdio>
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    int main(){
        int a = 9;
        const int &refa = a;
        refa = 0; //Error:assignment of read-only reference 'refa'
        a = 0;
        
        system("PAUSE");
        return 0;
    } 

## 模板类特殊化
    #include <cstdio>
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    template<typename T>
    class Example{
        public:
            T data;
        public:
            Example(T _data):data(_data){cout<<"Initial data="<<data<<endl;}
            void print(){cout<<"print data:"<<data<<endl;}
    };
    
    template<>
    class Example<double>{
        public:
            double data;
        public:
            Example(double _data):data(_data){cout<<"Special Initial data="<<data<<endl;}
            void print(){cout<<"Special print data:"<<data<<endl;}
    };
    
    int main(){
        Example<int> e_int(5);
        e_int.print();
        
        Example<double> e_double(1.2);
        e_double.print();
        
        system("PAUSE");
        return 0;
    } 
    

## 运算符`||`的结合律是从左往右还是从右往左
今见一网页声称`||`结合为从右往左，甚感奇怪，故用一程序探之。
    int main()
    {
        int *p;
        p = 0;
        int i;
        if((i=*p)||1)
            ;
        return 0;
    }
若`||`为真，则`i=*p;`不会执行。否则，出现非法访问错误。

结果：`||`结合律为从左往右。

## `c++`不能用对象实例实现多态/ 基类和继承类实例转换时出现的成员丢失（cut）

    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        int i;
        void showi(){cout<<i<<endl;}
    };
    
    class Derived : public Base
    {
    public:
        int j;
        void showj(){cout<<j<<endl;}
        Derived& operator=(const Base& other){i = other.i; return *this;}
    };
    
    int main()
    {
        Base b;
        b.i = 0;
        Derived d,d2;
        d.i = 1;
        d.j = 2;
        b = d;
        b.showi();
        system("PAUSE");
        d2 = b;
        d2.showi();
        d2.showj();
        system("PAUSE");
        return 0;
    }

## 枚举类型和整形的转换和比较
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    typedef enum _t{A,B,C}T;
    
    int main()
    {
        int i;
        T j,k;
        i = 0;
        k= B;
        j = (T)1;
        cout<<j<<endl;
        cout<<(j==k)<<endl;
        system("PAUSE");
        return 0;
    }

## `->`的优先级比`*`高
    #include <iostream>
    #include <cstdlib>
    
    using namespace std;
    
    typedef struct _node{
        int a;
        int *p;
    }node;
    
    int main()
    {
        node n;
        node *p = &n;
        node **pp = &p;
        
        int b = 2;
        n.a = 1;
        n.p = &b;
        cout << (*pp)->a << endl;
        // cout << *pp->a <<endl; //error node* has no member a
    
        system("PAUSE");
        return 0;
    }

## 指针的引用如何声明
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    //void foo(int* &p){} //Error
    void foo(int& *p){} //Ok
    
    int
    main()
    {
        int *p;
        foo(p);
        system("PAUSE");
        return 0;
    }

## `c++`中隐藏规则 的避免
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        void foo(int i){}
    };
    
    class Derived : public Base
    {
    public:
        void foo(string s){}
    };
    
    int main()
    {
        Derived d;
        d.Base::foo(1024);
        
        system("PAUSE");
        return 0;
    }
    
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        void foo(int i){}
        void bar(){}
    };
    
    class Derived : public Base
    {
    public:
        void foo(string s){}
        int bar;
    };
    
    int main()
    {
        Derived d;
        d.bar = 0;
        d.Base::bar();
        
        system("PAUSE");
        return 0;
    }



    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        void foo(int i){}
        int bar;
    };
    
    class Derived : public Base
    {
    public:
        void foo(string s){}
        void bar(){}
    };
    
    int main()
    {
        Derived d;
        d.Base::bar = 0;
        d.bar();
        
        system("PAUSE");
        return 0;
    }


    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        void foo(int i){}
    };
    
    class Derived : public Base
    {
    public:
        void foo(string s){}
        using Base::foo;
    };
    
    int main()
    {
        Derived d;
        d.foo("");
        d.foo(1024);
        
        system("PAUSE");
        return 0;
    }

## 子类中定义基类的访问控制标签
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        int i;
    };
    
    class Derived : public Base
    {
    //private:
    //    using Base::i;
    };
    
    
    int main()
    {
        Base baseObj;
        baseObj.i = 0;
        
        Derived derivedObj;
        derivedObj.i = 0;
        
        system("PAUSE");
        return 0;
    }

## 子类避免覆盖，调用父类虚函数
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Base
    {
    public:
        virtual int foo(int i){return i;}
    };
    
    class Derived : public Base
    {
    public:
        virtual int foo(int i){return i+1;}
    };
    
    int main()
    {
        Base *p = new Derived;
        cout<<p->foo(1)<<endl;
        cout<<p->Base::foo(1)<<endl;
        
        system("PAUSE");
        return 0;
    }

## 析构函数不会delete指针成员变量
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Kit
    {
    public:
        Kit(int ii):i(ii){}
        int i;
    };
    
    class Foo
    {
    public:
        Kit *p;
    };
    
    int main()
    {
        Foo *pf = new Foo;
        pf->p = new Kit(10);
        Kit *pk = pf->p;
        delete pf;
        cout<<pk->i<<endl; //Foo destructure dont delete data member pointer
        
        system("PAUSE");
        return 0;
    }

## copy构造函数的参数可为reference或者constant reference
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int i){}
        Foo(Foo& other){cout<<'*'<<endl;}
        Foo(const Foo& other){cout<<'&'<<endl;}
    };
    
    int main()
    {
        Foo f(1);
        Foo ff = f;
        system("PAUSE");
        return 0;
    }

## 数组进行传递时，sizeof变化
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    void foo(char s[])
    {
        cout<<sizeof(s)<<endl;
    }
    
    int main()
    {
        char s[] = "Hello";
        cout<<sizeof(s)<<endl;
        char *p = s;
        cout<<sizeof(p)<<endl;
        foo(s);
        system("PAUSE");
        return 0;
    }
    

## 类指针成员的一种管理方法： 值和指针的结合
 
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    struct bottle
    {
        int refCount;
        int weight;
        int size;
        bottle(int w,int sz):refCount(1),weight(w),size(sz){}
        bottle* getOwnCopy()
        {
            if(refCount==1)
                return this;
            refCount--;
            return new bottle(weight,size);
        }
    private:
        bottle(const bottle&);
        bottle& operator=(const bottle&);
    };
    
    class ProductBottle
    {
    public:
        ProductBottle(int w=10,int sz=20):pinst(new bottle(w,sz)){}
        ProductBottle(const ProductBottle& other)
        {
            other.pinst->refCount++;
            pinst = other.pinst;
        }
        ProductBottle& operator=(const ProductBottle& rhs)
        {
            rhs.pinst->refCount++;
            if(--pinst->refCount==0)
                delete pinst;
            pinst = rhs.pinst;
            return *this;
        }
        ~ProductBottle()
        {
            if(--pinst->refCount==0)
                delete pinst;
        }
        int AddWater(int waterWeight)
        {
            pinst = pinst->getOwnCopy(); 
            pinst->weight += waterWeight;
            return 0; 
        }
        int show()
        {
            cout<<pinst->weight<<' '<<pinst->size<<' '<<pinst->refCount<<endl;
        }
    private:
        bottle* pinst;
    };
    
    int main()
    {
        ProductBottle Cola;
        ProductBottle Sprint(Cola);
        ProductBottle Finda(Sprint);
        /*****************************/
        Cola.show();
        Sprint.show();
        Finda.show();
        /*****************************/
        Cola.AddWater(50);
        /*****************************/
        Cola.show();
        Sprint.show();
        Finda.show();
        /*****************************/    
        system("PAUSE");
        return 0;
    }

## 引起内存泄露的一种原因 函数中new对象，没有被delete
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(){cout<<"constructed"<<endl;}
        ~Foo(){cout<<"destructed"<<endl;}
        int array[10000];
    };
    
    Foo* handleFoo(Foo* pf)
    {
        Foo* pf2 = new Foo();
        return pf;
    }
    
    int main()
    {
        Foo* pf;
        while(true)
        {
            pf = handleFoo(pf);
        }
        system("PAUSE");
        return 0;
    }

警告：本代码会引起最终内存耗尽。在windows下的同学可以看到进程管理器中一条优美的内存增长曲线。

## 函数调用封装成类--运算符()的重载
原来的代码：
    #include <cstdlib>
    #include <iostream>
    #include <vector>
    using namespace std;
    
    class trival
    {
    public:
        void show(){cout<<e1<<' '<<e2<<' '<<e3<<endl;}
    private:
        int e1,e2,e3;
    };
    
    void show(trival& t)
    {
        t.show();    
    }
    
    int main()
    {
        vector<trival> tt(5);  
        for_each(tt.begin(),tt.end(),show);
        system("PAUSE");
        return 0;
    }

如果我需要累加三元组的第一号元素
    #include <cstdlib>
    #include <iostream>
    #include <vector>
    using namespace std;
    
    class trival
    {
    public:
        void show(){cout<<e1<<' '<<e2<<' '<<e3<<endl;}
    public:
        int e1,e2,e3;
    };
    
    void show(trival& t)
    {
        t.show();    
    }
    
    void inc(trival& t)
    {
        t.e1++;
    }
    
    int main()
    {
        vector<trival> tt(5);  
        for_each(tt.begin(),tt.end(),show);
        for_each(tt.begin(),tt.end(),inc);
        for_each(tt.begin(),tt.end(),show);
        system("PAUSE");
        return 0;
    }

问题出现了，如果我想自定义给三元组的每个元素加n呢？于是。。。
    #include <cstdlib>
    #include <iostream>
    #include <vector>
    using namespace std;
    
    class trival
    {
    public:
        void show(){cout<<e1<<' '<<e2<<' '<<e3<<endl;}
    public:
        int e1,e2,e3;
    };
    
    void show(trival& t)
    {
        t.show();    
    }
    
    class add
    {
    public:
        add(int ae1=0, int ae2=0, int ae3=0):adde1(ae1),adde2(ae2),adde3(ae3){}
        void operator()(trival& t){t.e1+=adde1; t.e2+=adde2; t.e3+=adde3;}
    private:
        int adde1,adde2,adde3;
    };
    
    int main()
    {
        vector<trival> tt(5);  
        for_each(tt.begin(),tt.end(),show);
        for_each(tt.begin(),tt.end(),add(1,2));
        for_each(tt.begin(),tt.end(),show);
        system("PAUSE");
        return 0;
    }


是不是很有意思?

## 类有指针成员的处理（2）传值
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    
    class Foo1
    {
    public:
        Foo1(int* pp, int vv):p(new int(*pp)),val(vv){cout<<"Foo construction\n";}
        
        Foo1(const Foo1& other){cout<<"Foo1 copy\n"; p = new int(*other.p); val=other.val;}
        Foo1& operator=(Foo1& rhs){cout<<"Foo1 assign\n"; delete p; p = new int(*rhs.p); val=rhs.val; return *this;}
        ~Foo1(){cout<<"Foo1 destruction\n"; delete p;}
        
        void show(){cout<<*p<<' '<<val<<endl;}
    public:
        int *p;
        int val;
    };
    
    
    
    int main()
    {
        int a=9,b=10;
        int* p=&a;
        int* pp=&b;
        Foo1 f(p,a);
        Foo1 f2(f);
        Foo1 f3(pp,b);
        f3 = f2;
        f.show();f2.show();f3.show();
        (*f.p)++;
        f.show();f2.show();f3.show();
        return 0;
    }
    

## copy和copy assignment的区别
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class foo
    {
    public:
        foo(){}
        foo(const foo& other){cout<<"copy\n";}
        foo& operator=(foo& rhs){cout<<"assign\n"; return *this;}
    };
    
    int main()
    {
        foo f;
        foo f2 = f;
        foo f3;
        f3 = f;
        system("PAUSE");
        return 0;
    }

## 类有指针成员时的处理（1）拷贝时复制指针
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class u_ptr
    {
        friend class Foo1;
    private:
        u_ptr(int* pp):p(pp),usecount(1){cout<<"u_ptr construction\n";}
    private:
        int usecount;
        int *p;
    };
    
    class Foo1
    {
    public:
        Foo1(int* pp, int vv):p(new u_ptr(pp)),val(vv){cout<<"Foo construction\n";}
        
        Foo1(const Foo1& other){ p = other.p; val=other.val; p->usecount++;cout<<"Foo1 copy"<<p->usecount<<"\n";}
        Foo1& operator=(Foo1& rhs){ rhs.p->usecount++; if(--(p->usecount)==0){cout<<"delete p"<<endl; delete p;} p = rhs.p; val=rhs.val;cout<<"Foo1 assign"<<p->usecount<<"\n"; return *this;}
        ~Foo1(){ if(--(p->usecount)==0) delete p;cout<<"Foo1 destruction"<<p->usecount<<"\n";}
        
    private:
        u_ptr *p;
        int val;
    };
    
    
    
    int main()
    {
        int a=9,b=10;
        int* p=&a;
        int* pp=&b;
        Foo1 f(p,a);
        Foo1 f2(f);
        Foo1 f3(pp,b);
        f3=f2;
        return 0;
    }


为了看清楚指针指向，改为public
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class u_ptr
    {
        friend class Foo1;
    private:
        u_ptr(int* pp):p(pp),usecount(1){cout<<"u_ptr construction\n";}
    public:
        int usecount;
        int *p;
    };
    
    class Foo1
    {
    public:
        Foo1(int* pp, int vv):p(new u_ptr(pp)),val(vv){cout<<"Foo construction\n";}
        
        Foo1(const Foo1& other){ p = other.p; val=other.val; p->usecount++;cout<<"Foo1 copy"<<p->usecount<<"\n";}
        Foo1& operator=(Foo1& rhs){ rhs.p->usecount++; if(--(p->usecount)==0){cout<<"delete p"<<endl; delete p;} p = rhs.p; val=rhs.val;cout<<"Foo1 assign"<<p->usecount<<"\n"; return *this;}
        ~Foo1(){ if(--(p->usecount)==0) delete p;cout<<"Foo1 destruction"<<p->usecount<<"\n";}
        
        void show(){cout<<*(p->p)<<' '<<val<<endl;}
    public:
        u_ptr *p;
        int val;
    };
    
    
    
    int main()
    {
        int a=9,b=10;
        int* p=&a;
        int* pp=&b;
        Foo1 f(p,a);
        Foo1 f2(f);
        Foo1 f3(pp,b);
        f3 = f2;
        f.show();f2.show();f3.show();
        (*f.p->p)++;
        f.show();f2.show();f3.show();
        return 0;
    }

## 友元函数访问私有成员变量
    #include <cstdlib>
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Foo
    {
    public:
        Foo():s(""){}
        void show(){cout<<s<<endl;}
        friend istream& operator>>(istream&,Foo&);
    private:
        string s;
    };
    
    istream& operator>>(istream& is,Foo& f)
    {
        is>>f.s;
        return is;
    }
    
    int main()
    {
        Foo f;
        cin>>f;
        f.show();
        system("PAUSE");
        return 0;
    }

## 函数参数的自动隐式类型转换只能一次
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class X
    {
    public:
        X(int xx):x(xx){}
        int x;
    };
    
    class Y
    {
    public:
        Y(X xx):y(xx.x){}
        int y;
    };
    
    void f(Y){}
    
    int main()
    {
    //======case 1======
        X x(2);
        f(x); //ok
        f(X(2));//ok
    //======case 2======
        f(2);  //error:conversion from int' to non-scalar type Y' requested 
        system("PAUSE");
        return 0;
    }

## 默认构造函数时可以使用的初始化类成员的类实例定义方式
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
    public:
    //    Foo(int aa=0,int bb=0,int cc=0):a(aa),b(bb),c(cc){} 
        int a,b,c;
    };
    
    int main()
    {
        Foo a={1,2,3};//只在无构造函数时可用
        return 0;
    }
    

## 函数内定义类实例，返回实例是否析构的问题
 运行的结果会很有趣的。
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(){cout<<"construct"<<endl;}
        ~Foo(){cout<<"destruct"<<endl;}
        Foo(const Foo& other){cout<<"copy"<<endl;}
    };
    
    int func1()
    {
        Foo f;
        return 0;
    }
    
    Foo func2()
    {
        Foo f;
        return f;
    }
    
    Foo& func3()
    {
        Foo f;
        return f;
    }
    
    int main()
    {
        func1(); //构建，析构 
        cout<<endl;
        int i = func1(); //编译器相关 
        cout<<endl;
        func2(); //构建，析构 
        cout<<endl;
        Foo f2 = func2(); //编译器相关 
        cout<<endl;
        func3(); //构建，析构 
        cout<<endl;
        Foo f3 = func3(); //编译器相关 
        system("PAUSE");
        return 0;
    }

## 类内和类外进行运算符重载的方法
    #include <cstdlib>
    #include <iostream>
    
    class comp
    {
    public:
        double re,im;
    public:
        comp(double r=0.0,double i=0.0):re(r),im(i){}
        comp operator+(comp);
        void printObj(){std::cout<<"re="<<re<<"im="<<im<<"\n";}
    };
    
    comp comp::operator+(comp b)
    {
        comp ret;
        ret.re = re + b.re;
        ret.im = im + b.im;
        return ret;
    }
    
    int main()
    {
        comp x(1.0,2.0);
        comp y(3.0,4.0);
        comp z;
        x.printObj();
        y.printObj();
        z = x+y;
        z.printObj();
        system("PAUSE");
        return 0;
    }
    
以上是类成员声明的运算符重载
    #include <cstdlib>
    #include <iostream>
    
    class comp
    {
    public:
        double re,im;
    public:
        comp(double r=0.0,double i=0.0):re(r),im(i){}
        void printObj(){std::cout<<"re="<<re<<"im="<<im<<"\n";}
    };
    
    comp operator+(comp a,comp b)
    {
        comp ret;
        ret.re = a.re + b.re;
        ret.im = a.im + b.im;
        return ret;
    }
    
    int main()
    {
        comp x(1.0,2.0);
        comp y(3.0,4.0);
        comp z;
        x.printObj();
        y.printObj();
        z = x+y;
        z.printObj();
        system("PAUSE");
        return 0;
    }

以上是函数声明的运算符重载

## 全局变量首次运行时初始化的包装
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    int& use_count()
    {
        static int uc = 0;
        return uc;
    }
    
    int main()
    {
        cout<<++use_count()<<endl;
        cout<<++use_count()<<endl;
        cout<<++use_count()<<endl;
        system("PAUSE");
        return 0;
    }

## 重载new函数 实现定址分配类
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int){};
        void* operator new(size_t,void* p){return p;}
    };
    
    
    int main()
    {
        void* buf = reinterpret_cast<void*>(0xF00F);
        Foo* p2 = new(buf)Foo(3);
        printf("%d\n",p2);
        system("PAUSE");
        return 0; 
    }

## 类成员类构造函数调用顺序
按照类成员声明顺序，非构造函数冒号后的顺序
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Bar1
    {
    public:
        Bar1(){cout<<"Bar1 Construct"<<endl;}
    };
    
    class Bar2
    {
    public:
        Bar2(){cout<<"Bar2 Construct"<<endl;}
    };
    
    class Foo
    {
    public:
        Foo():b2(),b1(){}
    private:
        Bar1 b1;
        Bar2 b2;
    };
    
    int main()
    {
        Foo f;
        system("PAUSE");
        return 0;
    }

## 有const对象的类 不能使用默认构造函数
    #include <cstdlib>
    #include <iostream>
    
    class Foo
    {
    public:
        Foo(int aa):a(aa){}
        const int a;
    };
    
    int main()
    {
        Foo f(3);
        return 0;
    }

## 保持const member function，实现修改成员变量的3种方法
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int n):val(n),changed(false){}
        int getVal()const
        {
            Foo *fp = const_cast<Foo*>(this);
            fp->changed=true;
            //changed=true; //error: in read-only structure
            return val;
        };
        bool isVisit()const{return changed;}
    private:
        int val;
        bool changed;
    };
    
    int main()
    {
        Foo f(10);
        cout<<f.isVisit()<<endl;
        cout<<f.getVal()<<endl;
        cout<<f.isVisit()<<endl;
        system("PAUSE");
        return 0;
    }
    

一种更好的方法是使用mutable关键字，表示即使在const情况下，仍然能被修改
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int n):val(n),changed(false){}
        int getVal()const
        {
            changed=true; //OK
            return val;
        };
        bool isVisit()const{return changed;}
    private:
        int val;
        mutable bool changed;
    };
    
    int main()
    {
        Foo f(10);
        cout<<f.isVisit()<<endl;
        cout<<f.getVal()<<endl;
        cout<<f.isVisit()<<endl;
        system("PAUSE");
        return 0;
    }

如果一个类中有很多需要被修改，可以单独作为一个成员类
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Bar
    {
    public:
        Bar():changed(false),val_count(0){}
        bool changed;
        int val_count;
    };
    
    class Foo
    {
    public:
        Foo(int n):val(n),b(new Bar()){}
        int getVal()const
        {
            b->changed=true; //ok
            b->val_count++;
            return val;
        };
        bool isVisit()const{return b->changed;}
        int getValCount()const{return b->val_count;}
    private:
        int val;
        Bar* b;
    };
    
    int main()
    {
        Foo f(10);
        cout<<f.isVisit()<<endl;
        cout<<f.getValCount()<<endl;
        
        cout<<f.getVal()<<endl;
        cout<<f.isVisit()<<endl;
        cout<<f.getValCount()<<endl;
    
        cout<<f.getVal()<<endl;
        cout<<f.isVisit()<<endl;
        cout<<f.getValCount()<<endl;
    
        system("PAUSE");
        return 0;
    }


特别注意上例中，指针b的初始化的写法，其实，在构造函数冒号后的member(val)相当于member=val

## 很有挑战性的题目（不断更新）
1. The second initialization below fails to compile. What can we infer about the definition of vector?  

1 vector\<int\> v1(42); //ok:42 elements, each 0
2 vector\<int\> v2 = 42; //error:what does this error tell us about vector?

2. Given this program:
    #include <iostream>
    
    int main()
    {
        std::cout<<"Hello, world!\n"<<endl;
        return 0; 
    }
modify it to produce this output:
Initialize
Hello,world!
Clean up
Do not change main() in any way.

## 拷贝函数 传引用时不调用拷贝函数 传值时调用拷贝函数
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int ii):i(ii){}
        Foo(const Foo &other){cout<<"c"<<endl;i=other.i;}
    private:
        int i;
    };
    
    void Bar1(Foo &f)
    {
    }
    
    int Bar2(Foo f)
    {
    }
    
    int main()
    {
        Foo f(9);
        cout<<"Bar1"<<endl;
        Bar1(f);
        cout<<"Bar2"<<endl;
        Bar2(f); //隐式拷贝 
    
        system("PAUSE");
        return 0;
    }

## 静态变量 静态函数 静态变量作为默认函数参数
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    
    class Foo
    {
    public:
        Foo(int ii):i(ii){}
        int getInt(){return i;}
    private:
        int i;
    };
    
    class Bar
    {
    public:
        static int FooVal(){c++; return f.getInt();}
        int callsFooVal(){return c;}
        int setValb(int bb=c){b=bb;return 0;} //可变的默认值 
        int show(){return b;}
    private:
        int b;
        static int c;
        static Foo f;
    };
    
    int Bar::c=0;
    Foo Bar::f(9);
    
    int main()
    {
        Bar b;
        cout<<Bar::FooVal()<<endl; 
        cout<<b.callsFooVal()<<endl;   
        cout<<b.setValb()<<endl;
        cout<<b.show()<<endl;
        cout<<"---------"<<endl;
        cout<<Bar::FooVal()<<endl; 
        cout<<b.callsFooVal()<<endl;
        cout<<b.setValb()<<endl;
        cout<<b.show()<<endl;  
        cout<<"---------"<<endl;
        cout<<Bar::FooVal()<<endl; 
        cout<<b.callsFooVal()<<endl; 
        cout<<b.setValb()<<endl;
        cout<<b.show()<<endl;
        cout<<"---------"<<endl;
        system("PAUSE");
        return 0;
    }

## `c++`定义类时的隐式拷贝函数和隐式构造函数
 显式就是调用构造，copy函数，隐式就是系统帮你调用，搜新婆啊。
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        explicit Foo(int ii):i(ii){}
    public:
        Foo(const Foo &other){i=other.i;}
    private:
        int i;
    public:
        int show(){return i;}
    };
    
    int main()
    {
        Foo tem(20);
        Foo f = tem;
        cout<<f.show()<<endl;
        system("PAUSE");
        return 0;
    }
    
显式构造，隐式copy，可运行。
    #include <cstdlib>
    #include <iostream>
    using namespace std;
    
    class Foo
    {
    public:
        explicit Foo(int ii):i(ii){}
    public:
        explicit Foo(const Foo &other){i=other.i;}
    private:
        int i;
    public:
        int show(){return i;}
    };
    
    int main()
    {
        Foo f = 20;
        cout<<f.show()<<endl;
        system("PAUSE");
        return 0;
    }
显式构造，显式拷贝，不能运行，需要都去除explicit关键字

