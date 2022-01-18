Observer Pattern is used where we need one to many relationship. It is ideally used when we need to notify all the Observers. It is like a pub-sub model.
When once entity emits the events and all the observers will receive the event.

Code Example:

```cpp

#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

//subject
class Car{

  int position;
  vector<class Observer*> observerList;

  public:
  int getPosition(){
    return position;
  }

  //set pos and notify it to all the available observer in observerList
  void setPosition(int pos){
    position = pos;
    notify();
  }

  // create attach and detach observer methods
  void attach(Observer* obs){

    observerList.push_back(obs);
  }

  void detach(Observer* obs){
    //erase will remove the list from v.erase(a,b) & remove will remove all the occurence of the 'c' remove(a,b,c)
    observerList.erase(std::remove(observerList.begin(),observerList.end(),obs),observerList.end());
  }

  void notify();

};

//Observer Class

class Observer{
  Car *_car;

  //In constructor we will be calling attach for the specific observer type
  public:
  Observer(Car *car){

    _car = car;
    _car->attach(this);

  }
  //update is to print the update
  virtual void update() = 0;
  //we need get_car to retrieve the car's instace which can be used to call the getPosition method
  protected:
    Car* getCar(){
      return _car;
    }

};

 void Car::notify(){
    for(int i=0;i<observerList.size();i++){
      observerList[i]->update();

    }
 }

class LeftObserver : public Observer{
  public:
    LeftObserver(Car *car): Observer(car){
    cout<<"In constructor"<<endl;
    }
    void update(){
      int pos = getCar()->getPosition();
      if(pos<0)
        cout<<"Left Side\n";
    }
};

class RightObserver : public Observer{
  public:
    RightObserver(Car *car): Observer(car){}
    void update(){
      int pos = getCar()->getPosition();
      if(pos>0)
        cout<<"Right Side\n";
    }
};

class MiddleObserver : public Observer{
  public:
    MiddleObserver(Car *car): Observer(car){}
    void update(){
      int pos = getCar()->getPosition();
      if(pos==0)
        cout<<"Car in center\n";
    }
};

int main(){
  Car *car = new Car();

  LeftObserver lo(car);
  MiddleObserver mo(car);
  RightObserver ro(car);

  //dir contains 1,5,3
  //1 -> left, 5 -> middle, 3 -> right, 0 to stop
  //rest all is invalid
  int dir;
  bool breakloop=false;
  do{
   cin>>dir;
  switch(dir){
      //left
    case 1:
      car->setPosition(-1);
      break;
      //right
    case 2:
      car->setPosition(1);
      break;
      //middle
    case 5:
      car->setPosition(0);
      break;
    case 0:
        breakloop=true;
        break;
    default:
      cout<<"Invalid Move\n";
      break;
  }

  } while(breakloop==false);

  cout<<"Program terminated\n";
}


```
