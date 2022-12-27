# Fast input

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
inline int read(){
    int n=0,f=1,ch=getchar_unlocked();
    while(ch<'0'||ch>'9'){
    	if(ch=='-')f=-1;
    	ch=getchar_unlocked();
    }
    while(ch>='0'&&ch<='9'){
    	n=n*10+ch-'0';
    	ch=getchar_unlocked();
    }
    return n*f;
}
vector<int>v[1000005];
int now[1000005];
signed main(){
    int t,n,m;
    t=read();
    for(int i=1; i<=t; i++){
    	n=read();
    	m=read();
        ...
    }
    return 0;
}
```
