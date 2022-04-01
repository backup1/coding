# Universal Matching (Randomization)

Question [https://uoj.ac/problem/79](https://uoj.ac/problem/79)

从前一个和谐的班级，所有人都是搞OI的。有 $$n$$ 个是男生，有 $$0$$ 个是女生。男生编号分别为 $$1,…,n$$。现在老师想把他们分成若干个两人小组写动态仙人掌，一个人负责搬砖另一个人负责吐槽。每个人至多属于一个小组。

有若干个这样的条件：第 $$v$$ 个男生和第 $$u$$ 个男生愿意组成小组。

请问这个班级里最多产生多少个小组？

#### 输入格式

第一行两个正整数，$$n,m$$ 。保证 $$n≥2$$。接下来 $$m$$ 行，每行两个整数 $$v,u$$ 表示第 $$v$$ 个男生和第 $$u$$ 个男生愿意组成小组。保证 $$1≤v,u≤n$$，保证 $$v≠u$$，保证同一个条件不会出现两次。

#### 输出格式

第一行一个整数，表示最多产生多少个小组。接下来一行 $$n$$ 个整数，描述一组最优方案。第 $$v$$ 个整数表示 $$v$$ 号男生所在小组的另一个男生的编号。如果 $$v$$ 号男生没有小组请输出 $$0$$。

Solution : [https://uoj.ac/submission/233938](https://uoj.ac/submission/233938)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=510;
vector<int>to[N];
int lnk[N],vis[N],tim=0;
inline void ae(int u,int v){
	to[u].push_back(v);
}
bool dfs(int x){
	if(x==0)return true;
	vis[x]=tim;
	vector<int>::iterator it=to[x].begin(),ti=to[x].end();
	random_shuffle(it,ti);
	for(int u,v;it!=ti;it++){
		u=*it,v=lnk[u];
		if(vis[v]<tim){
			lnk[x]=u,lnk[u]=x,lnk[v]=0;
			if(dfs(v))return true;
			lnk[u]=v,lnk[v]=u,lnk[x]=0;
		}
	}
	return false;
}
int main(){
	int n,e,ans=0;
	scanf("%d%d",&n,&e);
	for(int u,v;e--;scanf("%d%d",&u,&v),ae(u,v),ae(v,u));
	srand(time(0));
	memset(lnk+1,0,n<<2);
	for(int tot=5;tot--;){
		for(int i=1;i<=n;i++){
			if(!lnk[i]){
				tim++,ans+=dfs(i);
			}
		}
	}
	printf("%d\n",ans);
	for(int i=1;i<=n;i++){
		printf("%d ",lnk[i]);
	}
	putchar('\n');
	return 0;
}
```
