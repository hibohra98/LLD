Adapter Pattern is used when we need to identify and translate method call from one method to another. Convert interface of a class to the interface that client wants.

Eg:
Indian Charger <-- Adapter --> USA Socket
3.5mm HeadPhone Jack <-- Adapter --> Iphone Lightning cable -> Mobile Phone

Code Example:

```cpp


#include<iostream>
#include<memory>
using namespace std;

class IndianSocket{
  public:
    virtual void indianCharger(int socketType) = 0;

};

class USASocket{
  public:
    void USACharger(){
      cout<<"Charging USA plug\n";
    }
};


class GermanSocket{
  public:
    void GermanCharger(){
      cout<<"Charging using German plug\n";
    }
};

class UKSocket{
  public:
    void UKCharger(){
      cout<<"Charging using UK plug\n";
    }
};

class socketAdapter : public IndianSocket, public USASocket, public UKSocket, public GermanSocket{
  public:

    void indianCharger(int socketType){

      switch(socketType){
        case 1:
        UKCharger();
        break;

        case 2:
        USACharger();
        break;

        case 3:
        GermanCharger();
        break;

        default:
        cout<<"Adapter Type Not available at the moment";
        break;
      }
    }
};

int main(){
  unique_ptr<IndianSocket> indianAllInOneSocket = make_unique<socketAdapter>();
  indianAllInOneSocket->indianCharger(1);
  indianAllInOneSocket->indianCharger(2);
  indianAllInOneSocket->indianCharger(3);
  return 0;
}

//Code Needs C++14 and higher for usage for unique_ptr and make_unique 

```
