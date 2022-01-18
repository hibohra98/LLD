Strategy pattern as the name suggests encapsulates the strategy which we want to use at the runtime. It dynamically choose the strategy which clients want to use at the runtime.

Eg: Users choosing the sorting strategy[algo] at runtime, StockMarket protfolio's stock recommendation based on user's risk appetite at runtime.

Code Example:

```cpp
 

#include<iostream>
#include<vector>
#include<memory>
using namespace std;

class sortStrategy{
  protected:
  vector<int> elements;

  public:
  sortStrategy(vector<int> &itemList): elements(itemList){}

  virtual void sortf() = 0;
  void print(){
    for(int i=0;i<elements.size();i++){
      cout<<elements[i]<<" ";
    }
    cout<<endl;
  }
};

class quickSortStrategy : public sortStrategy{
  public:
  quickSortStrategy(vector<int> &itemList):sortStrategy(itemList){}
  void sortf(){
    sortf(elements,0,elements.size());
  }
  private:
int quickSortPartition(vector<int> &a,int startIndex, int endIndex){
    int i=startIndex-1;
    int pivot = endIndex;
    for(int j=startIndex;j<endIndex;j++){
        if(a[j]<a[pivot]){
            i++;
            swap(a[i],a[j]);
        }
    }
    i++;
    swap(a[i],a[pivot]);
    return i;
}

void sortf(vector<int>& a, int startIndex, int endIndex){
    if(startIndex>endIndex)
        return;
    int p = quickSortPartition(a,startIndex,endIndex);
    sortf(a,startIndex,p-1);
    sortf(a,p+1,endIndex);

}
};

class bubbleSortStrategy : public sortStrategy{
 public:
  bubbleSortStrategy(vector<int> &itemList):sortStrategy(itemList){}
  void sortf(){
        for(int i=0;i<elements.size();i++){
        for(int j=0;j<elements.size()-i-1;j++){
            if(elements[j]>elements[j+1])
                swap(elements[j],elements[j+1]);
        }
    }
  }
};

int main(){
  int n;
  cin>>n;
  vector<int> a;
  for(int i=0;i<n;i++){
      int l;cin>>l;
      a.push_back(l);
  }
  cout << "Choose the strategy using which you want to sort the array\n1. Bubble Sort\n2. Quick Sort\n";
  int choice;
  cin>>choice;
  sortStrategy *sortClient =nullptr;
  switch(choice){
  case 1:{
    sortClient= new bubbleSortStrategy(a);
    sortClient->sortf();
    sortClient->print();
    break;
  }

  case 2:{
    sortClient = new quickSortStrategy(a);
    sortClient->sortf();
    sortClient->print();
    break;
  }
  default:
    cout<<"No related strategy found"<<endl;
    break;
  }
  return 0;

}


```
