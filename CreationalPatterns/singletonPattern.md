In this pattern we ensure that the object is created only once for a given class and exposed by a function in the class.

Things to remember
```
Private Constructor
Remove Ability to use copy constructor/Copy Assignment operator
Provide Static methods to retrieve object/instance
Mutex to lock shared resource
```
Code Example

```cpp

#include<iostream>
using namespace std;

class GlobalCoffeeConfig{
  private:
    static GlobalCoffeeConfig *p;
    GlobalCoffeeConfig(){}
  public:
   static GlobalCoffeeConfig* getConfigInstance(){
     
      if(p==NULL){
        p = new GlobalCoffeeConfig();
      }
     
       return p;
   }
   GlobalCoffeeConfig(const GlobalCoffeeConfig&) = delete;
   GlobalCoffeeConfig& operator= (const GlobalCoffeeConfig&) = delete;
  void showConfig(){
    std::cout << "We are in coffee class"<<std::endl;
  }
};


GlobalCoffeeConfig * GlobalCoffeeConfig::p = NULL;

int main(){
  GlobalCoffeeConfig* t  = GlobalCoffeeConfig::getConfigInstance();
    t->showConfig();
}
```
