# Ford-Fulkerson 202101627 이준한 중간고사 대체과제 보고서  
 ---         
목차
1. Network Flow\
  -용어 정리
3. Ford-Fulkerson algorithm
3. 코드
---


Network Flow
---

시작하는 부분은 source라 하고 도착하는 부분을 sink라 한다.\
이때 source -> sink로 최대한 많은 유량을 흘려보내려면 어떤 방식을 사용할지를 다루는 문제를 network flow 라 한다.
사용되는 용어를 간단히 정리하자면
· 유량(flow) :                  두 정점 사이에서 흐르는 양\
· 용량(capacity) :              두 정점 사이에 최대로 흐를수 있는 양\
· 소스(source) :                유량이 시작되는 정점. 보통 S로 많이 나타낸다\
· 싱크(sink) :                  유량이 도착하는 정점. 보통 T로 많이 나타낸다\
· 증가경로(augmenting path) :   소스에서 싱크로 유량을 보내는 경로\ 
· 잔여용량(residual capacity) : 현재 흐를수 있는 유량. 즉 용량-유량을 의미한다.
정도가 된다\
이때 네트워크 유량은 항상 3가지의 규칙을 만족한다.


---
1. 용량 제한 속성 flow(u,v) <= capacity(u,v) : 간선에 흐르는 유량은 해당 간선의 용량을 초과할 수 없다.
2. 유량의 대칭성  flow(u,v) = -flow(v,u)       : u가 v에게 보낸 유량은 v가 u에게 음의 유량을 보낸 것과 같다.
3. 유량의 보존     ∑flow(u,v) = 0               : 시작점과 도착점을 제외한 정점에서 보낸 유량과 나간 유량은 같다.       
---
이러한 문제를 해결하기 위해서 사용되는 알고리즘은 대표적으로 두 개의 알고리즘이 존재하는데,\
바로 포드-폴커슨 알고리즘(Ford-Fulkerson algorithm)과 디닉 알고리즘(Dinic's Algorithm), 에드몬트 카프(Edmonds-Karp)알고리즘 등이 있다.  
이 중에 포드 폴커슨 알고리즘에 대해 자세히 알아보도록 하겠다. 






Ford-Fulkerson algorithm(포드 풀커슨 알고리즘)
---





이 알고리즘의 동작 원리는 다음과 같다.
포드 폴커슨 알고리즘을 자세히 알아보기 전에 먼저 네트워크 플로우의 진행 방식에 대해 간단히 알아보자면, 

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfMzgg/MDAxNTI4ODc3MTMyODg0.2gf1YpY4Ygu7renBP-l06LGPk07myMGp327x4dAWUgAg.H7mljrE_4CccWYcSLnb6zuaS0O5MLUGUVk65AI_bgIIg.PNG.jh20s/image.png?type=w800)

이 그래프에서 각 간선은 유량/용량을 나타내고 S->T로 진행된다. 

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfODEg/MDAxNTI4ODc3MTI4OTA4.8Vf0f1b-dokYXvwMS5K0OXhRVOYM0oCFp8PsZ_smG9sg.kykjnwANZGSCxzRC5WLYt_eDtPSf5t0Eakkv5iNkgxIg.PNG.jh20s/image.png?type=w800)
위로 2를 흘려 보내고

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfMjE3/MDAxNTI4ODc3MTIyNjgy.KYNE-KkUKkOz-z0YOGBLze3x2fd6MyW8Xnr1QEmxdO4g.a9SkxjJ4NUYM04tl1UoMj5N4ED2vM16I2aPMvRQrCYMg.PNG.jh20s/image.png?type=w800)
아래로 2를 흘려 보냈다면, 전체적으로 S에서 T로 4를 흘려보내 준 것이고 위 그래프에서는 최대 4만큼의 유량을 흘려보내 줄 수 있다고 한다. 

이제 포트 풀커슨 알고리즘의 작동 방식에 대해 자세히 알아보자.\
포드 풀커슨 알고리즘은 최초의 네트워크 유량 알고리즘으로 개념과 구현이 간단하다는 장점이 있다.\
이 알고리즘의 작동 방식은 다음과 같다.\
포드 풀커슨 알고리즘은 DFS기반의 알고리즘으로, 이 DFS를 통해 증가경로를 찾고 찾은 만큼 유량을 흘러보내고, 이 과정을 반복하면서 더 이상 증가경로를 찾을 수 없을 때 까지 실행하면 것이다.\

조금 더 자세하게 설명하자면, 


1. Source에서 SInk로 가는 경로(augmenting path)를 하나 찾는다./ 
   이때 해당 경로는 반드시 여유 용량이 남아 있어야 한다. 즉 c(a, b) - f(a, b) > 0


2.찾아낸 경로에 보낼수 있는 최대 flow을 찾는다. 이때 보낼수 있는 최대 flow는 경로에 남은 capacity의 최소값이다./
   현재 찾아낸 경로에서 보낼수 있는 가능한 최대의 flow 값은, 경로에 남은 capacity의 최소값이 된다. 즉 경로상 Min(c(a, b) - f(a, b))
     
     
3.찾아낸 경로에, 찾아낸 최대 flow을 실제 흘려보낸다.\
   전체 경로에 f(a, b) += flow 을 하게 됩니다.


   e.g) a - b - c - d 와 같은 경로라면, f(a, b) += flow, f(b, c) += flow, f(c, d) += flow 을 해준다.\
   동시에 '유량의 대칭' 조건에 따라서, 역방향 역시 flow을 음수값으로 흘려 보낸다.

  
   e.g) a - b - c - d 와 같은 경로라면, f(b, a) -= flow, f(c, b) -= flow, f(d, c) -= flow 을 해준다.\
        이게 되는 이유는, 가상의 역방향 간선의 capacity는 사실 0 이고, 해당 capacity에 - 값으로 flow(유량)을 흘려도, 용량의 제한 조건을 넘지 않기 때문./
        f(b, a) -= flow 일때, 즉 -1(flow) / 0(capacity) 형태이고, 이는 c(b, a) - f(b, a) > 0, (0 - (-1)) = 1이므로 조건을 만족한다.


1번에 해당하는 경로 찾기가 실패하기 전까지 위 1 ~ 3 번을 반복한다.\
이제 그림으로 알아보자.

![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow1.png)\
이 그래프에서 S->T로 가는 최대 유량을 구해보자



1. 증가경로 찾기\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow2.png)\
S->A->E->T경로를 찾았다.



2. 찾아낸 경로에 보낼 수 있는 최대 flow를 찾는다.\ 
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow3.png)\
경로에서 보낼 수 있는 최대 flow는 각각의 구간에 남은 capacity의 최소값이다.

S → A → E → T 에서 각각의 남은 capacity는 아래와 같다. (남은 capacity => c(a, b) - f(a, b))\
S → A: c(s, a) - f(s, a) = 3 - 0 = 3\
A → E: c(a, e) - f(a, e) = 3 - 0 = 3\
E → T: c(e, t) - f(e, t) = 5 - 0 = 5\
위 3가지 경우에서 min 은 3이다.


3. 찾아낸 경로에 최대 flow를 흘려보낸다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow4.png)\
S->A->E->T 경로에 2번 과정에서 찾은 3을 흘려보낸다.

4. 1~3을 반복한다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow5.png)\
S->B->E->T 경로를 찾고\
S → B → E → T 의 최대 Flow값 == Min(c(a,b) - f(a,b)) 인 2를 찾고\
S → B → E → T 에 2를 실제 흘려 보낸다\
현재 T 까지 도달한 Total유량은 5이다.

4. 1~3을 다시 반복한다. 이때 종료조건은 증가경로를 더 이상 찾지 못 할 때이다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow6.png)\
S → C → F → T 경로를 찾고

S → C → F → T 경로의 최대 Flow값 == Min(c(a,b) - f(a,b)) 인 4를 찾고\
S → C → F → T 경로에 4를 실제 흘려 보낸다\
현재 T 까지 도달한 Total유량은 9 이다

5. 이제 증가 경로를 더 이상 찾을 수 없으므로 T까지 도달한 Total 총량은 9이다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow7.png)\
그러나\
S → A → D → T 로 1

S → A → E → T 로 2

S → B → E → T 로 2

S → C → F → T 로 4

S → C → F → E → T 로 1\

하면 Total 유량이 10 이 나오게 된다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow8.png)\

따라서 정답은 10인데, 1~5까지의 과정과 실제 정답을 비교해 본다면,\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow9.png)\
실제 정답과, 오답을 비교해보면

S → A 로 흐르는 3 이라는 유량이 A → E 로 전부 3으로 흐르는 것이 아니라. A → D로 1 흐르고, A → E 로 2가 흐름을 알 수 있다.\
따라서 A → E → T 가 1만큼의 여유분이 생기고\
해당 여유분 만큼 S → C → F → E → T 을통해 1이 흐르게 된다.\
따라서 1~5의 과정에서 정답을 이끌어 내기 위해서는, 

처음 구한 A → E 유량 3을, 2가 되게 하고.\
남는 유량 1을 A → E → T 의 path로 흐르게 하면 된다.

이를 가능하게 하는 아이디어가 가상의 간선(역간선)이 있다고 가정하는 것이다.\
결국 네트워크 유량의 포드 풀커슨 알고리즘의 핵심은 이 가상의 간선(역간선)의 존재를 이용하는 것이다.

이러한 역간선이 있닥 가정하고 전의 과정을 반복하면,
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow10.png)\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow11.png)\
이때 증가경로는 S->C->F->E->A->D->T이다.
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow-remain-capa-1.png)\
역방향 간선인 E → A는 3이라는 용량을 흘릴 수 있다.

증가경로를 찾았다면, 흘릴수 있는 최대 flow (Min(c(a,b) - f(a,b)) 즉 남은용량중 제일 작은값을 찾는다.\

여기서는 S → C, F → E, A → D 가 1 로 Min 값이고, flow는 1이다.\
증가경로와, 흘릴수 있는 최대 flow값을 구하였으니, 해당 경로에 flow을 흘려 보낸다.\
![](https://gseok.gitbooks.io/algorithm/content/assets/network-flow12.png)\
경로에 flow을 흘릴때, 정방향은 flow을 + 하고, 역방향은 - 하는걸 볼 수 있다.\
f(a, b) += flow\
f(b, a) -= flow\
flow을 정, 역방향을 흘림으로 해서, 기존 A → E가 2로 줄어들고, 1만큼 남은 내용이 A → D → T 로 가게 됨을 볼 수 있다.\
역간선이 있는 상태에서, 1 → 3 다시 반복 해봅시다. 이제 다시 1번 ~ 3번 과정을 반복한다. 종료조건은 1번 (증가경로)를 더이상 찾지 못하는 경우가 된다.\

1번 (증가경로) 를 더이상 찾을 수 없다.

현재까지 방법으로 얻은 답은, T 까지 도달한 **Total 유량은 10 가 된다


코드
---

이 포드 풀커슨 알고리즘을 코드로 구현하면 
<pre><code>
#include<stdio.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<vector>
#include<tuple>
#include<string>
#include<map>
#include<queue>
#define inf 2000000000 
using namespace std;
typedef pair<int , int> Pair;
typedef pair<Pair, int> PPair;

typedef struct edge{
	int to;
	int capacity;
	edge* prev;
	edge(int to, int capacity) : to(to), capacity(capacity) {}
};


vector<int> check;
vector<vector<edge*>>  adj;
int s, t;

void add_edge(int u, int v, int cap) {
	edge* next = new edge(v, cap);
	edge* prev = new edge(u, 0);
	next->prev = prev;
	prev->prev = next;
	adj[u].push_back(next);
	adj[v].push_back(prev);
}

int dfs(int u, int cap) {

	if (check[u]) return 0;
	check[u] = 1;
	if (u == t) return cap;

	for (int i = 0; i < adj[u].size(); i++) {
		if (adj[u][i]->capacity>0) {
			int now_cap = adj[u][i]->capacity;
			if (cap!=0 && now_cap > cap) {//소스가 아닐경우 증가경로중 가장 작은 잔여용량을 찾음
				now_cap = cap;
			}
			int f = dfs(adj[u][i]->to, now_cap);
			if (f) {
				adj[u][i]->capacity -= f;//유량을 흘려보냄
				adj[u][i]->prev->capacity += f; //음수만큼 흘려보냄
				return f;
			}
		}
	}
	return 0;
}

int CtoI(char c) {
	if (c <= 'Z') return c - 'A';
	return c - 'a' + 26;
}

int main(void) {
	int n;
	scanf("%d", &n);
	adj.resize(100);
	check.resize(100);
	for (int i = 0; i < n; i++) {
		char a, b;int c;
		scanf(" %c %c %d", &a, &b, &c);
		a = CtoI(a), b = CtoI(b);
		add_edge(a, b,c);
		add_edge(b, a, c);
	}
	s = CtoI('A'), t = CtoI('Z');
	int ans = 0;
	while (1) {
		int flow;
		fill(check.begin(), check.end(), 0);
		flow = dfs(s, 0);
		if (flow == 0) break;
		ans += flow;
	}
	printf("%d", ans);
}
</code></pre>
이다.

JAVA로도 구현 가능한데, 

<pre><code>


public static Path findPath() {
    // dfs or bfs
    // c(a, b) - f(a, b) > 0 조건에 따라서 path을 찾을 수 있게 함.
}


getMaxTotalFlow() {
    total = 0;
    while( (path = findPath()) != null ) { // 1. 경로 찾기 및 반복
        // 2. flow값 찾기
        FlowVal f;
        for (p in path) {
          FlowVal pathFlow = p.capacity - p.flow;
          f = Math.min(f, pathFlow)
        }


        // 3. flow 흘리기
        for (p in path) {
            path(a,b).flow += f; // 순방향에는 + 로 흘려줌.
            path(b,a).flow -= f; // 역방향에는 - 로 흘려.
        }


        total += flow;
    }


    // total 이 최대 유량이 됨.
    return total;
}

</code></pre>






































































return 0;
