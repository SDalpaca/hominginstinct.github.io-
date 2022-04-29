# Ford-Fulkerson 202101627 이준한 중간고사 대체과제 보고서  

 ---         
목차
1. Network Flow\
  -용어 정리
2. Ford-Fulkerson algorithm
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


정도가 된다

이때 네트워크 유량은 항상 3가지의 규칙을 만족한다.

---
1. 용량 제한 속성 flow(u,v) <= capacity(u,v) : 간선에 흐르는 유량은 해당 간선의 용량을 초과할 수 없다.

2. 유량의 대칭성  flow(u,v) = -flow(v,u)       : u가 v에게 보낸 유량은 v가 u에게 음의 유량을 보낸 것과 같다.

3. 유량의 보존     ∑flow(u,v) = 0               : 시작점과 도착점을 제외한 정점에서 보낸 유량과 나간 유량은 같다.       
---

이러한 문제를 해결하기 위해서 사용되는 알고리즘은 대표적으로 두 개의 알고리즘이 존재하는데,\
바로 포드-폴커슨 알고리즘(Ford-Fulkerson algorithm)과 디닉 알고리즘(Dinic's Algorithm), 에드몬트 카프(Edmonds-Karp)알고리즘 등이 있다.  
이 중에 포드 폴커슨 알고리즘에 대해 자세히 알아보도록 하겠다. 

포드 폴커슨 알고리즘을 자세히 알아보기 전에 먼저 네트워크 플로우의 진행 방식에 대해 간단히 알아보자면, 

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfMzgg/MDAxNTI4ODc3MTMyODg0.2gf1YpY4Ygu7renBP-l06LGPk07myMGp327x4dAWUgAg.H7mljrE_4CccWYcSLnb6zuaS0O5MLUGUVk65AI_bgIIg.PNG.jh20s/image.png?type=w800)

이 그래프에서 각 간선은 유량/용량을 나타내고 S->T로 진행된다. 

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfODEg/MDAxNTI4ODc3MTI4OTA4.8Vf0f1b-dokYXvwMS5K0OXhRVOYM0oCFp8PsZ_smG9sg.kykjnwANZGSCxzRC5WLYt_eDtPSf5t0Eakkv5iNkgxIg.PNG.jh20s/image.png?type=w800)
위로 2를 흘려 보내고

![](https://mblogthumb-phinf.pstatic.net/MjAxODA2MTNfMjE3/MDAxNTI4ODc3MTIyNjgy.KYNE-KkUKkOz-z0YOGBLze3x2fd6MyW8Xnr1QEmxdO4g.a9SkxjJ4NUYM04tl1UoMj5N4ED2vM16I2aPMvRQrCYMg.PNG.jh20s/image.png?type=w800)
아래로 2를 흘려 보냈다면, 전체적으로 S에서 T로 4를 흘려보내 준 것이고 위 그래프에서는 최대 4만큼의 유량을 흘려보내 줄 수 있다고 한다. 

이제 포트 풀커슨 알고리즘의 작동 방식에 대해 자세히 알아보자.
