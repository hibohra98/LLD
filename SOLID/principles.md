SOLID principles are Object Oriented Design Principles used to create softwares that the Maintainable, extensible and reusable

S - Single Responsibility - Single Class Should have single Purpose, proper separation of concerns

O - Open Closed - Classes should be open for extension but closed for modification. Easily extensible without changing the code of the class

L - Liskov's Substitution - Behavioral Class Substitution. If Class A is derived from Class B then we should be able to substitute object of B to A

I - Interface Segregation - Favour Multiple specific interfaces over single interface. Clients should not be forced to implement method they dont need[From abstract class].

D - Dependency Inversion - Depend on Abstraction and not implementation

Code Examples:

Single Responsibility
=========================

```

What Not to do:

class CoffeeMachine{
  void pourCoffee(){
    cout<<"Pour Coffee"<<endl;
}

void SendCoffeeMetrics(){
  cout<<"Send metrics"<<endl;
  urlrequest re;
  re.uri("Metrics");
  re.perform(); // implementation not specific to coffee machine
};


Correct way:
class CoffeeMachine{
  void pourCoffee(){
    cout<<"Pour Coffee"<<endl;
}

void SendCoffeeMetrics(){
  cout<<"Send metrics"<<endl;
  coffeeService.sendMetrics(); //<-- Updated method abstracting implementation of sendMetrics as its not related to coffeeMachine class[Direct Dependency]
};

```


Open-Closed
=========================

```

What Not to do:
class coffeeMachine{
  private:
    vector<int> settings = {1,2,3}
    //in case we need more roast setting direct code has to be changed
  public:
    void RoastBySetting(int setting){
      switch(setting):
        case 1:
          cout<<"setting 1"<<endl;
          break;
        case 2:
          cout<<"setting 1"<<endl;
          break;
          //This class is not extensible as it encourages direct code change
    }
};

Correct way:
class coffeeMachine{
  public:
    void RoastBySetting(RoastSetting setting){
        roastService.roast(&setting);
    }
};
class RoastService{
 ... 
};

class RoastSetting{
 ...
};

```

Liskov's substitution
=========================

```
Correct Way:
//Base class
class Roaster{
  virtual void roast();  
};

//concrete child 1
class coffeeRoaster : Roaster {
  public:
    void roast(){
      ...
    }
};

//concrete child 2 
class ExpressoRoaster : Roaster {
  public:
    void roast(){
      ...
    }
};


class x{
  void roast(Roaster roaster){
    roaster.roast(); //<-- at runtime we decide which roast() to call. Here we have a behavioural subtyping. Expressoroaster<-Roaster we can add object of roaster to expressoroaster
  }
};
```


Interface Seggregation
=========================

```
What not to do ?

class machine{
  public:
    virtual void roast();
    virtual void pour();
    virtual void grid();
};

class allInOneCoffeeMachine : public machine{
  public:
    void roast(){...}
    void pour(){...}
    void grid(){...}
};

class simpleCoffeeMachine : public machine{
  public:
    void pour(){...}
    
    //No need for below two methods 
    
    void roast(){...}
    void grid(){...}
};


Correct:
struct roaster{
  virtual void roast();
};

struct Pourer{
  virtual void pour();
};

struct Grinder{
  virtual void grind();
};

struct RobustMachine : Roaster, Pourer, Grinder{};

class allInOneCoffeeMachine : public RobustMachine{
  public:
    void roast(){...}
    void pour(){...}
    void grid(){...}
};

class simpleCoffeeMachine : public Pourer{
  public:
    void pour(){...}
};


```

Dependency Inversion Principle
============================

Laptop <--HDMI CABLE--> Monitor

Help create loosely coupled software
```

What not to do ?
Class CoffeeMachine{
  vector<int> status;
  ...
};

class CoffeeTest{
   void start(coffeeMachine &machine){
      for(auto bit:machine.status){
        ...operate status bit//if coffee machine class change then coffee test will also change in case status varible change to other type like "set"
      }
   }
};

Correct:

struct coffeeStatusReader(){
  virtual vector<int> readStatus();
  //shared abstraction
};

class CoffeeMachine : coffeeStatusReader{
  vector<int> status;
  void readStatus{
    ...
  }
}
//CoffeeTest no longer depend on low level module coffeeStatusReader, we can change the implementation without the change in high level code
class coffeeTest{
  void start(CoffeStatusReader &Reader){
    reader.readStatus();
  } 
};

```


