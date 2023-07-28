# C++类模板

```c++
template <class 类型参数1, class 类型参数2, ...>
class 类模板名{
    成员函数和成员变量
};

template <typename 类型参数1, typename 类型参数2, ...>
class 类模板名{
    成员函数和成员变量
};
```

**类模板中的成员函数放到类模板定义外时的语法如下:**

```c++
template<class 类型参数1, class 类型参数2, ...>
返回值类型 类模板名<类型参数名列表>:: 成员函数名(参数表){
...
}
```

用类模板定义对象的写法如下：

```c++
类模板名<实际类型参数表> 对象名(构造函数实际参数表);
```

如果类模板有无参构造函数，那么也可以使用如下写法：

```c++
类模板名 <实际类型参数表> 对象名;
```

利用模板类来求两个相同类型变量的最大值和最小值

```c++
template <class DataType>
class Compare
{
public:
    Compare(DataType x, DataType y){
        this->x = x, this->y = y;
    };  
    DataType max(){
        return x > y ? x : y;
    }
    DataType min(){
        return x < y ? x : y;
    }
private:
    DataType x, y;
};

int main()
{
    Compare<int> c1(1, 2);
    cout << "max: " << c1.max() << endl;
    cout << "min: " << c1.min() << endl;
    
    Compare<float> c2(1.1, 2.2);
    cout << "max: " << c2.max() << endl;
    cout << "min: " << c2.min() << endl;
    return 0;
}
```

使用类模板构建一个Pair类

```c++
#include <iostream>
#include <string>
using namespace std;

template <class T1,class T2>
class Pair
{
public:
    T1 key;    //关键字
    T2 value;  //值
    Pair(T1 k,T2 v):key(k),value(v) { };
    bool operator < (const Pair<T1,T2> & p){
        return key < p.key;
    }
};

int main()
{
    Pair<string,int> s1("Tom",19); //实例化出一个类 Pair<string,int>
    Pair<string,int> s2("Leon",23);
    if (s1 < s2)
        cout << s2.key << " " << s2.value;
    else
        cout << s1.key << " " << s1.value;
    return 0;
}
```





在 PCL 中，PointCloud 的类原型定义可以在 `pcl/point_cloud.h` 头文件中找到。以下是 `pcl::PointCloud` 的类原型定义：

```c++
template <typename PointT>
class PointCloud
{
public:
  typedef boost::shared_ptr<PointCloud<PointT> > Ptr;
  typedef boost::shared_ptr<const PointCloud<PointT> > ConstPtr;

  /** \brief 构造函数。将点云设置为空。 */
  PointCloud ();

  /** \brief 拷贝构造函数。
    * \param[in] cloud 要复制到本点云的点云对象
    */
  PointCloud (const PointCloud &cloud);

  /** \brief 赋值操作符重载。
    * \param[in] cloud 要赋值到本点云的点云对象
    */
  PointCloud&
  operator = (const PointCloud &cloud);

  /** \brief 构造函数。调整点云大小并将每个点的默认值设置为 NaN。
    * \param[in] n 要分配的点的数量
    */
  PointCloud (std::size_t n);

  /** \brief 构造函数。调整点云大小并为每个点分配初始值。
    * \param[in] n 要分配的点的数量
    * \param[in] value 要设置的每个点的值
    */
  PointCloud (std::size_t n, const PointT& value);

  /** \brief 构造函数。将点云设置为已有的 PointT 对象向量。
    * \param[in] points 要复制到本点云的 PointT 对象向量
    */
  PointCloud (const std::vector<PointT> &points);

  /** \brief 析构函数。 */
  virtual ~PointCloud () {}

  /** \brief 检查点云是否为空（即 points_ 为空）。 */
  inline bool
  empty () const
  {
    return (points_.empty ());
  }

  /** \brief 预留足够的内存以容纳给定数量的点。 */
  inline void
  reserve (std::size_t n)
  {
    points_.reserve (n);
  }

  /** \brief 调整点云大小为给定的点数量。每个点的默认值将设置为 NaN。
    * \param[in] n 要分配的点的数量
    */
  void
  resize (std::size_t n);

  /** \brief 调整点云大小并为每个点分配初始值。
    * \param[in] n 要分配的点的数量
    * \param[in] value 要设置的每个点的值
    */
  void
  resize (std::size_t n, const PointT& value);

  /** \brief 与另一个点云交换数据。
    * \param[in,out] rhs 要与本点云交换的点云对象
    */
  inline void
  swap (PointCloud<PointT> &rhs)
  {
    points_.swap (rhs.points_);
  }

  /** \brief 向点云添加一个新的点。
    * \param[in] pt 要添加的新点
    */
  inline void
  push_back (const PointT& pt)
  {
    points_.push_back (pt);
  }

  /** \brief 移除点云的最后一个点。 */
  inline void
  pop_back ()
  {
    points_.pop_back ();
  }

  // ... 一系列其他成员函数 ...

  /** \brief 点数据数组。 */
  std::vector<PointT> points;
};
```

上面是 `pcl::PointCloud` 的部分定义，它是一个模板类，模板参数是 `PointT`，表示点云中的点类型。`pcl::PointCloud` 通过使用 STL 容器 `std::vector` 来存储点云数据。PointT 可以是任何用户自定义的数据类型，例如 `pcl::PointXYZ`、`pcl::PointXYZRGB` 等等。

> `pcl::PointCloud` 类的成员和函数的作用如下：
>
> **成员变量：**
>
> - `Ptr`: `Ptr` 是 `boost::shared_ptr<PointCloud<PointT>>` 的别名。它是一个智能指针，用于管理 `pcl::PointCloud` 对象的生命周期，并实现了共享所有权语义。这意味着可以有多个 `Ptr` 指向同一个 `pcl::PointCloud` 对象，当最后一个 `Ptr` 超出范围时，`pcl::PointCloud` 对象将被自动销毁。
> - `ConstPtr`: `ConstPtr` 是 `boost::shared_ptr<const PointCloud<PointT>>` 的别名。它也是一个智能指针，用于管理指向常量 `pcl::PointCloud` 对象的生命周期。这意味着不能通过 `ConstPtr` 修改指向的点云数据，只能读取其中的数据。
>
> - `std::vector<PointT> points`: 这是存储点云数据的主要成员变量。它是一个 `std::vector`，用于存储 `PointT` 类型的点云数据。`PointT` 是一个模板参数，可以是用户定义的点类型，例如 `pcl::PointXYZ`、`pcl::PointXYZRGB` 等。
>
> **成员函数：**
>
> - `PointCloud()`: 默认构造函数，用于创建一个空的点云对象。
> - `PointCloud(const PointCloud &cloud)`: 复制构造函数，用于从另一个点云对象复制数据来创建新的点云对象。
> - `operator = (const PointCloud &cloud)`: 赋值操作符重载，用于将一个点云对象的数据赋值给另一个点云对象。
> - `PointCloud(std::size_t n)`: 构造函数，调整点云大小为指定的点数量，每个点的默认值设置为 NaN。
> - `PointCloud(std::size_t n, const PointT& value)`: 构造函数，调整点云大小为指定的点数量，并为每个点分配初始值。
> - `PointCloud(const std::vector<PointT> &points)`: 构造函数，将点云设置为一个已有的 PointT 对象向量。
> - `virtual ~PointCloud()`: 虚析构函数，用于正确释放内存。
> - `bool empty() const`: 检查点云是否为空，即 `points_` 是否为空。
> - `void reserve(std::size_t n)`: 预留足够的内存以容纳给定数量的点。
> - `void resize(std::size_t n)`: 调整点云大小为给定的点数量，每个点的默认值设置为 NaN。
> - `void resize(std::size_t n, const PointT& value)`: 调整点云大小为给定的点数量，并为每个点分配初始值。
> - `void swap(PointCloud<PointT> &rhs)`: 与另一个点云交换数据。
> - `void push_back(const PointT& pt)`: 向点云添加一个新的点。
> - `void pop_back()`: 移除点云的最后一个点。
>
> 这些成员和函数允许你创建、修改和操作点云数据。你可以使用它们来实例化点云对象、添加或删除点，以及对点云进行各种操作。



