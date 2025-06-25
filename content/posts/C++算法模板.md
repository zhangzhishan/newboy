---
title: C++算法模板（下）
tags: jobs
categories: 校招
date: 11 Jan 2018 10:50 PM
---

# 最短路问题：

## Flored-warshall 算法(n^3)

```cpp	
	  for(int k=1;k<=n;k++)
	
	          for(int i=1;i<=n;i++)
	
	            for(int j=1;j<=n;j++)
	
	          if((dis[i][j]>dis[i][k]+dis[k][j]) && (i!=k) && (i!=j) && (k!=j))
	
	          {
	
	                  dis[i][j]=dis[i][k]+dis[k][j];
	
	                  pre[i][j]=pre[k][j];
	
	          }
```	
输出路径函数：
在调用处加上：`cout<<s<<” ”;   //s为起点；`

```cpp
	void print (int x)
	
	 {
	
	         if(pre[s][x]==0) return ;
	
	         print(pre[s][x]);
	
	         cout<<"->"<<x;
	
	 }
```
## dijkstra算法(n^2)
无优化，用了现写的快读搞定。洛谷的模板题P3371.用cin流最后一个点超时，scanf和快读都是可以过的。当数据规模比较大的时候用scanf好咯。

```cpp
	#include<iostream>
	
	#include<cstdio>
	
	#include<cstring>
	
	#include<cstdlib>
	
	#include<cmath>
	
	#include<algorithm>
	
	#define maxn 10010
	
	#define maxm 500010
	
	using namespace std;
	
	int num,minl,k,n,m,s;
	
	int v[maxn],dis[maxn],head[maxn];
	
	int read()
	
	{
	
	int out=0,fh=1;
	
	char cc=getchar();
	
	if (cc=='-')fh=-1;
	
	while(cc>'9'||cc<'0')cc=getchar();
	
	while(cc>='0'&&cc<='9'){out=out*10+cc-'0';cc=getchar();}
	
	return out*fh;}
	
	/*void write(int x)
	
	{
	
	if(x==0){
	
	putchar('0');
	
	return;}
	
	int num=0;char c[15];
	
	while(x)c[++num]=(x%10)+48,x/=10;
	
	while(num) putchar(c[num--]);
	
	putchar(' ');}  */
	
	struct Edge
	
	{
	
	int next;
	
	int to;
	
	int dis;
	
	}edge[maxm];
	
	void add_edge (int u,int v,int w)
	
	{
	
	num++;
	
	edge[num].next=head[u];
	
	edge[num].to=v;
	
	edge[num].dis=w;
	
	head[u]=num;
	
	}
	
	void dijstra (int v0)
	
	{
	
	for(int i=1;i<=n;i++)
	
	 dis[i]=2147483647;
	
	dis[v0]=0;
	
	for(int i=1;i<=n;i++)
	
	{
	
	           minl=2147483647;
	
	           k=0;
	
	           for(int j=1;j<=n;j++)
	
	           {
	
	                    if(v[j]==0 && minl>dis[j])
	
	                    {
	
	                             minl=dis[j];
	
	                             k=j;
	
	                    }
	
	           }
	
	           v[k]=1;
	
	           for(int i=head[k];i!=0;i=edge[i].next)
	
	           {
	
	                    if(v[edge[i].to]==0 && minl+edge[i].dis<dis[edge[i].to])
	
	                    dis[edge[i].to]=minl+edge[i].dis;
	
	           }
	
	 }
	
	}
	
	int main()
	
	{
	
	cin>>n>>m>>s;
	
	for(int i=1;i<=m;i++)
	
	{
	
	           int u,v,w;
	
	           u=read();
	
	           v=read();
	
	           w=read();
	
	           add_edge(u,v,w);
	
	}
	
	dijstra(s);
	
	for(int i=1;i<=n;i++)
	
	cout<<dis[i]<<" ";
	
	return 0;
	
	 }
```

## Dijkstra(堆优化):
实测516ms; 堆优化是指在寻找最近点时,用堆log时间复杂度取点,用priority_queue(堆/优先队列)实现;
较朴素算法,利用了堆,能更快取得最近点;

```cpp
	#include<cstdio>
	
	#include<iostream>
	
	#include<algorithm>
	
	#include<queue>
	
	using namespace std;
	
	const int INF=2147483647;
	
	const int maxn=10000+10;
	
	const int maxm=500000+10;
	
	int n,m,s;
	
	int fir[maxn],nxt[maxm],to[maxm],val[maxm],cnt;
	
	void add_edge(int u,int v,int w)
	
	{
	
	    nxt[++cnt]=fir[u];fir[u]=cnt;to[cnt]=v;val[cnt]=w;
	
	}
	
	struct Node {
	
	    int d,id;
	
	    Node(){}
	
	    Node(int d,int id):d(d),id(id){}
	
	    bool operator < (const Node& rhs) const {
	
	        return d>rhs.d;//重载<,方便堆
	
	    }
	
	};
	
	int dis[maxn],vis[maxn];
	
	void Dijkstra(int s)
	
	{
	
	    for(int i=1;i<=n;i++) dis[i]=INF; dis[s]=0;
	
	    priority_queue<Node>Q;
	
	    Q.push(Node(0,s));
	
	    while(!Q.empty()) {
	
	        Node u=Q.top(); Q.pop();
	
	        if(vis[u.id]) continue;//若某个点已经被更新到最优,就不用再次更新其他点
	
	        vis[u.id]=1;
	
	        for(int e=fir[u.id];e;e=nxt[e]) {
	
	            int v=to[e],w=val[e];
	
	            if(u.d+w<dis[v]) {
	
	                dis[v]=u.d+w;
	
	                Q.push(Node(dis[v],v));
	
	            }
	
	        }
	
	    }
	
	}
	
	int main()
	
	{
	
	    scanf("%d%d%d",&n,&m,&s);
	
	    for(int u,v,w,i=0;i<m;i++) {
	
	        scanf("%d%d%d",&u,&v,&w);
	
	        add_edge(u,v,w);
	
	    }
	
	    Dijkstra(s);
	
	    for(int i=1;i<=n;i++) printf("%d ",dis[i]);
	
	    return 0;
	
	}
```
## SPFA(无优化):
	
766ms;
	
耗时主要原因是可能某个能将更多点尽可能优化的,却放进了队尾;

```cpp
	#include<cstdio>
	
	#include<iostream>
	
	#include<algorithm>
	
	#include<queue>
	
	using namespace std;
	
	const int INF=2147483647;
	
	const int maxn=10000+10;
	
	const int maxm=500000+10;
	
	int n,m,s;
	
	int fir[maxn],nxt[maxm],to[maxm],val[maxm],cnt;
	
	void add_edge(int u,int v,int w)
	
	{
	
	    nxt[++cnt]=fir[u];fir[u]=cnt;to[cnt]=v;val[cnt]=w;
	
	}
	
	int dis[maxn],inq[maxn];
	
	void SPFA(int s)
	
	{
	
	    for(int i=1;i<=n;i++) dis[i]=INF; dis[s]=0;
	
	    queue<int>Q;Q.push(s);
	
	    while(!Q.empty()) {
	
	        int u=Q.front(); Q.pop();
	
	        for(int e=fir[u];e;e=nxt[e]) {
	
	            int v=to[e],w=val[e];
	
	            if(dis[u]+w<dis[v]) {
	
	                dis[v]=dis[u]+w;
	
	                if(!inq[v]) Q.push(v);
	
	            }
	
	        }
	
	    }
	
	}
	
	int main()
	
	{
	
	    scanf("%d%d%d",&n,&m,&s);
	
	    for(int u,v,w,i=0;i<m;i++) {
	
	        scanf("%d%d%d",&u,&v,&w);
	
	        add_edge(u,v,w);
	
	    }
	
	    SPFA(s);
	
	    for(int i=1;i<=n;i++) printf("%d ",dis[i]);
	
	    return 0;
	
	}
```
## SPFA(SLF优化):
实测497ms; SLF优化是指,当前进队的dis值与队首的dis值比较,若<=进队首,否则进队尾,用deque(双向队列)实现;

```cpp
	#include<cstdio>
	
	#include<iostream>
	
	#include<algorithm>
	
	#include<queue>
	
	using namespace std;
	
	const int INF=2147483647;
	
	const int maxn=10000+10;
	
	const int maxm=500000+10;
	
	int n,m,s;
	
	int fir[maxn],nxt[maxm],to[maxm],val[maxm],cnt;
	
	void add_edge(int u,int v,int w)
	
	{
	
	    nxt[++cnt]=fir[u];fir[u]=cnt;to[cnt]=v;val[cnt]=w;
	
	}
	
	int dis[maxn],inq[maxn];
	
	void SPFA(int s)
	
	{
	
	    for(int i=1;i<=n;i++) dis[i]=INF; dis[s]=0;
	
	    deque<int>Q;
	
	    Q.push_front(s);
	
	    while(!Q.empty()) {
	
	        int u=Q.front(); Q.pop_front(); inq[u]=0;
	
	        for(int e=fir[u];e;e=nxt[e]) {
	
	            int v=to[e],w=val[e];
	
	            if(dis[u]+w<dis[v]) {
	
	                dis[v]=dis[u]+w;
	
	                if(!inq[v]) {
	
	                    if(Q.empty()||dis[v]<=dis[Q.front()]) Q.push_front(v);
	
	                    else Q.push_back(v);
	
	                    inq[v]=1;
	
	                }
	
	            }
	
	        }
	
	    }
	
	}
	
	int main()
	
	{
	
	    scanf("%d%d%d",&n,&m,&s);
	
	    for(int u,v,w,i=0;i<m;i++) {
	
	        scanf("%d%d%d",&u,&v,&w);
	
	        add_edge(u,v,w);
	
	    }
	
	    SPFA(s);
	
	    for(int i=1;i<=n;i++) printf("%d ",dis[i]);
	
	    return 0;
	
	}
```
总结: 对于此题而言,时间效率:SPFA(ELF优化)>Dijkstra(堆优)>SPFA>Dijkstra;
实际上SPFA的时间复杂度不够稳定,有些时候易被出题人卡常数,建议使用更稳定的Dijkstra;
# 最小环问题
	
ans为最小环的最优值：

```cpp
	for(int k=1;k<=n;k++)
	
	           for(int i=1;i<k;i++)
	
	            for(int j=i+1;j<k;i++)
	
	           ans=min(ans,dis[i][k]+dis[k][j]+dis[i][j]);
	
	          for(int i=1;i<=n;i++)
	
	            for(int j=1;j<=n;j++)
	
	              dis[i][j]=min(dis[i][j],dis[i][k]+dis[k][j]);
```
# 并查集
	
## “并”：

```cpp
void unionn (int x,int y)
	
	{
	
	         x=find(x);
	
	         y=find(y);
	
	         if(x!=y) 
	
	         pre[x]=y; 
	
	}
```
## “查”：

```cpp
int find (int x)
	
	{
	
	         if(pre[x]!=x)                  //路径压缩算法，如果不需要可以这样写
	
	           pre[x]=find(pre[x]);       //if(x!=y) return find(pre[x]);
	
	         return pre[x];               //else return x;
	
	}
```
## 集

```cpp
bool judge(int x,int y)  //判断是否连通，可以直接写在代码里不用另写函数。
	
	{
	
	         x=find(x);             
	
	         y=find(y);
	
	         if(x==y) return true;
	
	         else return false;
	
	 }
```
# MST(最小生成树)
## prim算法

```cpp
	 #include<iostream>
	
	#include<cstring>
	
	#define maxn 5010
	
	#define maxm 200010
	
	using namespace std;
	
	int map[maxn][maxn];
	
	int e[maxm];
	
	int v[maxn];
	
	int n,x,y,w,m;
	
	int main()
	
	{
	
	memset(e,0x7f,sizeof(e));
	
	cin>>n>>m;
	
	for(int i=1;i<=m;i++)
	
	{
	
	           cin>>x>>y>>w;
	
	           map[x][y]=map[y][x]=w;
	
	}
	
	e[1]=0;
	
	for(int i=1;i<=n;i++)
	
	{
	
	           int k=0;
	
	           for(int j=1;j<=n;j++)
	
	            if((v[j]==0) && (e[j]<e[k]))
	
	              k=j;
	
	           v[k]=1;
	
	           for(int j=1;j<=n;j++)
	
	            if((v[j]==0) && (map[k][j]<e[j]))
	
	              e[j]=map[k][j];
	
	}
	
	int sum=0;
	
	for(int i=1;i<=n;i++)
	
	 sum+=e[i];
	
	cout<<sum<<endl;
	
	return 0;
	
	}
	
	2.      kruskal算法
	
	#include<iostream>
	
	#include<algorithm>
	
	#define maxn 5010
	
	#define maxm 200010
	
	using namespace std;
	
	int n,k,m,sum,pre[maxn];
	
	int find (int x);
	
	struct point
	
	{
	
	         int x;
	
	         int y;
	
	         int v;
	
	}a[maxm];
	
	void unionn (int x,int y)
	
	{
	
	         x=find(x);
	
	         y=find(y);
	
	         if(x!=y) 
	
	         pre[x]=y; 
	
	}
	
	int find (int x)
	
	{
	
	         if(pre[x]!=x)          
	
	           pre[x]=find(pre[x]);  
	
	         return pre[x];     
	
	}
	
	int cmp(const point &a,const point &b)
	
	{
	
	         if(a.v<b.v)return 1;
	
	         else return 0;
	
	}
	
	int main()
	
	{
	
	         cin>>n>>m;
	
	         for(int i=1;i<=m;i++)
	
	         {
	
	                   cin>>a[i].x;
	
	                   cin>>a[i].y;
	
	                   cin>>a[i].v;
	
	         }
	
	         for(int i=1;i<=n;i++)
	
	           pre[i]=i;
	
	         sort(a+1,a+1+m,cmp);
	
	         for(int i=1;i<=m;i++)
	
	         {
	
	                   if(find(a[i].x)!=find(a[i].y))
	
	                   {
	
	                            unionn(a[i].x,a[i].y);
	
	                            sum+=a[i].v;
	
	                            k++;
	
	                   }
	
	                   if(k==n-1) break;
	
	         }
	
	         cout<<sum<<endl;
	
	         return 0;
	
	}
```
# 拓扑排序

```cpp
	#include<iostream>
	
	using namespace std;
	
	int a[101][101],c[101],r[101],ans[101];
	
	int i,j,tot,temp,num,n,m;
	
	int main()
	
	{
	
	         cin>>n;
	
	         for(int i=1;i<=n;i++)
	
	         {
	
	                   do
	
	                   {
	
	                            cin>>j;
	
	                            if(j!=0)
	
	                            {
	
	                                     c[i]++;
	
	                                     a[i][c[i]]=j;
	
	                                     r[j]++;
	
	                            }
	
	                   }while(j!=0);
	
	         }
	
	         for(int i=1;i<=n;i++)
	
	          if(r[i]==0)
	
	           ans[++tot]=i;
	
	         do
	
	         {
	
	                   temp=ans[tot];
	
	                   cout<<temp<<" ";
	
	                   tot--;
	
	                   num++;
	
	                   for(int i=1;i<=c[temp];i++)
	
	                   {
	
	                            r[a[temp][i]]--;
	
	                            if(r[a[temp][i]]==0)
	
	                             ans[++tot]=a[temp][i];
	
	                   }
	
	         }while(num!=n);
	
	         return 0;
	
	}
```
# 关键路径
## 求出每个点的最早发生的时间，正序求得

```cpp
	Earliest[1…n]=max(Earliest[1…n+map[pre][ 1…n])
```
## 求出每个点最迟发生的时间，逆序求得
	
```cpp
	Last[n…1]=min(Last[n…1]-map[n…1][pre])
```
## 最迟－最早的的余量如果为0，则此点为关键点，项连得路径叫做关键路径。
	
# 数论
	
## 辗转相除法

```cpp
	 a / b = b / a%b；
	
	int gcd (int a,int b)
	
	{
	
	      if(b==0)
	
	      return a;
	
	      else
	
	      gcd(b,a%b);
	
	 }
```
也可以用来求最小公倍数，lcm=a/gcd(a,b)*b。先除后乘的原因是为了避免乘法溢出。
	
## 唯一分解定理：一个数可以分成若干个素数相乘的形式。
	
## 素数定理：π(x)~x/lnx.求出有多少个小于x的素数。
	
## Eratosthenes筛素数

```cpp
	 Int m=sqrt(n+0.5);
	
	 For（int i=1;i<=n;i++）
	
	  If(vis[i]==0)
	
	   For(int j=i*i;j<=n;j++)
	
	Vis[j]=1;
```
还要确定1不是素数，2是素数。如果标记为1则i不为素数，如果未标记则为素数。
	
## 扩展欧几里得算法

```cpp
	 Void gcd (int a,int b,int &d,int &x,int &y)
	
	{
	
	  If(b==0)  {d=a; x=1; y=0;}
	
	  Else { gcd(b,a%b,y,x);
	
	        Y-=x*(a/b);
	
	       }
	
	}
```
求出一组解来，其他部分可写为(x0+kb’,y0-ka’),a’=a/gcd(a,b),b’=b/gcd(a,b);k是整数。
	
G=gcd(g/b)  若ax+bx=c 若c不为g的倍数是，则此方程无整数解。
	
## 幂取模
	
a^n %m 的值：

```cpp
	 Int mod (int a,int b,int m)
	
	{
	
	If(n==0) return 1;
	
	 Int x=mod(a,n/2,m);
	
	 Long long ans=(long long)x*x %m;
	
	If(n%2==1) ans=ans*a % m;
	
	Return (int) ans;
	
	}
```
## 同余与模运算：

```cpp
	  (a+b)%n=((a %n)+(b%n))%n
	
	  (a-b)%n=((a%n)-(b%n)+n)%n
	
	  ab%n=(a%n)(b%n)%n
```
## 杨辉三角：
	
	                      1
	
	                   1    1
	
	                  1   2   1
	
	               1    3    3   1
	
	             1    4   6    4    1
	
	           1   5    10  10   5    1  
	
	         1   6   15    20  15   6    1
	
	
```cpp
	C[i]=c[i+1]*(n-i+1)/i;
	
	C[i]=n!/i!(n-1)!;
```
> [呼呼呼山]()(http://code4fun.me)
> 11 Jan 2018 10:49 PM 


