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

class GlobalCoffeeConfig{
  private:
    
    GlobalCoffeeConfig(){}
  public:
   GlobalCoffeeConfig(GlobalCoffeeConfig const &) = delete;
   GlobalCoffeeConfig & operator = GlobalCoffeeConfig const & = delete;
   static GlobalCoffeeConfig &getConfigInstance(){
      static GlobalCoffeeConfig p;
      if(p==nullptr){
        p = new GlobalCoffeeConfig;
      }
     else
       return p;
   }
  void showConfig(){
    std::cout << "We are in coffee class"<<std::endl;
  }
};


int main(){
  GlobalCoffeeConfig& t  = GlobalCoffeeConfig::getConfigInstance();
    t.showConfig();
}

```
