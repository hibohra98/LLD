Factory Method pattern is one of the creational patterns which focuses on the creation of object based on the type we need. 

Code Example:

```cpp

#include<iostream>
#include<memory>
using namespace std;

class coffeeMachine{
  public:
    virtual void brew() = 0;
};

class simpleCoffeeMachine : public coffeeMachine{
  public:
    void brew(){
      cout<<"Brewing in simpleCoffeeMachine"<<endl;
    }
};

class robustCoffeeMachine : public coffeeMachine{
  public:
    void brew(){
      cout<<"Brewing in robustCoffeeMachine"<<endl;
    }
};
//Encapsulates the factory method for coffeeMachine Type
class coffeeMachineFactory{
  public:
  unique_ptr<coffeeMachine> createMachine(int MachineType){
    switch(MachineType){
      case 1:
        return make_unique<simpleCoffeeMachine>();
      case 2:
        return make_unique<robustCoffeeMachine>();
      default:
        return make_unique<simpleCoffeeMachine>();
    }
  }
};

int main(){
    unique_ptr<coffeeMachineFactory> ob = make_unique<coffeeMachineFactory>();
    //coffeeMachineFactory ob = new coffeeMachineFactory();
    unique_ptr<coffeeMachine> simple = ob->createMachine(1);
    unique_ptr<coffeeMachine> robust = ob->createMachine(2);
    simple->brew();
    robust->brew();

}


```
