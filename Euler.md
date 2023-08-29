**欧拉通路**：若图中存在一条通路，包括了图中的所有边 $\rightarrow$ 图中仅有0或2的点的度数为奇数
**欧拉回路**： 若该欧拉路径是一条回路 $\rightarrow$ 图中各个点的度数为偶数

>[!有向图]
**欧拉通路** $\Leftrightarrow$ 除了起点(出度比入度大1)，重点(入度比出度大1)，其它点的出度均等于入度
**欧拉回路** $\Leftrightarrow$ 图上每个点的出度等于入度

> [!无向图]
**欧拉通路** $\Leftrightarrow$ 图中度为奇数的点等于0或2
**欧拉回路** $\Leftrightarrow$ 图上每个点的度等于偶数


``` cpp
//有向图中查找欧拉路径的最小字典序
#include<bits/stdc++.h>
using namespace std;
typedef pair<int,int> pii; 
const int N=100100,M=500100;
int n,m,cnt,st;
int din[N],dout[N];
int idx,h[N],e[M],ne[M];
int used[M];
vector<pii> edg,ans;
vector<int> ver[N];
void add(int a,int b)
{
    e[idx]=b,ne[idx]=h[a],h[a]=idx++;
}
void dfs(int u)//判断每条边能不能都走一次
{
    for(int &i=h[u];~i;)
    {
        int j=e[i],k;
        k=i;
        if(used[i])
        {
            i=ne[i];
            continue;
        }
        used[i]=1;
        i=ne[i];
        dfs(j);
        ans.push_back({edg[k]});
    }
}
int main()
{
    memset(h,-1,sizeof h);
    scanf("%d%d",&n,&m);
    for(int i=1;i<=m;i++)
    {
        int a,b;
        scanf("%d%d",&a,&b);
        ver[a].push_back(b);
        dout[a]++,din[b]++;
    }
    int a=0;
    for(int i=1;i<=n;i++)
    {
        if((din[i]+dout[i])%2)  
        {
            a++;
            if((dout[i]-din[i])==1) st=i;
        }
    }
    if(a!=0&&a!=2)
    {
        printf("No");
        return 0;
    }
    for(int i=1;i<=n;i++)   sort(ver[i].begin(),ver[i].end(),greater<int>());
    for(int i=1;i<=n;i++)
    {
        for(auto it:ver[i])
        {
            add(i,it);
            edg.push_back({i,it});
        }
    }
    if(a==0)
    {
        for(int i=1;i<=n;i++)
        {
            if(~h[i])
            {
                dfs(i);
                break;
            }
        }
    }
    else 
    {
        dfs(st);
    }
    if(ans.size()==m)  
    {
        printf("%d ",ans[ans.size()-1].first);
        for(int i=ans.size()-1;i>=0;i--)    printf("%d ",ans[i].second);        
    }
    else printf("No");
    return 0;
}

```