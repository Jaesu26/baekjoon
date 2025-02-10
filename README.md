# Baekjoon Online Judge

[백준](https://www.acmicpc.net/) 알고리즘 문제풀이 (파이썬)

## 백준 하면서 알게된 점

`1.` 같은 언어, 같은 코드여도 시간이 다를 수 있음

`2.` tuple unpacking을 적극적으로 활용하자

`3.` `Python` vs `PyPy` : `Python`은 느리고 메모리 사용 적음, `PyPy`는 빠르고 메모리 사용 많음 

`4.` `BFS` 구현할 때 `queue`에서 `popleft` 하고 방문 여부를 확인하는 것이 아닌 `append` 하기 전에 확인해야 한다, 그렇지 않으면 중복하여 방문하게 된다 (여태까지 비효율적으로 구현했네...)

`5.` `4`의 내용은 DFS도 마찬가지이다, 재귀를 이용해 구현할 때 방문 여부를 확인하고 방문할지 말지 판단해야 한다 (방문하고 나서 방문 여부를 확인하면 중복하여 방문하게 된다)

`6.` 반복문을 통해 입력을 많이 받는 상황이면 시간 초과를 피하기 위해 `input` 대신에 `sys.stdin.readline`을 사용하자

`7.` 음수 사이클 유무를 확인하기 위해 벨만-포드 알고리즘을 적용할 시 거리를 `float("inf")`로 초기화하는 대신 큰 양수를 사용해야 한다, `float("inf")`와의 상수 연산은 `float("inf")`이므로 최단 경로 갱신이 되지 않는다
