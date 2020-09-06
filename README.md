#include<iostream>
#include<cstring>
#include<time.h> 
#include<vector>
#include <algorithm>
using namespace std;
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
		     color=false;//false代表红色 
		  else
		      color=true;}//true代表黑色 
	bool getcolor()
	    {return color;} 
    void die()
	     {life=false;}//调用即死亡 
	bool alive()
	    {return<<life;}
	void voteto()
	     {vote++;}//投票调用身上票加一 
	void voteclean()
	     {vote=0;}//回合重新开始，vote清零 
	void sticket(int x)
	     {superticket=x;}//村长特票特殊处理 
	     
	};
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
void speak(int x)
     {}cout<<"请"<<x<<"号玩家发言，发言结束输入任意字符"<<endl
	  int temp;
	  while(cin>>temp) 
	       {return 0;} 
	  } //发言函数 
int vote(int x,int n,class a[n])
    {
     for(int temp=0;; )
	    {cout<<"请"<<x<<"号玩家投票，发言结束输入1"<<endl;
	    cin>>temp;
	    if(temp>n||temp<=0||temp==x)
	       {cout<<"投票错误，请输入正确玩家编号"<<endl; 
	       continue;
	       }
	    else
	       {bool temp1;
		temp1=a[temp].alive;
		   if(temp1==false)
		      {cout<<"该玩家已死亡，请重新投票给一位还活着的玩家"<<endl;
			  continue; 
			  }
		   else
		      return temp;
		   }
	    }//投票函数，并保证投票的有效性  
	}
int judge(int n,bool a[n],bool b[n])//a是生命，b是身份 
     {}int black=0;int red=0
	 for(int i=1;i<=n;i++)
         while(a[i]==true)
               {if(b[i]==true) black++;
                  else red++;
			   }
	 if(black==0)
	    {cout<<"红队赢得了最后的胜利"<<endl;
		 return 1;}
	 if(red==0)
	   {cout<<"黑队赢得了最后的胜利"<<endl;
		 return 2;}
	 return 0; 
	 }//每回合判断最终胜负情况,并返回值来供下一步最终结算   








int main ()
{int n;
cin>>n;
struct server
    {bool borr;//红夜黑夜 
    int leader;//村长位置
	int series=0;//回合数 
    bool psituation[n];//存储玩家当回合情况 
	int dietime [n]; 
    }
player p[n]; 
        
int *a=new int [n];
randperm(n,a);//创建随机数组 
for(int i=1;i<n+1;i++)
{p[i].setstart(i);
p[i].setcolor(a[i]);  
}
delete a;
         //for(int i=1;i<n+1;i++)
        //  cout<<p[i].getstart()<<"  "<<p[i].getcolor()<<endl;
		 //验证分配编号和颜色，已成功 

cout<<"游戏正式开始"<<endl;
cout<<"投票选出第一任村长"<<endl;
int *temp =new int[n]
for( int i=1;i<=n;i++)
   temp[n]=vote(i,n,p);
	     
		
		
		
		} 
        
