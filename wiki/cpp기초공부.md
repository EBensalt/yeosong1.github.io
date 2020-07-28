
# 출력 cout 

```C++
#include <iostream>

int main(int argc, char** argv)
{
  std::cout << "Hi world" << std::endl;
  return (0);
}
```

# argv 인자 받아 출력

```C++
#include <iostream>
using namespcae std;

int main (int argc, char** argv)
{
cout << "Have " argc << "argc: " << endl;
int i = -1;
while(++i < argc)
  cout << "argv[" << i << "] : " << argv[i] <<endl;
return (0);
}
```
