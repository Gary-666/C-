# C++中的STL中map用法详解(出处：https://blog.csdn.net/u013550000/article/details/80521509)

Map是STL的一个关联容器，它提供一对一（其中第一个可以称为关键字，每个关键字只能在map中出现一次，第二个可能称为该关键字的值）的数据 处理能力，由于这个特性，它完成有可能在我们处理一对一数据的时候，在编程上提供快速通道。这里说下map内部数据的组织，map内部自建一颗红黑树(一 种非严格意义上的平衡二叉树)，这颗树具有对数据自动排序的功能，所以在map内部所有的数据都是有序的，后边我们会见识到有序的好处。

1. **map简介**

   map是一类关联式容器。它的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。
   对于迭代器来说，可以修改实值，而不能修改key。
2. **map的功能**

   自动建立Key － value的对应。key 和 value可以是任意你需要的类型。

   根据key值快速查找记录，查找的复杂度基本是Log(N)，如果有1000个记录，最多查找10次，1,000,000个记录，最多查找20次。

   快速插入Key -Value 记录。

   快速删除记录

   根据Key 修改value记录。

   遍历所有记录。
3. **使用map**
   
   使用map得包含map类所在的头文件

   #include <map>  //注意，STL头文件没有扩展名.h

   map对象是模板类，需要关键字和存储对象两个模板参数：

   std:map<int,string> personnel;

   这样就定义了一个用int作为索引,并拥有相关联的指向string的指针.

   为了使用方便，可以对模板类进行一下类型定义，

   typedef map<int,CString> UDT_MAP_INT_CSTRING;

   UDT_MAP_INT_CSTRING enumMap;
4. **数据插入**


   下面展示三种插入方法
   ```C
   //数据的插入--第一种：用insert函数插入pair数据  
   #include <map>  
  
   #include <string>  
  
   #include <iostream>  
  
   using namespace std;  
  
   int main()  
  
     {  
  
    map<int, string> mapStudent;  
  
    mapStudent.insert(pair<int, string>(1, "student_one"));  
  
    mapStudent.insert(pair<int, string>(2, "student_two"));  
  
    mapStudent.insert(pair<int, string>(3, "student_three"));  
  
    map<int, string>::iterator iter;  
  
    for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
  
       cout<<iter->first<<' '<<iter->second<<endl;  
  
     }  
     
   ```
   ```C++
   //第二种：用insert函数插入value_type数据，下面举例说明  
  
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent.insert(map<int, string>::value_type (1, "student_one"));  
    
        mapStudent.insert(map<int, string>::value_type (2, "student_two"));  
    
        mapStudent.insert(map<int, string>::value_type (3, "student_three"));  
    
        map<int, string>::iterator iter;  
    
        for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
    
        cout<<iter->first<<' '<<iter->second<<endl;  
    
    }  
    
   ```
   ``` C++
    //第三种：用数组方式插入数据，下面举例说明  
    
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent[1] = "student_one";  
    
        mapStudent[2] = "student_two";  
    
        mapStudent[3] = "student_three";  
    
        map<int, string>::iterator iter;  
    
        for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
    
            cout<<iter->first<<' '<<iter->second<<endl;  
    
    }  

   ```
   当然了第一种和第二种在效果上是完成一样的，用insert函数插入数据，在数据的 插入上涉及到集合的唯一性这个概念，即当map中有这个关键字时，insert操作是插入数据不了的，但是用数组方式就不同了，它可以覆盖以前该关键字对 应的值，用程序说明
   ```C++
    //验证插入函数的作用效果  
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        pair<map<int, string>::iterator, bool> Insert_Pair;  
    
        Insert_Pair = mapStudent.insert(pair<int, string>(1, "student_one"));  
    
        if(Insert_Pair.second == true)  
    
            cout<<"Insert Successfully"<<endl;  
    
        else  
    
            cout<<"Insert Failure"<<endl;  
    
        Insert_Pair = mapStudent.insert(pair<int, string>(1, "student_two"));  
    
        if(Insert_Pair.second == true)  
    
            cout<<"Insert Successfully"<<endl;  
    
        else  
    
            cout<<"Insert Failure"<<endl;  
    
        map<int, string>::iterator iter;  
    
        for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
    
        cout<<iter->first<<' '<<iter->second<<endl;  
    
    }  
   ```
   ```C++
    //验证数组形式插入数据的效果  
    
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent[1] = "student_one";  
    
        mapStudent[1] = "student_two";  
    
        mapStudent[2] = "student_three";  
    
        map<int, string>::iterator iter;  
    
        for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
    
        cout<<iter->first<<' '<<iter->second<<endl;  
    }  
    

   ```
5. **map的大小**
   Int size = mapStudent.size();
6. **数据的遍历**
   1. 前向迭代器
   2. 反向迭代器
      ```C++
            //第二种，利用反向迭代器  
        
        #include <map>  
        
        #include <string>  
        
        #include <iostream>  
        
        using namespace std;  
        
        int main()  
        
        {  
        
            map<int, string> mapStudent;  
        
            mapStudent.insert(pair<int, string>(1, "student_one"));  
        
            mapStudent.insert(pair<int, string>(2, "student_two"));  
        
            mapStudent.insert(pair<int, string>(3, "student_three"));  
        
            map<int, string>::reverse_iterator iter;  
        
            for(iter = mapStudent.rbegin(); iter != mapStudent.rend(); iter++)  
        
                cout<<iter->first<<"  "<<iter->second<<endl;  
        
        }  
      ```

      ```C++
                //第三种：用数组方式，程序说明如下  
        
        #include <map>  
        
        #include <string>  
        
        #include <iostream>  
        
        using namespace std;  
        
        int main()  
        
        {  
        
            map<int, string> mapStudent;  
        
            mapStudent.insert(pair<int, string>(1, "student_one"));  
        
            mapStudent.insert(pair<int, string>(2, "student_two"));  
        
            mapStudent.insert(pair<int, string>(3, "student_three"));  
        
            int nSize = mapStudent.size();  
        
        //此处应注意，应该是 for(int nindex = 1; nindex <= nSize; nindex++)  
        //而不是 for(int nindex = 0; nindex < nSize; nindex++)  
        
            for(int nindex = 1; nindex <= nSize; nindex++)  
        
                cout<<mapStudent[nindex]<<endl;  
        
        }
      ```
7. map查找方法
   第一种：用count函数来判定关键字是否出现，其缺点是无法定位数据出现位置,由于map的特性，一对一的映射关系，就决定了count函数的返回值只有两个，要么是0，要么是1，出现的情况，当然是返回1了

   第二种：用find函数来定位数据出现位置，它返回的一个迭代器，当数据出现时，它返回数据所在位置的迭代器，如果map中没有要查找的数据，它返回的迭代器等于end函数返回的迭代器

   ```C++
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent.insert(pair<int, string>(1, "student_one"));  
    
        mapStudent.insert(pair<int, string>(2, "student_two"));  
    
        mapStudent.insert(pair<int, string>(3, "student_three"));  
    
        map<int, string>::iterator iter;  
    
        iter = mapStudent.find(1);  
    
        if(iter != mapStudent.end())  
    
        cout<<"Find, the value is "<<iter->second<<endl;  
    
        else  
    
        cout<<"Do not Find"<<endl;  
        
        return 0;  
    }  
   
   
   ```
    第三种：这个方法用来判定数据是否出现，是显得笨了点，但是，我打算在这里讲解

 

    lower_bound函数用法，这个函数用来返回要查找关键字的下界(是一个迭代器)

    

    upper_bound函数用法，这个函数用来返回要查找关键字的上界(是一个迭代器)

    

    例如：map中已经插入了1，2，3，4的话，如果lower_bound(2)的话，返回的2，而upper-bound（2）的话，返回的就是3

    

    Equal_range函数返回一个pair，pair里面第一个变量是Lower_bound返回的迭代器，pair里面第二个迭代器是Upper_bound返回的迭代器，如果这两个迭代器相等的话，则说明map中不出现这个关键字，

    程序说明

    ```C++
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent[1] = "student_one";  
    
        mapStudent[3] = "student_three";  
    
        mapStudent[5] = "student_five";  
    
        map<int, string>::iterator iter;  
    
        iter = mapStudent.lower_bound(1);  
    
        //返回的是下界1的迭代器  
    
            cout<<iter->second<<endl;  
    
        iter = mapStudent.lower_bound(2);  
    
        //返回的是下界3的迭代器  
    
            cout<<iter->second<<endl;  
    
        iter = mapStudent.lower_bound(3);  
    
        //返回的是下界3的迭代器  
    
            cout<<iter->second<<endl;  
    
        iter = mapStudent.upper_bound(2);  
    
        //返回的是上界3的迭代器  
    
            cout<<iter->second<<endl;  
    
        iter = mapStudent.upper_bound(3);  
    
        //返回的是上界5的迭代器  
    
            cout<<iter->second<<endl;  
    
        pair<map<int, string>::iterator, map<int, string>::iterator> mappair;  
    
        mappair = mapStudent.equal_range(2);  
    
        if(mappair.first == mappair.second)  
    
            cout<<"Do not Find"<<endl;  
    
        else  
    
            cout<<"Find"<<endl;  
    
        mappair = mapStudent.equal_range(3);  
    
        if(mappair.first == mappair.second)  
    
            cout<<"Do not Find"<<endl;  
    
        else  
    
            cout<<"Find"<<endl;  
    
        return 0;  
    }  

    ```
8. 从map中删除元素
    移除某个map中某个条目用erase（）

    该成员方法的定义如下：

    iterator erase（iterator it);//通过一个条目对象删除

    iterator erase（iterator first，iterator last）//删除一个范围

    size_type erase(const Key&key);//通过关键字删除

    clear()就相当于enumMap.erase(enumMap.begin(),enumMap.end());

    这里要用到erase函数，它有三个重载了的函数，下面在例子中详细说明它们的用法

    ```C++
    #include <map>  
    
    #include <string>  
    
    #include <iostream>  
    
    using namespace std;  
    
    int main()  
    
    {  
    
        map<int, string> mapStudent;  
    
        mapStudent.insert(pair<int, string>(1, "student_one"));  
    
        mapStudent.insert(pair<int, string>(2, "student_two"));  
    
        mapStudent.insert(pair<int, string>(3, "student_three"));  
    
            //如果你要演示输出效果，请选择以下的一种，你看到的效果会比较好  
    
        //如果要删除1,用迭代器删除  
    
        map<int, string>::iterator iter;  
    
        iter = mapStudent.find(1);  
    
        mapStudent.erase(iter);  
    
        //如果要删除1，用关键字删除  
    
        int n = mapStudent.erase(1);//如果删除了会返回1，否则返回0  
    
        //用迭代器，成片的删除  
    
        //一下代码把整个map清空  
    
        mapStudent.erase( mapStudent.begin(), mapStudent.end() );  
    
        //成片删除要注意的是，也是STL的特性，删除区间是一个前闭后开的集合  
    
        //自个加上遍历代码，打印输出吧  
    
    }  

    ```
9. map中的swap用法

   
   map中的swap不是一个容器中的元素交换，而是两个容器所有元素的交换

10. 排序 ·  map中的sort问题
    map中的元素是自动按Key升序排序，所以不能对map用sort函数；
 
    这里要讲的是一点比较高深的用法了,排序问题，STL中默认是采用小于号来排序的，以上代码在排序上是不存在任何问题的，因为上面的关键字是int 型，它本身支持小于号运算，在一些特殊情况，比如关键字是一个结构体，涉及到排序就会出现问题，因为它没有小于号操作，insert等函数在编译的时候过 不去，下面给出两个方法解决这个问题。

    第一种：小于号重载，程序举例。

    ```C++
        #include <iostream>  
        #include <string>  
        #include <map>  
        using namespace std;  
        
        typedef struct tagStudentinfo  
        
        {  
        
            int      niD;  
        
            string   strName;  
        
            bool operator < (tagStudentinfo const& _A) const  
        
            {     //这个函数指定排序策略，按niD排序，如果niD相等的话，按strName排序  
        
                    if(niD < _A.niD) return true;  
        
                    if(niD == _A.niD)  
        
                        return strName.compare(_A.strName) < 0;  
        
                return false;  
        
            }  
        
        }Studentinfo, *PStudentinfo; //学生信息  
        
        int main()  
        
        {  
        
            int nSize;   //用学生信息映射分数  
        
            map<Studentinfo, int>mapStudent;  
        
            map<Studentinfo, int>::iterator iter;  
        
            Studentinfo studentinfo;  
        
            studentinfo.niD = 1;  
        
            studentinfo.strName = "student_one";  
        
            mapStudent.insert(pair<Studentinfo, int>(studentinfo, 90));  
        
            studentinfo.niD = 2;  
        
            studentinfo.strName = "student_two";  
        
            mapStudent.insert(pair<Studentinfo, int>(studentinfo, 80));  
        
            for (iter=mapStudent.begin(); iter!=mapStudent.end(); iter++)  
        
                cout<<iter->first.niD<<' '<<iter->first.strName<<' '<<iter->second<<endl;  
        
            return 0;  
        }  

    ```
    ```C++
    //第二种：仿函数的应用，这个时候结构体中没有直接的小于号重载，程序说明  
    
    #include <iostream>  
    
    #include <map>  
    
    #include <string>  
    
    using namespace std;  
    
    typedef struct tagStudentinfo  
    
    {  
    
        int      niD;  
    
        string   strName;  
    
    }Studentinfo, *PStudentinfo; //学生信息  
    
    class sort  
    
    {  
    
    public:  
    
        bool operator() (Studentinfo const &_A, Studentinfo const &_B) const  
    
        {  
    
            if(_A.niD < _B.niD)  
    
                return true;  
    
            if(_A.niD == _B.niD)  
    
                return _A.strName.compare(_B.strName) < 0;  
    
        return false;  
    
        }  
    };  
    
    int main()  
    
    {   //用学生信息映射分数  
    
        map<Studentinfo, int, sort>mapStudent;  
    
        map<Studentinfo, int>::iterator iter;  
    
        Studentinfo studentinfo;  
    
        studentinfo.niD = 1;  
    
        studentinfo.strName = "student_one";  
    
        mapStudent.insert(pair<Studentinfo, int>(studentinfo, 90));  
    
        studentinfo.niD = 2;  
    
        studentinfo.strName = "student_two";  
    
        mapStudent.insert(pair<Studentinfo, int>(studentinfo, 80));  
    
        for (iter=mapStudent.begin(); iter!=mapStudent.end(); iter++)  
    
            cout<<iter->first.niD<<' '<<iter->first.strName<<' '<<iter->second<<endl;  
    }  

    ```
11.  map的基本操作函数：
    C++ maps是一种关联式容器，包含“关键字/值”对

     begin()         返回指向map头部的迭代器

     clear(）        删除所有元素

     count()         返回指定元素出现的次数

     empty()         如果map为空则返回true

     end()           返回指向map末尾的迭代器

     equal_range()   返回特殊条目的迭代器对

     erase()         删除一个元素

     find()          查找一个元素

     get_allocator() 返回map的配置器

     insert()        插入元素

     key_comp()      返回比较元素key的函数

     lower_bound()   返回键值>=给定元素的第一个位置

     max_size()      返回可以容纳的最大元素个数

     rbegin()        返回一个指向map尾部的逆向迭代器

     rend()          返回一个指向map头部的逆向迭代器

     size()          返回map中元素的个数

     swap()           交换两个map

     upper_bound()    返回键值>给定元素的第一个位置

     value_comp()     返回比较元素value的函数