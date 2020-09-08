#include<iostream>
#include<cstring>
#include<time.h> 
#include<vector>
#include <algorithm>
using namespace std;
struct serve
    {bool warpeace//记录现在是选村长还是杀人
	bool borr;//红夜黑夜 
    int leader;//村长位置
	int series=0;//回合数 
    int black;//存活黑牌人数 
    int red;//存活红牌人数 
    };
class player
	{private:
	        int start;//初始分配序号  
	        bool life=true;//活的还是死的 
	        bool color;//所拿身份牌颜色 
	        bool leader;//是否是村长 
	        int vote;//投票阶段有身上有多少票 
	        int ticket;//普通人就是一票，村长会波动 
	        
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
	     {life=false;
		 if(color==false)
		   cout<<start<<"号玩家身份是红牌"<<endl;
		   else
		   cout<<start<<"号玩家身份是黑牌"<<endl; 
		if(leader==1)
		  {cout<<"主宰已经被击杀，下一条大龙即将重生"<<endl; 
		  }
		      
		   }//调用即死亡 
	bool alive()
	    {return life;}
	void voteto(int x)
	     {vote+=x;}//投票调用身上票增加，同时x保证村长的票也能使适用 
	int getvote()
	     {return vote;}
	void voteclean()
	     {vote=0;}//回合重新开始，vote清零 
	void superticket(int x)
	     {ticket=x;}//村长特票特殊处理 
	     
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
     {cout<<"请"<<x<<"号玩家发言，发言结束输入任意字符"<<endl;
	  int temp;
	  cin>>temp; 
	  } //发言函数 
int littlevote(int x,int n, player* &a)
    {for(int temp=0;;)
	    {cout<<"请"<<x<<"号玩家投票，发言结束输入1"<<endl;
	    cin>>temp;
	    if(temp>n||temp<=0||temp==x)
	       {cout<<"投票错误，请输入正确玩家编号"<<endl; 
	       continue;
	       }
	    else
	       {bool temp1;
		   temp1=a[temp].alive();
		   if(temp1==false)
		      {cout<<"该玩家已死亡，请重新投票给一位还活着的玩家"<<endl;
			  continue; 
			  }
		   else
		      return (temp-1);
		   }
	    }//投票函数，并保证投票的有效性  
	}
int judge(int b.int r)//b->black;r->red
     {
	 if(b==0)
	    {cout<<"红队赢得了最后的胜利"<<endl;
		 return 1;}
	 if(r==0)
	   {cout<<"黑队赢得了最后的胜利"<<endl;
		 return 2;}
	 return 0; 
	 }//每回合判断最终胜负情况,并返回值来供下一步最终结算
	 
void declare(int x,bool &a,player* &p,int n)//declare完成一轮投票之后发生的一系列事情 
//x号玩家，a判断是选村长还是杀人，n是人数 
{if(a==0)
        {cout<<"投票结果已经产生，村长是"<<x<<"号玩家,新王登基，万国朝宗"
        if(n%2==0)
          p[x].superticket(1.5);
        else
          p[x].superticket(0.5);	 
        }
   else
   {cout<<"投票结果已产生，"<<x<<"号玩家英勇牺牲，骨灰撒大海"
    p[x].die();
   }
 } 
void mainvote(int n,player* &p,serve s,bool &a)//a表示投票选村长还是踢人出局,a==true选村长，a==false杀人 
     {
     int *temp =new int[n];
     for( int i=0;i<n;i++)
     temp[i]=littlevote(i,n,p);//i号玩家投票，共n个人，并存入被投票玩家对象的记录票数的成员 
     for(int i=0;i<n;i++)   
        p[temp[i]].voteto(1);//存入每个人头上的票数
     int max=p[0].getvote();
     int temp1=0;//temp1记录最高票数重复数量
     int *temp2=new int[n];//temp2记录谁是最高票 
     for(int i=0;i<n;i++)    
        if(p[i].getvote()>max)
     max=p[i].getvote();//统计最大值 
     for(int i=0 ,int k=0;i<n;i++)
        if(p[i].getvote()==max)
          {temp[1]++;
           temp2[k]=i+1;
           k++;//遍历得到temp2作为最高票小分队，进入此分队成员被送往下一轮投票 
	      }
     if(temp1==1)//一轮投票已结束，判断是否要二次投票 
       {
		 if(a==true) 
	        declare(temp2[0],true,p,n);
	     else 
	        declare(temp2[0],false,p,n)//投票可能导致的一系列事件处理 
       }
     else
       while(1)
       {againvote() 
	   }
	  
     
	  
	
	 }  
} 
int main ()
{int n;
cin>>n;

player p[n]; 
serve server; 
        
int *a=new int [n];
randperm(n,a);//创建随机数组 
for(int i=0;i<n;i++)
{p[i].setstart(i);
p[i].setcolor(a[i]);  
}
delete a;
         //for(int i=1;i<n+1;i++)
        //  cout<<p[i].getstart()<<"  "<<p[i].getcolor()<<endl;
		 //验证分配编号和颜色，已成功 ，前期准备工作做完。 

cout<<"游戏正式开始"<<endl<<"第一天，真是个做水果蛋糕的好日子"<<endl;
for(int i=0;i<n;i++)
    {speak(i);
	if(i=n-1)
	 cout<<"发言结束"<<endl;
	 }	
cout<<"开始投票选出第一任村长"<<endl;

}
	
		
		
		
