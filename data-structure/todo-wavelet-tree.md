---
description: Bugged code
---

# \[TODO] Wavelet tree

Wavelet trees can help us generally 2 sorts of queries.

The following code answers this query :

« Number of elements in subarray L … R less than or equal to x. »

But wavelet trees can also answer kth smallest number in the range of L … R. And we can do basic update : « swap ith element with i+1th element.

The concept : Instead of having ranges as index like in segment trees, we have values. In my implementation : wavelet has for each node a vector of pairs who gives information (index, value). We will just go down in the correct beaches and when we arrive at a step where our interval of (MinValue, MaxValue) has MaxValue <= x, then we do lower\_bound on index R and L and stop exploring this branch.

One can think this as a variant of segment tree. \


```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5;
vector<vector<pair<int,int>>> wavelet(4*N+5);
vector<int> init(N);

// construct wavelet

void buildWavelet(int idx, int left, int right){
	if (left == right) return;
	int mid = (left+right)>>1;
	for (int iElem=0; iElem<wavelet[idx].size(); ++iElem){
		if (wavelet[idx][iElem].second <= mid){
			wavelet[2*idx].emplace_back(wavelet[idx][iElem]);
		}
		else{
			wavelet[2*idx+1].emplace_back(wavelet[idx+1][iElem]);
		}
	}
	buildWavelet(2*idx, left, mid);
	buildWavelet(2*idx+1, mid+1, right);
}

// Number of elements in subarray A[L...R] that are less than or equal to x

int countOccurence(int idx, int left, int right, int L, int R, int x){
	if (left > x) return 0;
	if (right <= x){
		pair<int,int> cible1 = {R,INT_MAX}, cible2 = {L,INT_MIN};
		return (lower_bound(wavelet[idx].begin(),wavelet[idx].end(),cible1)-wavelet[idx].begin()) -
				 (lower_bound(wavelet[idx].begin(),wavelet[idx].end(),cible2)-wavelet[idx].begin());
	}
	int mid = (left+right)>>1;
	return countOccurence(2*idx,left,mid,L,R,x) +
			 countOccurence(2*idx+1,mid+1,right,L,R,x);
}

int main(){
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   cout.tie(nullptr);
	int nElements;
	cin >> nElements;
	int MIN_VAL = INT_MAX, MAX_VAL = INT_MIN;
	for (int iElem=0; iElem < nElements; ++iElem){
		cin >> init[iElem];
		MAX_VAL = max(MAX_VAL,init[iElem]);
		MIN_VAL = min(MIN_VAL,init[iElem]);
		wavelet[1].emplace_back(iElem, init[iElem]);
	}
	buildWavelet(1,MIN_VAL,MAX_VAL);
	int nQuery;
	cin >> nQuery;
	for (int iQuery=0; iQuery<nQuery; ++iQuery){
		int left, right, x;
		cin >> left >> right >> x;
		cout << countOccurence(1,MIN_VAL,MAX_VAL,left,right,x) << '\n';
	}
	return 0;
}

```
