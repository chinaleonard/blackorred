#include<iostream>
#include<cstring>
#include<time.h> 
#include<vector>
#include <algorithm>
using namespace std;
void randperm(int Num,int a[])//得到一个长度为n的随机数列（从1到n），不重复 
 {
     vector<int> temp;
     for (int i = 0; i < Num; ++i)
     {
         temp.push_back(i + 1);
     }
     random_shuffle(temp.begin(), temp.end());
     for (int i = 0; i < temp.size(); i++)
     a[i]=temp[i];
 }

class player
	{private:
	        int start;//初始分配序号  
	        bool life=true;//活的还是死的 
	        bool color;//所拿身份牌颜色 
	        bool leader;//是否是村长 
	        int vote;//投票阶段有身上有多少票 
	        int superticket;//村长专属，普通人不用管 
	 public:
	void setstart(int x)
	     {start=x;}//序号分配，从1到n顺序分配 
    int  getstart()
         {return start;}
    void setcolor(int x)
         {if(x%2==1)
		     color=false;
		  else
		      color=true;}//颜色输入
	bool getcolor()
	    {return color;} 
    void die()
	     {life=false;}//调用即死亡 
	void voteto()
	     {vote++;}//投票调用身上票加一 
	void voteclean()
	     {vote=0;}//回合重新开始，vote清零 
	void sticket(int x)
	     {superticket=x;}//村长特票特殊处理 
	     
	};
int main ()
        {int n;
         cin>>n;
         player p[n]; 
         int *a=new int [n];
         randperm(n,a);//创建随机数组 
		 for(int i=1;i<n+1;i++)
         {p[i].setstart(i);
		   p[i].setcolor(a[i]);  
		 }
         //for(int i=1;i<n+1;i++)
         //  cout<<p[i].getstart()<<"  "<<p[i].getcolor()<<endl;
		 //验证分配编号和颜色，已成功 
        } 
