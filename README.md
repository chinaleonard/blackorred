//玩家编号在数组中和自己的类中都是从0开始的，只在最后输出的时候+1，从1开始 

#include<iostream>
#include<cstring>
#include<vector>
#include <algorithm>
#include<time.h>
#include <cstdlib>
#define random(x) rand()%(x)
#include <unistd.h>
using namespace std;
struct serve
    {
    int leader;//村长位置
	int day=1;//天数 
    int black;//存活黑牌人数 
    int red;//存活红牌人数 
    };
class player
	{private:
			int start;//初始分配序号  
	        bool life=true;//活的还是死的 
	        bool color;//所拿身份牌颜色 
	        bool leader=false;//是否是村长 
	        float ticket=1;//普通人就是一票，村长会波动 
	        float vote=0;//记录被投了多少票 
	public:
	void setstart(int x)
	     {start=x;}//序号分配，从1到n顺序分配 
    int  getstart()
         {return start;}
    void setcolor(int x)
         {if(x%2==1)
		     color=false;//false代表红色 
		  else
		      color=true;
		 return;}//true代表黑色 
	bool getcolor()
	    {return color;} 
    void die()
	     {life=false;
		  if(color==false)
		     cout<<(start+1)<<"号玩家身份是红牌"<<endl;
		  else
		     cout<<(start+1)<<"号玩家身份是黑牌"<<endl;
		 return;       
		 }//调用即死亡 
	bool alive()
	    {return life;}
	float getticket()
	    {return ticket;}
	void voted(float t)
	     {vote+=t;
		 return;}
	void voteclean()
	     {vote=0;}
	float getvote()
	    {return vote;}
	void setticket(float x)
	     {ticket=x;}

	     
		    
	};
void randperm(int Num,int *a)//得到一个长度为n的随机数列（从1到n），不重复 
 {   srand((unsigned)time(NULL)); 
     vector<int> temp;
     for (int i = 0; i < Num; ++i)
     {
         temp.push_back(i + 1);
     }
     random_shuffle(temp.begin(), temp.end());
     for (int i = 0; i < temp.size(); i++)
     a[i]=temp[i];
 }
void clean(int *t,int n,int q)
     {for(int i=0;i<n;i++)
          t[i]=q;
	 }
void clean(float *t,int n,float q)
      {for(int i=0;i<n;i++)
          t[i]=q;
	 }
void speak(int x)
     {cout<<"请"<<(x+1)<<"号玩家发言，发言结束输入任意字符"<<endl;
	  char t;
	  cin>>t;
	  } //发言函数
void bigspeak(serve server,player *p,int n)
     {if(server.leader==-1)
	     {for(int i=0;i<n;i++)
	          if(p[i].alive())
	             speak(i);
		  return;
		  }//如果村长死了，那么就按照顺序发言 
	 int  hand;
	 cout<<"请村长选择左手边还是右手边开始发言,左手请1，右手请输0,搞事情输其他字符就当左手了"<<endl;
	  cin>>hand;
	  int temp;
	  temp=server.leader;//leader在数组中的位置 
	  if(hand!=0)//左边开始大回环,leader-1到最后一位，再从第0位到leader-2； 
	    {if(temp==0)
	        { if(p[n-1].alive()==false)
		        cout<<n<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   
			  speak(n-1);
			  	for(int i=0;i<n-1;i++)
			 if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   
			  speak(i);
			}
		else	
		
		{
		for(int i=temp-1;i<n;i++)
			 {
			 if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   
			  speak(i);
			  } 
	    for(int i=0;i<temp-1;i++)
	        {if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   speak(i);
			  }
	     } 
		}
	  if(hand==0)//右边开始大回环,leader+1到第一位，再从最后一位到leader+2； 
	     {if(temp==n-1)
	        { if(p[0].alive()==false)
		        cout<<1<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   
			  speak(0);
			  	for(int i=n-1;i>0;i--)
			 if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else   
			  speak(i);
			}
		 else
		 
		 {
		 for(int i=temp+1;i>=0;i--)
	          {if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else
			    speak(i);
			  } 
	     for(int i=n-1;i>temp+1;i--)
		 	 {if(p[i].alive()==false)
		        cout<<(i+1)<<"号玩家已经死亡，没得资格说话，下一位"<<endl; 
			  else
			    speak(i);
			  }
	     }
		 }	  
	
	  }
void declare(int x,bool &a,player* p,int n,serve &s)//declare完成一轮投票之后发生的一系列事情 
     //x号玩家，a判断是选村长还是杀人，n是人数 
{if(a==1)
        {
		s.leader=x;
		cout<<"投票结果已经产生，村长是"<<(x+1)<<"号玩家,新王登基，万国朝宗"<<endl;
		if(n%2==0)
        {p[x].setticket(1.5);
		  cout<<"村长的票是1.5票 "<<endl;}
        else
           {p[x].setticket(0.5);
		    cout<<"村长的票是0.5票 "<<endl;}//不论如何，在一轮的投票完成后，村长票数重置	 
       
		return ;} 
	    
 else
   {cout<<"投票结果已产生，"<<(x+1)<<"号玩家英勇牺牲，骨灰撒大海"<<endl;
    p[x].die();
    if(x==s.leader)
       {s.leader=-1;
	   cout<<"大清亡了，哈哈哈哈哈"<<endl;
	   }//重置村长，使得新的一天会选村长 
    if(p[x].getcolor()==false)
      s.red--;
   if(p[x].getcolor()==true)
      s.black--;
      return;
   }

}
void littlevote(int x,int n,int *list,player *p)//返回的是x号（数组位置）玩家投票给的那个玩家的编号 ,list记录可以被投票的人的名单 
    {char t;
	bool ok=false;
	int m;
	for(;;)
	    {int temp=0;
		cout<<"请"<<(x+1)<<"号玩家投票,投票给：";
	    cin>>t;
	    temp=(int)t-48;
		 if(temp<0||temp>30)
	      { cin.clear();
	      cin.sync();
		  cout<<"请重新输入"<<endl;
		  continue; 
	    temp--;
	    cout<<endl; 
	   
		  }
	    if(temp==x)
	       {cout<<"不能投给自己，请输入正确玩家编号"<<endl; 
	       continue;}//投票给自己的重新投 
	       
	  int k=0;
	     for(; ;k++)
	        {if(temp==list[k]&&k<n)
	            {ok=true;
				break;}
	         if(k>=n)
	            {break;}
			 continue; //这个人要在被投票名单里 
			} 
		if(k>=n) 
		   {cout<<"投票错误，请输入正确玩家编号"<<endl; 
	       continue;}
	     if(ok)
		 {break;  
		 m=temp;
		 }
	    }
	    p[m].voted( p[x].getticket() );
	}//投票函数，并保证投票的有效性  
	
void againvote1speak(int &temp1,int *list,player* p,serve &s,int n)//temp1是同票人数，temp2是同票名单 
    {
    float *votet=new float[n]; 
    clean(votet,n,0);    
	cout<<"不幸的是，有并列最高票的成员,分别为："<<endl;
	for(int i=0;i<temp1;i++)
	    {cout<<( list[i]+1 )<<"号玩家 "; }//输出当时还能被投票的玩家名单 
	cout<<endl; 
	for(int i=0;i<temp1;i++)
       speak( list[i] );//进入决赛圈的玩家发言 
	for(int i=0;i<n;i++)
       {if(p[i].alive())
          {littlevote(i,n,list,p);}
	   }
	clean(list,n,-1);
	 int kk=0;//变量kk=0,供后续使用 
    for(int i=0;i<n;i++)
       {votet[i]=p[i].getvote();}
     temp1=0;  
     float max=votet[0];
     for(int i=0;i<n;i++)    
         {if(votet[i]>max)
           max=votet[i];}//统计最大值 
     for(int i=0;i<n;i++)
        {if(votet[i]==max)
          {temp1++;
           list[kk]=i;//是玩家在数组中的位置 ，不是玩家编号！！！！ 
           kk++;//遍历得到新的list作为最高票小分队，进入此分队成员被送往下一轮投票 
	      }
	    }

	delete votet;
	}


void mainvote( int n,player *p,serve &s,bool a)//a表示投票选村长还是踢人出局,a==true选村长，a==false杀人 
     { int k=0;
	 int list[n];
     clean(list,n,-1);
	for(int i=0;i<n;i++)
	   {if(p[i].alive())
	       {list[k]=i;
	        k++;
		   }
	   }
	   //建立list名单，这个名单存储在能被投票的范围里的人
	 k=0;//k置0.供下一步使用 
     for(int i=0;i<n;i++)
        { if(p[i].alive()==true) 
		    littlevote(i,n,list,p);//i号玩家投票(如果还活着的话），调用小投票函数，直接将票数加到每个玩家对象的vote数据成员中；注意，vote每偷完一次票都要clean  
	    }
	 float vote[n];
	 clean(vote,n,0);
	 for(int i=0;i<n;i++)
	    {vote[i]=p[i].getvote();}//调出每个人头上的票数，进行分析 
     float max=vote[0];
     int temp1=0;//temp1记录最高票数重复数量
     clean(list,n,-1); 
     
	 for(int i=0;i<n;i++)    
        if(vote[i]>max)
     max=vote[i];//统计最大值
     
     for(int i=0;i<n;i++)
         if(vote[i]==max)
            {temp1++;
             list[k]=i;//是玩家在数组中的位置 ，不是玩家编号！！！！ 
             k++;//遍历得到新的list，作为最高票小分队，进入此分队成员被送往下一轮投票 
	         }

	 k=0;
     for(;;)
       {if(temp1==1)
	      {if(a==true) 
	         {declare(list[0],a,p,n,s);}
	       else 
	         {declare(list[0],a,p,n,s);}//投票可能导致的一系列事件处理
	         
		   break;}
	    else //temp1还没变成1，就不停再次投票 
		    {againvote1speak(temp1,list,p,s,n);
			}
	   }
	   for(int i=0;i<n;i++)
	       p[i].voteclean();
		   
	 }
void nightvote(int rorb,int n,player* p,serve a)
     {if(n%2==0&&p[ a.leader].getticket()==2.5)
         p[a.leader].setticket(1.5);
      if(n%2==1&&p[ a.leader].getticket()==1.5)
          p[a.leader].setticket(0.5);
	 int redeye=0;
	 int blackeye=0; 
	  if(p[a.leader].alive()==true) 
	     {
		   for(int i=0;i<n;i++)
             {if(p[i].alive())
                {cout<<(i+1)<<"号玩家请决定是否睁眼,0为拒绝睁眼，1为睁眼" <<endl;
			     bool tt;
			     cin>>tt;
			     if(tt==true)
			       {if(p[i].getcolor())
			          blackeye++;
			        else
			          redeye++;
				   } 
			    }
	        }
	    
		 if(rorb==0&&redeye%2==0)
		   {if(n%2==0)
		       p[a.leader].setticket(2.5);
		    else
		       p[a.leader].setticket(1.5);
		    cout<<"睁眼的红牌人数为偶数，村长加一票"<<endl; 
		   }
		 if(rorb==1&&blackeye%2==0)
		    {if(n%2==0)
		       p[a.leader].setticket(2.5);
		    else
		       p[a.leader].setticket(1.5);
		    cout<<"睁眼的红牌人数为偶数，村长加一票"<<endl; 
		    }
	     
	     cout<<p[ a.leader].getticket()<<endl;
		 }///////////////////////////
	 else  
	    cout<<"村长在白天投票过程中已死亡，大家睡个好觉"<<endl; 
	 }
 

int judge(int b,int r,serve s)//b->black;r->red
     {
	 if(b==0)
	    {cout<<"红队赢得了最后的胜利"<<endl;
		 return 1;}
	 if(r==0)
	   {cout<<"黑队赢得了最后的胜利"<<endl;
		 return 1;}
	 if(b==1&&r==1&&s.leader==-1)
	    {cout<<"双方各剩一人，平局"<<endl;
		return 1;
		}
	 return 0; 
	 }//每回合判断最终胜负情况,并返回值来供下一步最终结算
	 

int main()
{int n;
cout<<"请输入玩家个数:"; 
cin>>n;
player p[n];//建立player对象数组

 
serve server;
server.black=n/2;
server.red=n-server.black;//红黑存活人数初始化
server.leader=0;//当leader的值为0时，因为不存在0号玩家，所以当做村长死亡的象征 
        
int *a=new int [n];
randperm(n,a);//创建随机数组 
for(int i=0;i<n;i++)
{p[i].setstart(i);
p[i].setcolor(a[i]);  
}
delete a;
         //for(int i=1;i<n+1;i++)
        //  cout<<p[i].getstart()<<"  "<<p[i].getcolor()<<endl;
 //验证分配编号和颜色，已成功 ，
 //至此系统初始化完成 
for(int i=0;i<n;i++)
    if(p[i].getcolor()==false)
       cout<<(i+1)<<"号玩家的身份是红牌"<<endl;
	else
	   cout<<(i+1)<<"号玩家的身份是黑牌"<<endl;
	   
cout<<"游戏正式开始"<<endl;
server.leader=-1;

while(1)
     {cout<<"第"<<server.day<<"天，真是个做水果蛋糕的好日子"<<endl;
       server.day++; 
       if(server.leader==-1)
	     {cout<<"现在开始选村长"<<endl;
		 bigspeak(server,p,n);
		 mainvote (n,p,server,true);
		 }
	   else
	     {cout<<"今天必须有一个人要死"<<endl;
		 bigspeak(server,p,n); 
		 mainvote(n,p,server,false);
		 }
	   int decide;
	   decide=judge(server.black,server.red,server);
	   if(decide==1)
	      {break;} 
	      
	   cout<<"夜晚即将到来"<<endl; 
	   
	   int rorb; 
        srand((int)time(0));  // 产生随机种子
        rorb=random(2);
    
        if(rorb==0)
           cout<<"今晚是红夜"<<endl;
		else
		   cout<<"今晚是黑夜"<<endl; 
        nightvote(rorb,n,p,server);
        
        
		 
	 }


return 0;
}
	
		
		
		

