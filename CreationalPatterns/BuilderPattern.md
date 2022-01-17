Allow Customization for object creation
Provides declarative and step by step implementation for creating complex object
Coffee -> ingredient[milk] -> ingredient[sugar]

Code Example
```cpp

#include<iostream>

//end product
class Plane{
  private:
    string _planeType;
    string _body;
    string _engine;
  public:
    Plane(string PlaneType) : _planeType(PlaneType){}
   
    //setter
    void setEngine(string engine){_engine = engine;}
    void setBody(string body){_body = body;}
    
    //getter
    string getEngine(string engine){return _engine;}
    string getBody(string body){return _body;}
    
    //show method
    void show(){
      cout<<_planeType << " has " << _body << " with " << _engine << endl; 
    }  
};


//abstract plane builder
class PlaneBuilder{
  protected:
    Plane *_plane;
  public:
    virtual void getPartsDone() = 0 ;
    virtual void buildBody() = 0;
    virtual void buildEngine() = 0;
    Plane *getPlane() { return _plane;} 
};

//concrete child class 1
class propellerBuilder : public PlaneBuilder{
  public:
    void getPartsDone(){
      _plane = new Plane("Propeller Builder");
    }
    void buildBody(){
      _plane->setBody("Propeller Body");
    }
    void buildEngine(){
     _plane->setEngine("Propeller Engine");
    }
};

//concrete child class 2
class JetBuilder : public PlaneBuilder{
  public:
    void getPartsDone(){
      _plane = new Plane("Jet Builder");
    }
    void buildBody(){
      _plane->setBody("Jet Body");
    }
    void buildEngine(){
     _plane->setEngine("Jet engine");
    }
};

class director{
  public:
    //method to create plane using builder
    Plane *createplane(PlaneBuilder *builder){
      builder->getPartsDone();
      builder->buildBody();
      builder->buildEngine();
      return builder->getPlane();
    }
};

int main(){
  Director dir;
  JetBuilder jb;
  PropellerBuilder pb;
  // jbp -> jetbuilder plane, pbp -> propeller builder plane
  Plane *jbp = dir.createPlane(jb);
  Plane *pbp = dir.createPlane(pb);
  jbp->show();
  pbp->show();
  
}


```
