## 'this' Pointer

The 'this' keyword is generally used in two ways in c++,the 2 ways are,

1.To refer an instance of class
2.As a pointer of the class

### Example : 1
```c
#include <iostream>
using namespace std;

class student
{
  private:
  int marks;
  string name;
  public:
  student(string name,int marks)
  {
    this->marks=marks;
    this->name=name;
  }
  void print()
  {
  cout<<"Marks obtained by "<<this->name<<" is "<<this->marks;
  }
};

int main()
{
    student a("ram",91);
    student b("shyam",88);
    a.print();
    return 0;
}
```
### Check Result [here](https://onecompiler.com/cpp/3ykqftgbz)

### Example : 2
```c
#include<iostream>
using namespace std;
  
class Test
{
private:
  int x;
  int y;
public:
  Test(int x = 0, int y = 0) { this->x = x; this->y = y; }
  Test &setX(int a) { x = a; return *this; }
  Test &setY(int b) { y = b; return *this; }
  void print() { cout << "x = " << x << " y = " << y << endl; }
};
  
int main()
{
  Test obj1(5, 5);
  
  // Chained function calls.  All calls modify the same object
  // as the same object is returned by reference
  obj1.setX(10).setY(20);
  
  obj1.print();
  return 0;
}
```
### Check Result [here](https://onecompiler.com/cpp/3ykqgqgac)