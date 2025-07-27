<front size="2">

# 一、基本概念
 ## 1.类型、变量和算术运算
 int  //整数 4字节
 float  //浮点数 
 char  //字符，如‘z’ 1字节
 double  //双精度浮点数 8字节
 bool  //布尔值，可取true或false 1字节

注:=是赋值运算符，==用于等性判断

c++中有多种初始化方式：
```
double d1=2.3;
double d2{2.3};
complex<double>z=1;
complex<double>z2{d1 d2};
complex<double>z3={1,2};  //当使用{...},符号=可选
```
常量在声明中不能不进行初始化，普通变量也应在极有限的情况下不进行初始化。
在声明变量时，如果变量的类型可以由初始化器判断得到，我们无需显示指定其类型：
```
auto b=true;  //变量类型是bool
auto ch='x';  //~~      char
auto d=1.2;  //          float
auto z=sqrt(y); //z的类型是sqrt(y)的返回类型
```
当没有明显理由需要显示指定数据类型时，一般使用auto，明显的理理由是：
1.该定义位于一个大的作用域，需要读者清楚知道其类型
2.希望明确规定某个变量的范围和精度（比如希望使用double而非float）
## 2.常量
两种不变性概念：const和condtexpr
const:不改变这个值，这个变量传入函数时也不会被函数内改变。编译器负责确认并执行const的承诺
constexpr:用于说明常量，作用是允许将数据置于只读内存中（不大可能被破坏）以及提升性能
```
const int dmv=17;   //dmv是一个命名的常量
int var =17;        //var不是常量  
constexpr double max1=1.4*square(dmv);  //如果square(17)是常量表达式，则正确
constexpr double max2=1.4*square(var);  //错误，var不是常量表达式
const double max3=1.4*square(var);      //Ok,可以在运行时求值
double sum(const vector<double>&);      //sum不会更改它的参数的值
vector<double> v {1.2,3.4,4.5};         //v不是常量
const double s1=sum(v);                 //OK，在运行时求值
constexpr double s2=summ(v);            //错误，sum(v)不是常量表达式
```
##  3.检验与循环
switch  case
外面加上while 或者 for
## 4.指针  、数组和循环
元素为char的数组可以声明为；
```
char v[6];    //含有6个字符的数组
char*p;       //指针指向字符
```
[]表示“...的数组” *表示“指向...”。所有数组下标从0开始v包含v[0]到v[5]。指针变量中存放着一个相应类型对象的地址：
```
char*p=&v[3];   //p指的第4个元素
char x=*p；     //*p是p所指的对象
```
前置一元运算符*表示“...的内容”，&表示“...的地址”
一元后置运算符&表示“...的引用”。类似于指针，但无需使用前置运算符*访问所引用的值，同样，一个引用在初始化之后就不能在引用其他对象了。
```
T a[n]；    //T[n]:n个T组成的数组
T*p；       //T*：指向T的指针
T&r；       //T&：T的引用
T f(A);     //T(A):是一个函数，接受A类型的实参，返回T类型的结果
```
当确实没有对象时可以用空指针nullptr，所有指针类型都共享一个nullptr
1.可以使用++将指针移动到数组的下一个元素
2.在for语句中如果不需要初始化操作，则可以省略他
## 5.结构
构建一种新类型的第一步通常是把所需的元素组成一种数据结构。
```
struct Vector{
int sz;        //元素的数量  
double*elem;   //指向元素的指针
}
```
访问struct的成员：1.通过名字或引用，使用点运算符(.)2.通过指针，使用->
```
void f(Vector v,Vector&rv,Vector*pv)
{
    int i1=v.sz;   //名字访问
    int i2=rv.sz;  //引用
    int i3=pv->sz; //指针
}
```
## 6.类
把类型的接口（所有代码都可以使用的部分）与其实现（对其他不可访问的数据具有访问权限）分离出来。
public成员定义为该类的接口，private成员只能通过接口访问
注：c++语言处理可变数量信息的一项基本技术：一个固定大小的句柄指向位于”别处“的一组可变数据
与所属类同名的函数称为构造函数，用来构造类的对象的函数。其作用是初始化类的对象，定义一个构造函数可以解决类变量为初始化的问题
## 7.枚举
除了类以外的另一种简单的用户定义类型（规模较小）
```
enum class Color{red,blue green};
enum class Traffic_light{green,yellow,red};
Color col=Color::red;
Traffic_light light=Traffic_light::red
```
enum后面的class指明了枚举是强类型的，且他的枚举值位于指定的作用域中。不同的enum class是不同的类型，防止对常量的意外使用。例如上述不能混用Traffic_light和color的值
```
Color x=red;                 //错误，哪个red？
Color y=Traffic_light::red;  //错，这个red不是color的对象
Color z=Color::red;          //ok
```
若不想显示的限定枚举值的名字，并希望其是int（无需显式转换），则应该去掉class而得到一个普通的enum
## 8.模块化
c++中使用声明来描述接口，声明制定了某个函数或某种类型所需要的所有内容。
## 9.分离编译
用户代码只能看见所用类型和函数声明，它们的定义则分别放在分离的源文件里
一般把描述模块接口的声明放置在一个特定的文件中，文件名通常指示模块的预期用途，这段声明被放到头文件（header file）里，用户将其包含（include）进程序以访问接口，为了帮助编译器确保一致性，负责提供函数实现部分的cpp文件同样应包含提供的接口.h文件
## 10.名字空间
namespace一方面表达某些声明是属于一个整体的，另一方面表它们的名字不会与其他名字空间的名字冲突
访问其他名字空间的某个名字，在这个名字前面加上名字空间作为限定(std::cout和My_code::main),要想获取标准库名字空间名字的访问权，应该使用using指示
名字空间便于将独立开发的组件组装成一个程序
## 11.错误处理（未懂）
无路任何时候时候只要试图定义一个函数，就应该前置条件是什么以及检验该条件的的过程是否简洁
不变式：假定某事为真的声明称为类的不变式
静态断言：static_assert能用于任何可以表达为常量表达式的东西
```
static_assert(speed<c,"can't go too fast")  //错误，速度必须是常量换成local_max即可
```
# 二、抽象机制
## 1.类
### a.具体类型
1. 表现形式是定义的一部分，表现形式是一个或几个指向保存在别处的数据的指针，但这种表现现形式出现在具体类型的每一个对象中
类的表现形式一般是私有的，只有通过成员函数进行访问。表现形式发生变换需要重新编译
2. 容器：包含若干元素的对象
为了防止c++的垃圾回收的接口不可用以，行使回收功能或处于逻辑考虑需要更精准的资源释放控制。需要与一种机制确保构造函数斯分配的空间一定会被摧毁，于是有了析构函数
```
class Vector{
  private:
    double* elem;   //elem指向一个包含sz个double的数组
    int sz;
  public:
    Vector(int s):elem{new double[s]},sz{s}  //析构函数：请求资源
    {
        for (int i=0;i!=s,++i) elem[i]=0;    //初始化元素素
    }
    ~Vector(){delete[] elem; }               //析沟函数，释放资源
    double& operator[](int i);
    int size() const;                  
};
```
析构函数命名即一个求补运算符～后加类名，含义上来说是构造函数的补充
例子中：Vector的构造函数使用new运算符从自由储存分配一些内存空间，析构函数通过delete运算符释放该空间达到清理资源的目的
构造函数负责分配元素空间并正确初始化Vector，析构函数负责释放空间。避免在普通代码中分配内存存，而是把分配操作隐藏在行为良好的抽象的实现内部，避免了裸new裸delete
3. 初始化容器（两种简洁的方式）
a.初始化器列表构造函数：使用元素的列表进行初始化
b.push_back():在序列的末尾添加一个新元素
```
Vector read(istream& is)
{
  Vector v;
  for (double d;is>>d)   //将浮点数读入d
      v.push_back(d);    //把d加到v当中
      return v;
}
```
该循环的的终止条件是遇到文件结尾或者格式错误误，在此之前每个读入的数依次添加到的Vector的尾部
### b.抽象类型
抽象类型则将使用者与类的实现细节完全隔离开。分离接口与表现形式并且放弃了纯局部变量
因为对抽象类型的表现形式一无所知，所以必须从自由储存为对对象分配空间，然后通过引用或指针访问对象。
设计一个Container类设计接口
```
class Container{
  public:
  virtual double& operator[](int)=0;   //纯虚函数
  virtual int size() const=0;          //常量成员函数
  virtual ~Container(){};              //析构函数
};
```
关键字virtual的意思是“可能随后在其派生类中重新定义”————虚函数
=0说明该函数是纯虚函数，意味着Container的派生类必须定义这个函数，因此我们不能单纯定义一个Container的对象，Container只是作为接口出现，它的派生类负责具体实现operator[]()和size().
在Container中没有构造函数，数据不需要初始化，其还含有一个Virtual的析构函数，
