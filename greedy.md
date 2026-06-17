# 그리디 알고리즘 (Greedy Algorithm)

## 동전 0

- 문제 출처: [백준 11047번](https://www.acmicpc.net/problem/11047)

`-` 준규가 가지고 있는 동전은 총 $N$ 종류이고 각각의 동전을 매우 많이 가지고 있다

`-` 동전을 적절히 사용해서 그 가치의 합을 $K$로 만들려고 한다

`-` 이때 필요한 동전 개수의 최솟값을 구하는 프로그램을 작성하시오

`-` 동전의 가치 $A_i$가 오름차순으로 주어진다 ($1 \le A_i \le 1000000, A_1 = 1$, $i \le 2$인 경우에 $A_i$는 $A_{i-1}$의 배수)


```python
arr = [0] * 10001
N = int(input())
for i in range(N):
    arr[int(input())] += 1
for i in range(1, 10001):
    for j in range(arr[i]):
        print(i)

# input
# 5
# 1
# 5
# 5
# 2
# 4
```

     5
     1
     5
     5
     2
     4
    

    1
    2
    4
    5
    5
    

## 잃어버린 괄호

- 문제 출처: [백준 1541번](https://www.acmicpc.net/problem/1541)

`-` 세준이는 양수와 `+`, `-`, 그리고 괄호를 가지고 식을 만들고 난 후 괄호를 모두 지웠다 

`-` 그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다

`-` 한 번만 `minus 기호`가 나오면 그 뒤에 수는 모두 빼기로 만들 수 있음

`-` $50 - 70 + 80 + 90 \longrightarrow 50 - (70 + 80 + 90)$

`-` `join()` , `split()` , `replace()` , `map()` 함수 사용


```python
string = str(input())
s1 = string.split('-')
s2 = s1[0]
s2 = s2.split('+')
s2 = list(map(int, s2))
s3 = s1[1:]
if len(s3) > 0:
    s3 = '-'.join(s3)
    s3 = s3.replace('-', '+')
    s3 = s3.split('+')
    s3 = list(map(int, s3))
    print(sum(s2) - sum(s3))
else: 
    print(sum(s2))

# input
# 00000+00000+00000+00000+00000+0
```

     00000+00000+00000+00000+00000+0
    

    0
    

## ATM

- 문제 출처: [백준 11399번](https://www.acmicpc.net/problem/11399)

`-` 배열을 sort 함수를 통해 오름차순으로 정렬

`-` $\text{최솟값} = a_1\times N + a_2\times(N-1) + \cdots + a_n \times 1$


```python
N = int(input())
times = list(map(int, input().split()))
times.sort()
print(sum([times[i] * (N - i) for i in range(N)]))

# input
# 5
# 3 1 4 3 2
```

     5
     3 1 4 3 2
    

    32
    

## 주유소

- 문제 출처: [백준 13305번](https://www.acmicpc.net/problem/13305)

`-` 현재 주유소 가격보다 더 싼 주유소가 있는 곳까지의 거리만큼만 구매하여 더 싼 주유소까지 감

`-` 정확히는 현재 주유소 가격보다 더 싼 주유소 중 가장 거리가 가까운 주유소

`-` 더 싼 주유소 까지 갔으면 그 주유소보다 더 싼 주유소가 있는 곳 까지의 거리만큼만 구매하여 더 싼 주유소까지 감

`-` 만약 더 싼 곳이 없으면 목표지점까지 남은 거리 만큼 기름을 구매 

`-` 이를 도착할 때 까지 반복함

`-` 백준 문제 예시를 보자

`1.` 기름 가격이 현재 $5$원 보다 더 싼 곳은 $2$원이므로 $5$원에서 $2$원까지 거리인 $2$km를 갈 수 있을 정도만 기름을 구매함 ($2$L 구매)

`2.` 현재 $2$원 보다 더 싼 곳은 $1$원이므로 $2$원에서 $1$원까지 거리인 $4$km를 갈 수 있을 정도만 기름을 구매함 ($4$L 구매)

`3.` 목표 지점에 도착했으므로 끝 


```python
N = int(input())
lengths = list(map(int, input().split()))
oils = list(map(int, input().split()))
idx = 0
costs = 0
for i in range(1, len(oils)):
    costs += oils[idx] * lengths[i - 1]
    if oils[i] <= oils[idx]:
        idx = i
print(costs)

# input
# 4
# 3 3 4
# 1 1 1 1
```

     4
     3 3 4
     1 1 1 1
    

    10
    

### 주유소 문제 디버깅 

`-` 아래는 잘못된 코드임

```python
N = int(input())
lengths = list(map(int, input().split()))
oils = list(map(int, input().split()))
idx = 0
costs = 0
for i in range(1, len(oils)):
    if oils[i] <= oils[idx]:
        costs += oils[idx] * lengths[idx]
        idx = i
    else:
        costs += oils[idx] * lengths[i - 1]
print(costs)
```

`-` 아래 코드가 잘못됨

```python
costs += oils[idx] * lengths[idx]
```

`-` 아래 코드로 수정해야함

```python
lengths[idx] ---> lengths[i - 1]
```

`-` 디버깅 하면서 느낀점: 다양한 상황을 고려하고 [$\star$]`종이에 쓰면서`[$\star$] 어떻게 흘러가는지 분석하자

## 회의실 배정

- 문제 출처:[백준 1913번](https://www.acmicpc.net/problem/1931)

`-` 가장 빨리 끝나는 회의를 배정하는 것이 best임

`-` 시작 시간과 종료 시간이 같은 것이 있을 수 있으므로 종료 시간이 같다면 먼저 시작하는 것을 앞에 배정함

`-` 그 회의를 마치고 다음 회의를 고를 때 마찬가지로 가장 빨리 끝나는 회의를 배정함 

`-` 이를 반복하면 된다

`-` `[1, 1]`과 `[0, 1]`이 있다면 `[0, 1]`을 배정하고 `[1, 1]`을 배정 해야한다

`-` 순서를 바꾸면 $1$개가 최대임

`-` 처음에는 종료 시간만 고려해서 틀림 ---> 디버깅을 합시다

`-` 그래서 생각한 것이 "아!, 시작 시간과 종료 시간 간격이 짧은 것이 더 좋겠지(`[5, 8]`보단 `[6, 8]`더 좋을 거야)"라고 했음

`-` 그래서 종료 시간 기준으로 오름차순 정렬한 후 시작 시간 기준으로 내림차순 정렬을 했지만 위에서 다룬 `[1, 1]`과 `[0, 1]`의 상황에 의해 틀림

`-` 종료 시간기준으로 오름차순 정렬한 후 이를 시작 시간 기준으로 오름차순 정렬해야 함 ---> 맞았습니다


```python
meeting_time = []
N = int(input())
for i in range(N):
    meeting_time.append(list(map(int, input().split())))
meeting_num = 1
meeting_time.sort(key = lambda finish_time: (finish_time[1], finish_time[0]))  # 종료 시간을 기준으로 오름차순 정렬한 것을 시작 시간을 기준으로 오름차순 정렬
fir_meeting = meeting_time[0]
for i in range(1, N):
    if meeting_time[i][0] >= fir_meeting[1]:
        meeting_num += 1
        fir_meeting = meeting_time[i]
print(meeting_num)

# input
# 11
# 1 4
# 3 5
# 0 6
# 5 7
# 3 8
# 5 9
# 6 10
# 8 11
# 8 12
# 2 13
# 12 14
```

     11
     1 4
     3 5
     0 6
     5 7
     3 8
     5 9
     6 10
     8 11
     8 12
     2 13
     12 14
    

    4
    

## A $\to$ B 

- 문제 출처: [백준 16953번](https://www.acmicpc.net/problem/16953)

`-` 미로찾기 할 때 출발지점에서 시작하는 것보다 도착지점에서 시작하는 것이 미로 탈출을 더 쉽게 만듦

`-` 이런 느낌으로 접근하자

`-` $A$에서 $B$를 만드는 것보다 역으로 $B$에서 $A$를 만든다고 생각하는 거임

`-` 만약 $B$가 $x$자리수인데 $x$자리 숫자가 $1$이라면 $A$에서 $B$를 만드는 과정에서 $A$가 $x-1$자리인 순간이 있었고 그 때 마지막 자리에 $1$을 추가하는 연산을 했음을 알 수 있음

`-` 이런식으로 생각하면 만약 $B$의 $x$자리의 숫자가 $1$이 아닌 홀수다 ---> $A$에서 $B$를 만드는 것이 불가능

`-` 만약 $B$의 $x$자리의 숫자가 짝수다 ---> 일단 $B$를 $2$로 나누자

`-` 위에서 한 것을 반복하면 됨 

`-` 그리디 알고리즘으로 풀었는데 BFS로도 풀 수 있다고 함 ---> 나중에 그래프 탐색 문제도 풀어보자


```python
A, B = map(int, input().split())
num = 1
while B != A:
    if str(B)[-1] == '1' and B > A:
        B = int(str(B)[:-1])
        num += 1
    elif B % 2 == 1 or B < A:
        num = -1
        break
    else:
        B = B // 2
        num += 1
print(num)

# input
# 2 162
```

     2 162
    

    5
    

## 카드 합체 놀이
- 문제 출처: [백준 15903번](https://www.acmicpc.net/problem/15903)

`-` 가장 작은 카드 $2$장을 뽑아서 더한 값으로 초기화 함

`-` 또 다시 전체 카드 중에서 가장 작은 카드 $2$장을 뽑아서 더한 값으로 초기화 함

`-` 위를 반복하면 됨

`-` 힙(heap) 자료구조로 풀 수 있다고 함 ---> 공부하기


```python
n, m = map(int, input().split())
cards = list(map(int, input().split()))
cards.sort()
cnt = 0
while cnt < m:
    cards[0] = cards[1] = cards[0] + cards[1]
    cards.sort()
    cnt += 1
print(sum(cards))

# input
# 3 1
# 3 2 6
```

     3 1
     3 2 6
    

    16
    

## 보물

- 문제 출처: [백준 1026번](https://www.acmicpc.net/problem/1026)

`-` 배열 $A$만 재배열이 가능하고 배열 $B$는 재배열이 불가능함

`-` 제일 큰 $B$에 제일 작은 $A$를 매칭

`-` 그 둘을 제외하고 다시 제일 큰 $B$에 제일 작은 $A$를 매칭

`-` 이를 반복하면 될 것 같다


```python
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))
S = 0
while A and B:
    S += A.pop(A.index(min(A))) * B.pop(B.index(max(B)))
print(S)

# input
# 9
# 5 15 100 31 39 0 0 3 26
# 11 12 13 2 3 4 5 9 1
```

     9
     5 15 100 31 39 0 0 3 26
     11 12 13 2 3 4 5 9 1
    

    528
    

## 거스름돈

- 문제 출처: [백준 5585번](https://www.acmicpc.net/problem/5585)

`-` $\text{거스름돈 = 1000 - 지불 금액}$

`-` 거스름돈 개수가 가장 작을려면 큰 금액을 잔돈으로 주면 된다

`-` $500$엔으로 거슬러주고 거슬러줄 돈이 $500$엔보다 작아지면 $100$엔으로 거슬러주고 이를 $1$엔까지 반복하면 된다


```python
money = 1000 - int(input())
num = 0
for i in [500, 100, 50, 10, 5, 1]:
    num += money // i
    money %= i
print(num)
```

     380
    

    4
    

## 카드 정렬하기

- 문제 출처: [백준 1715번](https://www.acmicpc.net/problem/1715)

`-` 카드를 $2$묶음씩 골라야 하는데 최소한의 비교를 위해서는 카드의 수가 가장 작은 $2$묶음을 고르면 된다

`-` 매 선택마다 최솟값 $2$개를 고르고 고른 후에 정렬을 한다 ---> 이를 카드 뭉치가 하나 남을 때까지 반복하면 된다

`-` 최솟값만 고르면 되니 heap 자료구조를 사용하여 해결하면 될 것 같다


```python
import heapq

N = int(input())
heap = [int(input()) for _ in range(N)]
heapq.heapify(heap)
value = 0
while len(heap) >= 2:
    tmp = heapq.heappop(heap) + heapq.heappop(heap)
    value += tmp
    heapq.heappush(heap, tmp)
print(value)

# input
# 3
# 10
# 20
# 40
```

     3
     10
     20
     40
    

    100
    

## 로프

- 문제 출처: [백준 2217번](https://www.acmicpc.net/problem/2217)

`-` 여러개의 로프를 사용하여 무게가 $w$인 하나의 물체를 들어올리면 된다

`-` 로프가 $k$개 있다고 하자

`-` 로프 중에서 들 수 있는 중량이 가장 낮은 로프의 중량을 $x$라고 하자

`-` 그러면 $k$개의 노드를 사용하여 최대 $x\times k$ 무게까지 들어올릴 수 있다

`-` 이를 반대로 생각하여 중량이 가장 높은 로프의 중량을 $y$라고 하고

`-` 그 다음으로 중량이 높은 로프의 중량을 $z$라고 했을 때

`-` 하나의 노드만을 사용하여 $y$만큼 들어올리느냐 vs 두 개의 노드를 사용하여 $2\times z$만큼 들어올리느냐의 싸움이다

`-` 만약 전자의 무게가 더 높다면 로프가 버틸 수 있는 최대 중량은 $y$이다

`-` 후자의 무게가 더 높다면 $z$다음으로 가장 중량이 높은 로프의 중량을 $a$라고 했을 때

`-` 두 개의 노드를 사용하여 $2\times z$만큼 들어올리느냐 vs 세개의 노드를 사용하여 $3\times a$만큼 들어올리느냐의 싸움이다

`-` 이를 계속 반복하면 된다

`-` 리스트를 정렬한 후 위를 적용하자

```python
n = int(input())
w = [int(input()) for _ in range(n)]
w.sort(reverse = True)
weight = w[0]
for i in list(range(n - 1)):
    if w[i] * (i + 1) >= w[i + 1] * (i + 2):
        weight = w[i] * (i + 1)
        break
    else:
        weight = w[i + 1] * (i + 2)
print(weight)
```

`-` 위 논리에 따라 구현한 코드가 위의 것인데 위의 코드는 틀린 코드이다

`-` 위에서 두 개의 노드를 사용하여 $2\times z$만큼 들어올리느냐 vs 세개의 노드를 사용하여 $3\times a$만큼 들어올리느냐의 싸움이라고 했었다

`-` 그런데 사실 전자가 크더라도 이것이 최댓값임을 보장할 수 가 없다

`-` 왜냐하면 $a$다음으로 가장 큰 중량을 $b$라 할 때 네개의 노드를 사용하여 $4\times b$ 한 것이 $2\times z$보다 클 수 있기 때문이다

`-` 이를 반영하여 탐색을 멈추지 않고 모든 노드에 대해 실행하자


```python
n = int(input())
w = [int(input()) for _ in range(n)]
w.sort(reverse = True)
weight = w[0]
for i in list(range(1, n)):
    if weight <= w[i] * (i + 1):
        weight = w[i] * (i + 1)
print(weight)

# input
# 2
# 10
# 15
```

     2
     10
     15
    

    20
    

`-` 일단 위의 코드는 정답이다 

`-` 그런데 왜 정답인지 모르겠다

`-` 왜냐하면 노드의 수가 최대 $100000$개이다

`-` 정렬하는데 $N\log N$ 이고

`-` for문이 $N$번 작동하므로 전체 코드의 시간 복잡도는 $O\left(N^2 \log N\right)$이다

`-` 그런데 시간제한이 $2$초여서 시간초과일 줄 알았는데 아니다...

`-` 웃긴게 걸린 시간이 $0.14$초이다 ㅋㅋㅋㅋ

`-` 내가 바보라 잘못 생각했다....

`-` 정렬을 for문마다 하는것이 아니라 처음 한번만 하는것이므로 곱하기가 아니라 더하기다

`-` 즉 전체 코드의 시간 복잡도는 $O\left(N+ N\log N\right)$ = $O\left(N\log N\right)$이다 ($N$은 Big-O 표기법에 의해 버려진다)

## 소트

- 문체 출차: [백준 1083번](https://www.acmicpc.net/problem/1083)

`-` 소트한 결과가 사전순으로 가장 뒷서는 것은 배열의 원소를 내림차순으로 정렬한 것이다

`-` 만약 $S$가 충분히 크다면 소트한 결과가 사전순으로 가장 뒷서는 것은 내림차순 정렬된 원소이다

`-` 하지만 $S$가 배열의 원소를 내림차순 정렬하기에 충분치 않다면 적당한 합의를 봐야한다

`-` 사전순으로 가장 뒷서로독 해야하므로 가장 중요한 건 첫 번째 원소이다

`-` 예컨대 배열 $A$인 $[5, 1, 2, 3, 4]$과 배열 $B$인 $[4, 5, 3, 2, 1]$을 사전순으로 비교하면 $A$가 $B$보다 사전순으로 뒷선다

`-` 왜냐하면 사전순으로 비교는 첫 번째 원소부터 하기때문에 뒤에 원소의 크기 비교는 우선순위에서 밀리기 때문이다

`-` 단순하게 생각하면 어차피 최종 목표는 내림차순 정렬된 배열이므로 일단 가장 큰 원소를 배열의 맨 앞으로 이동시키도록 한다

`-` 만약 $S$가 충분치 않아 불가능하다면 다음으로 큰 원소를 배열의 맨 앞으로 이동시키도록 한다

`-` 이런식으로 진행하여 배열의 첫 번째 원소를 확정지었으면 두 번째 원소부터 마지막 원소까지 같은 논리를 적용한다

`-` 이를 소트한 횟수가 $S$가 되거나 배열의 원소가 내림차순으로 정렬될 때까지 반복한다

`-` 참고로 두 원소를 교환하는 방법을 통해 배열의 원소를 내림차순 정렬하는덴 최악의 경우 $(n-1) + (n-2)+\cdots + 1 = \dfrac{n(n-1)}{2}$이므로 $S$는 이를 초과할 필요가 없다


```python
N = int(input())
array = list(map(int, input().split()))
sorted_array = sorted(array, reverse=True)
S = int(input())
max_sort_count = (N**2 - N) // 2
S = max_sort_count if S > max_sort_count else S
step = 0  # 정답 배열에 원소를 추가할 때마다 +1
answer = []
while step < N:  # array에 남은 원소가 1개일 때까지 (1개 남으면 그대로 추가하면 됨)
    kth = 0  # 현재 step에서 정답 배열에 추가해야할 sorted_array에서 k번째로 큰 원소
    while True:
        possible_max = sorted_array[kth]  # 정답 배열에 추가될 가능성이 있는 sorted_array에서 k번째로 큰 원소 
        index_max = array.index(possible_max)  # array에서 possible_max의 인덱스
        if index_max <= S:  # 옮기기에 S가 충분하다면
            kth_element = array.pop(index_max)
            answer.append(kth_element)
            sorted_array.pop(kth)
            S -= index_max
            break
        else:
            kth += 1 
    step += 1
answer += array
print(*answer)

# input
# 5
# 3 5 1 2 4
# 2
```

     5
     3 5 1 2 4
     2
    

    5 3 2 1 4
    

## 수들의 합

- 문제 출처: [백준 1789번](https://www.acmicpc.net/problem/1789)

`-` $N$을 최대로 크게 할려면 가장 작은 자연수부터 고려한 후 $S$에서 해당 값을 제외한 후 이를 다시 반복하면 된다

`-` 만약 자연수 $x$를 고려했을 때 $S-x$가 $x$보다 작거나 같다면 $S$를 마지막 자연수로 고려하고 알고리즘을 종료한다 

`-` 결과적으로 보면 $S$를 $1+2+\cdots+x+l$로 나타내는 것이며 $N=x+1$이다

`-` 왜 위와 같이 하면 $N$이 가장 커지는지 알아 보자

`-` 만약 현재 상황에서 선택할 수 있는 가장 작은 수 $x$가 아닌 이보다 더 큰 $x'$을 선택했다고 하자

`-` 그러나 $x'=x+k$로 모든 자연수가 선택된 후 $x'$을 $x$와 $k$로 나누고 가장 큰 수 $m$에 $k$를 더한다면 $X'$이 아닌 $x$를 선택한 것과 최소 같은 $N$을 얻게 된다

`-` 또한 $m+k$를 여태까지 선택되지 않은 서로 다른 두 자연수로 나뉠 여지도 존재한다

`-` 따라서 현재 상황에서 고려할 수 있는 가장 작은 작은 수를 선택하는 것이 $N$을 가장 크게 만드는 결과를 가져온다 


```python
def solution():
    S = int(input())
    maximum = 1
    N = 0
    while True:
        if S - maximum > maximum:
            S -= maximum
            maximum += 1
            N += 1
        else:
            N += 1
            break
    print(N)


solution()

# input
# 200
```

     200
    

    19
    

## 30

- 문제 출처: [백준 10610번](https://www.acmicpc.net/problem/10610)

`-` 어떤 수 $x$가 $30$의 배수일려면 $3$의 배수이면서 $10$의 배수여야 한다

`-` $x$가 $10$의 배수일려면 일의 자리가 $0$이어야 하며 $3$의 배수일려면 각 자릿수의 합이 $3$의 배수여야 한다

`-` 주어진 수 $x$에 $0$이 포함되면서 각 자릿수의 합이 $3$의 배수이면 $x$는 $30$이다

`-` 이를 가장 크게 만들면 $30$의 배수가 되는 가장 큰 수가 되는데 이는 단순히 $0$을 일의 자리에 배치하고 가장 큰 숫자부터 맨 앞자리에 배치하면 된다 

`-` 그런데 $0$은 가장 작은 숫자이므로 $x$를 각 자릿수를 기준으로 내림차순 정렬해도 괜찮다


```python
def solution():
    N_str = input()
    is_multiple_3 = sum(int(s) for s in N_str) % 3 == 0
    is_multiple_10 = "0" in N_str 
    if not (is_multiple_3 and is_multiple_10):
        print(-1)
    else:
        print("".join(sorted(N_str, reverse=True)))


solution()

# input
# 80875542
```

     80875542
    

    88755420
    

## 기타줄

- 문제 출처: [백준 1049번](https://www.acmicpc.net/problem/1049)

`-` 가장 싼 패키지와 가장 싼 낱개만 사용하고 나머지는 사용하지 않는 것이 비용을 최소로 할 수 있다

`-` 단순하게 생각하면 가능한 패키지를 많이 사고 남는 줄은 낱개로 사는 것이 좋아보인다

`-` 하지만 낱개 가격이 패키지에 비해 매우 비싼 경우, 줄을 필요한 것보다 더 사더라도 패키지로만 구매하는 것이 낫다

`-` 반대로 패키지의 가격이 매우 비싼 경우, 오직 낱개로만 구매하는 것이 낫다


```python
def solution():
    N, M = map(int, input().split())
    INF = 1e4
    package_min_price = INF
    item_min_price = INF
    for _ in range(M):
        package_price, item_price = map(int, input().split())
        if package_price < package_min_price:
            package_min_price = package_price
        if item_price < item_min_price:
            item_min_price = item_price
    package_only = ((N // 6) + 1) * package_min_price
    item_only = N * item_min_price
    general = (N // 6) * package_min_price + (N % 6) * item_min_price
    answer = min(package_only, item_only, general)
    print(answer)


solution()

# input
# 4 2
# 12 3
# 15 4
```

     4 2
     12 3
     15 4
    

    12
    

## 팰린드롬 만들기

- 문제 출처: [백준 1213번](https://www.acmicpc.net/problem/1213)

`-` 이름이 짝수라면 각 알파벳의 등장 횟수도 짝수여야 한다

`-` 이름이 홀수라면 하나의 알파벳은 홀수 번 등장하고 나머지 알파벳은 짝수 번 등장해야 팰린드롬을 만들 수 있다

`-` 만약 위의 조건을 만족하지 않으면 팰린드롬을 만들 수 없는 것이다

`-` 정답이 여러개일 경우 사전순으로 앞서는 것을 출력해야 하므로 사전순으로 앞서는 알파벳을 팰린드롬의 앞 글자로 사용해야 한다

`-` 팰린드롬을 좌우 대칭이므로 가운데를 기준으로 앞에만 배치한 후 가운데에 들어갈 알파벳이 있다면 추가한 후 앞의 배치를 반전하여 이어 붙이면 된다


```python
def get_alphabet2count(name):
    alphabet2count = {}
    for alphabet in name:
        if alphabet not in alphabet2count:
            alphabet2count[alphabet] = 1
            continue
        alphabet2count[alphabet] += 1
    return alphabet2count


def solution():
    name = input()
    alphabet2count = get_alphabet2count(name)
    length = len(name)
    even = []
    odd = []
    for alphabet, count in alphabet2count.items():
        if count % 2 == 0:
            even.extend([alphabet] * (count // 2))
        else:
            even.extend([alphabet] * (count // 2))
            odd.append(alphabet)   
    even.sort()
    odd.sort()
    if (length % 2 == 0 and odd) or (length % 2 == 1 and len(odd) != 1):
        print("I'm Sorry Hansoo")
        return
    print("".join(["".join(even), "".join(odd), "".join(even)[::-1]]))


solution()

# input
# AAABB
```

     AAABB
    

    ABABA
    

## 팔

- 문제 출처: [백준 1105번](https://www.acmicpc.net/problem/1105)

`-` 만약 $L$과 $R$의 자릿수가 다르다면 $10^{\lfloor\log R \rfloor}$은 $L$보다 크고 $R$보다 작거나 같다

`-` 해당 숫자에는 $8$이 $0$개 들어있으므로 문제의 정답이다 

`-` 예컨대 $L = 88, R = 150$이라면 $10^{\lfloor\log R \rfloor}=100$이다

`-` 만약 $L$과 $R$의 자릿수가 같다면 앞에 숫자부터 탐색한다

`-` 만약 두 숫자가 동일하다면 해당 숫자를 무조건 사용해야 하며 해당 숫자가 $8$이라면 정답에 $1$을 더하면 되고 아니라면 다음 자리로 넘어가면 된다

`-` 그리고 해당 자리의 숫자는 제거했다고 생각하자

`-` 만약 두 숫자가 다르다면 두 수 사이에 $8$이 들어가지 않는 숫자가 무조건 존재하므로 탐색을 종료한다

`-` 이유는 다음과 같은데, 일단 두 수의 차이가 크면 선택의 기회만 많아지므로 두 수의 차이가 $1$인 경우를 고려하겠다

`-` $L$의 맨 앞자리가 $8$이라면 $900$을 선택하면 되고 $R$의 맨 앞자리가 $8$이라면 $799$를 선택하면 되기 때문이다

`-` 예컨대 $L=799, R=800$이라면 두 수 사이에 존재하면서 $8$이 들어가지 않는 수로 $799$가 존재한다

`-` $L=899, R=900$이어도 $900$을 선택하면 되므로 상관없다

`-` 이외의 경우엔 가장 큰 자리에 $8$이 등장하지도 않으므로 고려할 가치가 없다


```python
def solution():
    L, R = input().split()
    if len(L) != len(R):
        print(0)
        return
    answer = 0
    for l, r in zip(L, R):
        if l != r:
            break
        if l != "8":
            continue
        answer += 1
    print(answer)


solution()

# input
# 8808 8880
```

     8808 8880
    

    2
    

## 접두사

- 문제 출처: [백준 1141번](https://www.acmicpc.net/problem/1141)

`-` 사전순으로 단어를 정렬한 후 현재 위치한 단어가 다음 순서 단어의 접두사인지 아닌지 확인하면 된다


```python
def solution():
    N = int(input())
    words = [input() for _ in range(N)]
    words.sort()
    answer = N
    for i in range(N - 1):
        word = words[i]
        next_word = words[i + 1]
        if next_word.startswith(word):
            answer -= 1
    print(answer)


solution()

# input
# 3
# topcoder
# topcoder
# topcoding
```

     3
     topcoder
     topcoder
     topcoding
    

    2
    

## 뒤집기

- 문제 출처: [백준 1439번](https://www.acmicpc.net/problem/1439)

`-` 같은 숫자로 이루어진 연속된 부분 문자열은 동일한 작업을 수행해야 하므로 사실상 하나의 문자열로 취급된다

`-` 동일한 작업을 수행하지 않는 것은 부분 문자열을 쪼개는 행위이다

`-` 그런데 모두 같은 숫자로 만들기 위해서는 위에서 쪼갠 문자열을 결국에는 원 상태로 되돌려야 한다

`-` 즉, 동일한 작업을 수행해야 최소 횟수인 정답을 도출할 수 있다


```python
def solution():
    S = input()
    change_count = 0
    S_last = S[0]
    for s in S:
        if S_last == s:
            continue
        S_last = s
        change_count += 1
    answer = (change_count + 1) // 2
    print(answer)


solution()

# input
# 11101101
```

     11101101
    

    2
    

## 삼각형 만들기

- 문제 출처: [백준 1448번](https://www.acmicpc.net/problem/1448)

`-` 삼각형에서 가장 긴 변의 길이는 나머지 변들의 길이의 합보다 작다

`-` 주어진 $N$개의 빨대를 길이를 기준으로 내림차순 정렬하자

`-` 가장 긴 빨대의 길이를 $x_1$, 그 다음을 $x_2$, 그 다음을 $x_3$라고 하자

`-` 주어진 $N$개의 빨대로 만들 수 있는 삼각형 세 변의 길이의 합의 최댓값은 $x_1+x_2+x_3$이다

`-` 그러나 $x_1 \ge x_2 + x_3$라면 삼각형을 만들 수 없다

`-` 이 경우 $x_1$ 길이의 빨대는 삼각형을 구성하는데 사용할 수 없으므로 제외한다

`-` 남은 빨대에서 길이를 기준으로 상위 $3$개의 빨대를 선별하고 삼각형이 만들어지는지 확인한다

`-` 이를 삼각형이 만들어질 때까지 반복하면 된다


```python
def solution():
    N = int(input())
    lengths = sorted([int(input()) for _ in range(N)], reverse=True)
    for i in range(N - 2):
        top_1, top_2, top_3 = lengths[i], lengths[i + 1], lengths[i + 2]
        if top_1 >= top_2 + top_3:
            continue
        print(top_1 + top_2 + top_3)
        return
    print(-1)


solution()

# input
# 3
# 2
# 2
# 1
```

     3
     2
     2
     1
    

    5
    

## 합

- 문제 출처: [백준 1132번](https://www.acmicpc.net/problem/1132)

`-` 숫자가 알파벳으로 표현되어 있어 어떻게 최대합을 구할지 애매해 보인다

`-` 하지만 알파벳으로 표현된 숫자는 각 자릿수에 따라 합으로 나타낼 수 있다

`-` 예를 들어 입력된 수가 `CBA`라면 이는 사실 `100C + 10B + A`이다

`-` N개의 입력에 대해 수를 위와 같은 꼴로 나타내 모두 합한다

`-` 그러면 최종 결과는 `xA + yB + ...`이 될 것이다

`-` 알파벳 앞의 계수가 가장 큰 것부터 $9$를 부여하면 합의 최댓값이 된다

`-` 만약 알파벳이 $10$개인 경우 $0$인 알파벳이 존재한다

`-` 이는 문제의 조건에 따라 수의 가장 처음에 주어지지 않은 알파벳 중 계수가 가장 작은 알파벳을 $0$으로 설정하면 된다


```python
from collections import defaultdict


def solution():
    N = int(input())
    alphabet2count = defaultdict(int)
    alphabet2is_zero = {}
    for _ in range(N):
        num = input()
        for i, digit_index in enumerate(range(len(num) - 1, -1, -1)):
            digit = num[digit_index]
            if i == len(num) - 1:
                alphabet2is_zero[digit] = False
            alphabet2count[digit] += 10**i
    alphabets = sorted(alphabet2count, key=alphabet2count.get, reverse=True)
    max_digit = 9
    result = 0
    should_exist_zero = len(alphabet2count) == 10
    if should_exist_zero:
        for a in reversed(alphabets):
            if a not in alphabet2is_zero:
                zero_alphabet = a
                break
    for a in alphabets:
        if should_exist_zero:
            if a == zero_alphabet:
                continue
        result += alphabet2count[a] * max_digit
        max_digit -= 1
    print(result)


solution()

# input
# 1
# ABCDEFGHIJ
```

     1
     ABCDEFGHIJ
    

    9876543210
    

## 단어 수학

- 문제 출처: [백준 1339번](https://www.acmicpc.net/problem/1339)

`-` [합](https://www.acmicpc.net/problem/1132) 문제를 풀었으면 쉽게 해결할 수 있다


```python
from collections import defaultdict


def update_alphabet2num(alphabet2num, alphabets):
    i = 1
    for a in alphabets[::-1]:
        alphabet2num[a] += i
        i *= 10


def solution():
    N = int(input())
    alphabet2num = defaultdict(int)
    for _ in range(N):
        alphabets = input()
        update_alphabet2num(alphabet2num, alphabets)
    alphabet_num_pair = sorted(alphabet2num.items(), key=lambda x: -x[1])
    answer = 0
    i = 9
    for alphabet, num in alphabet_num_pair:
        answer += num * i
        i -= 1
    print(answer)


solution()

# input 
# 2
# AB
# BA
```

     2
     AB
     BA
    

    187
    

## 용액 2

- 문제 출처: [백준 32178번](https://www.acmicpc.net/problem/32178)

`-` 나는 투 포인터 문제겠거니 하고 문제의 알고리즘 분류를 확인했다

`-` 그리디, 정렬, 누적 합이 보이고 풀이 방법이 떠올랐다

`-` 안 보고 풀어야 되는데 이미 봐서 어쩔 수 없다

`-` 일단 임의의 구간 합은 누적 합 배열을 만들어두면 $O(1)$에 확인할 수 있다

`-` 가장 중성에 가까운 용액의 특성값을 찾는 방법으론 누적 합 배열에서 두 원소의 차이가 가장 $0$과 가까운 두 원소를 찾는것이 있다

`-` 우선 누적 합 배열에 $0$을 추가하자 ($0$번째까지의 누적 합을 나타내기 위함)

`-` 가장 가까운 두 원소를 찾기 위해 누적 합 배열을 오름차순 정렬한다

`-` 그리고 연속된 두 원소를 모두 탐색하고 $0$과 가장 가까운 두 원소를 정하면 된다

`-` 누적 합 배열의 원소를 정렬하면 순서가 보장되지 않을 수 있다

`-` $\operatorname{dp}[n]$을 처음부터 $n$번째까지 특성값들의 합이라고 하고 $\operatorname{dp}[5]=10, \operatorname{dp}[8]=-10$이라고 해보자

`-` $6$에서 $8$번째까지의 부분 합은 $\operatorname{dp}[8]-\operatorname{dp}[5]$이지만 $\operatorname{dp}[8]$이 더 작으므로 실제로 계산할 땐 $\operatorname{dp}[5]- \operatorname{dp}[8]$이 된다

`-` 그런데 어차피 $0$까지의 거리를 계산하는 것이고 이를 위해 절댓값을 씌운다

`-` 절댓값을 적용하면 둘 다 똑같으므로 상관없다

`-` 비교는 절댓값으로 하니 상관없지만 정답을 갱신할 땐 부호를 신경써야 한다

`-` 이제 모든 경우의 연속된 두 원소를 탐색하면 정답을 얻을 수 있음을 증명하자

`-` 누적 합 배열을 오름차순 정렬했으므로 $a\le b\le c\le d \cdots \le z$와 같다

`-` 만약 연속되지 않은 두 원소를 $x,y$를 골랐다고 해보자

`-` 두 원소 사이에는 $k_x, k_y$가 존재하며 $x\le k_x \le \cdots \le k_y \le y$이다

`-` 따라서 $x$와 $y$의 차이보다 $x$와 $k_x$ 또는 $y$와 $k_y$의 차이가 더 작으므로 $x,y$는 정답이 될 수 없다

`-` 연속된 두 원소로 가능한 경우는 $N$개 존재하므로 탐색의 시간 복잡도는 $O(N)$이다


```python
def solution():
    N = int(input())
    s_n = list(map(int, input().split()))
    prefix_sum = [0 for _ in range(N)]
    prefix_sum[0] = s_n[0]
    for i in range(1, N):
        prefix_sum[i] = s_n[i] + prefix_sum[i - 1]
    prefix_sum = [0] + prefix_sum
    prefix_sum = [(value, index) for index, value in enumerate(prefix_sum)]
    prefix_sum.sort()
    answer = 1e10
    left = 0
    right = N
    for i in range(N):
        value_left, index_left = prefix_sum[i]
        value_right, index_right = prefix_sum[i + 1]
        generated_value = value_right - value_left
        if abs(generated_value) < abs(answer):
            answer = generated_value
            if index_left < index_right:
                left = index_left + 1
                right = index_right
            else:
                left = index_left
                right = index_right + 1
                answer *= -1
    if left == 0:
        left += 1
    if right == 0:
        right += 1
    print(answer)
    print(*sorted([left, right]))


solution()

# input
# 4
# -1 5 7 2
```

     4
     -1 5 7 2
    

    -1
    1 1
    

## 수 묶기

- 문제 출처: [백준 17444번](https://www.acmicpc.net/problem/1744)

`-` 우선은 배열에 양수만 있다고 가정하자

`-` $1$과 임의의 수 $x$를 고려하자

`-` 둘을 곱하면 $x$이지만 각각을 더하면 $x+1$이다

`-` 따라서 $1$은 곱하는데 사용하는 것보다 더하는게 무조건 이득이다

`-` 따라서 $1$은 정답에 더해주고 배열에서 삭제하자

`-` $1$보다 큰 수에 대해선 더하는 것보다 곱하는게 낫다

`-` $xy \ge x+y \Longrightarrow 1 \ge \frac{1}{x} + \frac{1}{y}$

`-` $\frac{1}{x} + \frac{1}{y}$는 감소 함수이며 $x=2,y=2$일 때 등호가 성립한다

`-` 따라서 이보다 더 큰 값에 대해선 부등식이 성립한다

`-` 만약 배열에 홀수개의 원소가 있다면 가장 작은 원소는 더하고 나머지끼리 곱하는게 이득이다

`-` 가장 작은 수를 $a$, $a$와 곱할 원소를 $x$, 홀로 남아 더해질 수를 $y$라고 하자

`-` $a$를 곱하는데 사용하면 $ax + y$가 되고 $a$를 더하는데 사용하면 $xy + a$가 된다

`-` $x > 1$이고 $y\ge a$이므로 $x(y-a) \ge y- a$이다

`-` 따라서 $xy+a \ge ax+y$이므로 가장 작은 수는 더하는데 사용하는게 이득이다

`-` 이제 배열에 짝수개의 원소가 있다고 해보자

`-` 가장 큰 수 $a$와 그 다음으로 큰 수 $b$를 고려하자

`-` 그럼 $a$와 $b$끼리 곱하는게 이득이다

`-` 둘끼리 곱하지 않고 $a$는 $x$와 $b$는 $y$와 곱하는게 더 이득이라고 해보자

`-` 그런데 $b\ge x$이고 $a \ge y$이므로 $a(b-x) \ge y( b- x)$이고 $ab + xy\ge ax + by$이다

`-` 따라서 가장 큰 두 수를 곱하는게 이득이다

`-` 이제 음수를 고려하자

`-` 음수와 양수를 곱하는 건 손해이다

`-` 음수가 짝수개 있다면 서로를 곱하는게 이득이며 이때 절댓값이 큰 두 음수끼리 곱해나가야 이득이다

`-` 음수가 홀수개 있다면 절댓값이 가장 작은 음수를 더하고 나머지에 대해 곱하자

`-` 배열에 있는 $0$은 기본적으로 더하되 음수가 홀수개 있다면 절댓값이 가장 작은 음수와 곱하는게 이득이다

`-` $n=1$인 케이스에서 print가 아니라 return을 사용해서 30분 삽질함 (너무 힘들었음)

`-` `94%`에서 틀린거 보고 알았어야 했는데...


```python
def find_first_positive(sorted_array):
    for i, a in enumerate(sorted_array):
        if a > 0:
            return i


def bind_positives(positives):
    total = 0
    n = len(positives)
    if n <= 1:
        return sum(positives)
    positives.sort()
    if n % 2 == 1:
        total += positives.pop(0)
    while positives:
        big = positives.pop()
        small = positives.pop()
        if small <= 1:
            total += big + small
            continue
        total += big * small
    return total


def bind_negatives(negatives):
    total = 0
    n = len(negatives)
    if n <= 1:
        return sum(negatives)
    while len(negatives) >= 2:
        small = negatives.pop(0)
        big = negatives.pop(0)
        total += big * small
    total += sum(negatives)
    return total


def solution():
    N = int(input())
    a_n = [int(input()) for _ in range(N)]
    if N == 1:
        print(a_n[0])
        return
    a_n.sort()
    if a_n[0] <= 0 and a_n[N - 1] > 0:
        i = find_first_positive(a_n)
        positives = a_n[i:]
        negatives = a_n[:i]  # 0 포함
    elif a_n[0] > 0 and a_n[N - 1] > 0:
        positives = a_n
        negatives = []  # 0 포함
    else:
        positives = []
        negatives = a_n  # 0 포함
    answer = bind_positives(positives) + bind_negatives(negatives)
    print(answer)


solution()

# input
# 3
# -1
# 0
# 1
```

     3
     -1
     0
     1
    

    1
    

## 구두 수선공

- 문제 출처: [백준 14908번](https://www.acmicpc.net/problem/14908)

`-` 단계별로 풀어보기 중 그리디에 있는 문제이다

`-` 단계별로 풀어보기에 등재된 문제는 문제의 핵심 태그와 간략한 접근법이 적혀있기도 해서 문제 해결이 쉬워진다

`-` 이 문제의 핵심 태그는 그리디이고 간략한 접근법엔 `exchange argument 연습 문제 1`이라고 적혀있다 (근데 저게 뭔지는 모름)

`-` 이 문제가 아니더라도 단계별로 풀어보기에 등재된 문제를 해결할 땐 핵심 태그와 간략한 접근법을 확인하니 읽는 사람은 인지하면 좋다 (아무런 정보 없이 푼 게 아니다)

`-` 특정 작업을 수행한다는 것은 해당 작업을 제외한 나머지 작업의 보상금을 지불한다는 의미다

`-` 보상금을 적게 줄려면 손실이 가장 적은 작업을 먼저 수행해야 한다

`-` 즉, 작업을 수행해서 나머지 작업에 대한 보상금을 지불해야 될 때 이를 최소로 하는 작업을 먼저 수행해야 한다

`-` $x, y, D$ 순서가 있다고 해보자 ($D$는 그룹)

`-` 그런데 초기 상태에서 $y$를 먼저 수행하는게 보상금을 덜 지불한다고 해보자

`-` 그럼 $y$랑 $x$를 바꾸는게 최적이 된다

`-` $D$는 어차피 $x$와 $y$를 만드는 동안 기다려야하니 $x, y$의 순서가 바뀌어도 상관 없다

`-` 초기 상태에서 $x$를 먼저하는건 $y, D$의 보상금을 지불하는 것이고 $y$를 먼저하는건 $x, D$의 보상금을 지불하는 것이다

`-` 그런데 $D$는 공통이므로 제외해도 된다

`-` 그런데 $y$를 먼저 수행하는게 보상금을 덜 지불하므로 이는 $x$의 보상금이 더 크다는 뜻이다

`-` 따라서 $y$를 먼저하는게 맞다

`-` 이를 모든 인접한 원소에 대해 원하는만큼 적용할 수 있는데 그러면 결과적으로 처음의 명제가 성립한다

`-` 정확히 말하자면 나머지 작업에 대한 보상금 지불이 아닌 다음 작업에 대한 보상금 지불이다

`-` 왜냐하면 $x, y, D$이든 $y, x, D$이든 $D$에 대한 보상금 지불은 공통이다

`-` 따라서 $x, y$가 나은지 $y, x$가 나은지만 판단하면 된다

`-` 처음에 오름차순으로 배치를 한 뒤 인접한 원소마다 순서를 바꾸는게 더 나은지 아닌지 판단하고 더 낫다면 순서를 바꾼다

`-` 이를 한 사이클 반복하면 가장 뒤에 배치된 작업은 고정된다

`-` 이를 $N-1$번 반복하면 된다

`-` $x, y, D$ 순서에서 $D$는 영향 안 받으니 $x, y$만 비교하는 것이다

`-` 왜 `exchange argument`인지 알 것 같다

`-` 시간 복잡도는 $O\left(N^2\right)$이다


```python
def solution():
    N = int(input())
    durations = [0]
    compensations = [0]
    for _ in range(N):
        t, s = map(int, input().split())
        durations.append(t)
        compensations.append(s)
    order = list(range(1, N + 1))
    for i in range(N - 1, 0, -1):
        for j in range(i):
            w1 = order[j]
            w2 = order[j + 1]
            if durations[w2] * compensations[w1] < durations[w1] * compensations[w2]:
                order[j], order[j + 1] = order[j + 1], order[j]
    print(*order)


solution()

# input
# 4
# 3 4
# 1 1000
# 2 2
# 5 5
```

     4
     3 4
     1 1000
     2 2
     5 5
    

    2 1 3 4
    

## 소가 길을 건너간 이유 4

- 문제 출처: [백준 14464번](https://www.acmicpc.net/problem/14464)

`-` 단계별로 풀어보기 중 그리디에 있는 문제이다

`-` 특별한 조건이 주어진 이분 매칭이라고 한다 (이분 매칭이 뭔데)

`-` $j$번 소가 $i$번 닭의 도움을 받아 건너려면 $A_j \le T_i \le B_j$여야 한다

`-` $1$마리의 소만 도움 가능하고 소는 $1$번만 도움받을 수 있다

`-` 즉 일대일 매칭이다

`-` 최상의 결과는 모든 닭이 도움을 주는 것이며 이때의 정답은 $C$이다

`-` 각 닭에 대해 도움을 줄 수 있는 소를 골라내고 그 중 한 마리한테 도움을 주면 된다

`-` $T$를 오름차순 정렬하고 $A$ 기준으로 오름차순 정렬하자

`-` $A_j \le T_i$일 때까지 pop을 하자 (소를 도울 수 있는 여지가 있다)

`-` 여기서 $T_i \le B_j$이면 합격이고 아니면 버리자 

`-` 해당 소에게 도움을 줄 수 있는 닭은 없다

`-` $T$를 기준으로 오름차순 정렬했으므로 진행함에 따라 $T$가 더 커진다

`-` 해당 $T$를 $x$라 할 때 $x$가 도움을 줄 수 있으면 현재의 $T$도 도움 가능 ($T <= x$)

`-` 따라서 어떤 소를 도운다면 $B$가 가장 작은 소를 도우는게 이득이다

`-` 기준을 충족하는 소를 $B$를 기준으로 최소 힙에 넣자

`-` 그리고 최솟값을 pop하자

`-` 다음 닭에 대해 가능한 소를 pop해서 조건 만족하면 heap에 넣고 최솟값을 pop하자

`-` 모든 닭에 대해 반복하면 된다

`-` 이거 그냥 [보석 도둑](https://www.acmicpc.net/problem/1202)과 유사한 문제이다

`-` 시간 복잡도는 $O(N\log N + C\log C)$이다

`-` 소는 내림차순 정렬하고 끝에서 pop하자

`-` 계속 틀려서 반례 보고 풀었다


```python
import heapq


def solution():
    C, N = map(int, input().split())
    chickens = sorted([int(input()) for _ in range(C)])
    cows = [list(map(int, input().split())) for _ in range(N)]
    cows.sort(key=lambda x: (-x[0], -x[1]))
    heap = []
    count = 0
    for t in chickens:
        while cows:
            a, b = cows[-1]
            if b < t:
                cows.pop()
                continue
            if a <= t:
                cows.pop()
                heapq.heappush(heap, b)
            else:
                break
        while heap:
            b = heapq.heappop(heap)
            if t <= b:
                count += 1
                break
    print(count)


solution()

# input
# 5 4
# 7
# 8
# 6
# 2
# 9
# 2 5
# 4 9
# 0 3
# 8 13
```

     5 4
     7
     8
     6
     2
     9
     2 5
     4 9
     0 3
     8 13
    

    3
    

## 큰 수 만들기

- 문제 출처: [백준 16496번](https://www.acmicpc.net/problem/16496)

`-` `exchage argument` 문제이다

`-` [구두 수선공](https://www.acmicpc.net/problem/14908) 문제를 해결했다면 이 문제도 풀 수 있다

`-` 나열된 숫자 $a b D$를 고려하자 ($D$는 나열된 숫자 그룹)

`-` $ab$가 $ba$보다 커야 정답이다

`-` 이것이 모든 인접한 원소에 대해 성립하면 정답이다

`-` 만약 이게 아니라고 해보자

`-` 즉 $a b D$가 정답인데 $ba$가 $ab$보다 크다 해보자

`-` $abD$나 $baD$나 $a,b$의 자릿수 합은 같으므로 $D$에는 영향을 안준다 (애초에 $D$가 일의 자리부터 시작이라 자릿수 합 달라도 영향 안준다)

`-` $ab$가 더 크므로 $abD$가 $baD$보다 크다

`-` 이를 인접한 원소에 대해 반복적으로 적용하면 된다

`-` 큰 원소를 앞으로 보내자

`-` 한 사이클 돌리면 맨 앞의 원소는 고정이다

`-` 이를 $N-1$번 반복하면 최적의 값이다 

`-` 정답이 $0$으로 시작하면 앞에 있는 $0$을 제거하자

`-` 단, 모든 원소가 $0$이면 $0$이 정답이다

`-` 원소의 최댓값은 $10^9$이니 자릿수를 $d$라 하면 $d$는 최대 $10$이다

`-` 두 원소의 크기 비교를 위해 문자를 결합하는데 $O(d)$이고 이걸 한 사이클 당 $O(N)$번 반복한다

`-` 총 사이클은 $N-1$번이므로 전체 알고리즘의 시간 복잡도는 $O\left(dN^2\right)$이다


```python
def compare(left, right):
    s1 = int(left + right)
    s2 = int(right + left)
    return s2 > s1


def solution():
    N = int(input())
    array = list(input().split())
    for i in range(N - 1):
        for j in range(N - 1, 0, -1):
            left, right = array[j - 1], array[j]
            if compare(left, right):
                array[j], array[j - 1] = array[j - 1], array[j]
    while array and array[0] == "0":
        array.pop(0)
    if not array:
        print(0)
        return
    print("".join(array))


solution()

# input
# 5
# 3 30 34 5 9
```

     5
     3 30 34 5 9
    

    9534330
    

## 소방서의 고민

- 문제 출처: [백준 2180번](https://www.acmicpc.net/problem/2180)

`-` [구두 수선공](https://www.acmicpc.net/problem/14908), [큰 수 만들기](https://www.acmicpc.net/problem/16496) 문제와 같은 `exchage argument` 문제이다

`-` $x y D$ 배치라면 그룹 $D$의 입장에선 $x y$이든 $y x$이든 $t$는 동일하다

`-` $x y$와 $y x$ 중 $at + b$가 더 작은 결과를 사용해야 된다

`-` $x y \to a_yb_x+b_y$, $y x \to a_xb_y+b_x$

`-` 최종적으로 순서를 정한 뒤 시간을 합산하면 되며 이는 $O(n)$의 시간 복잡도를 가진다

`-` 먼저 처리해도 되는 걸 앞으로 계속 보내겠다

`-` 이걸 $n-1$번 반복하면 정렬된다

`-` 그럼 순서를 정하는데 $O\left(n^2\right)$이다

`-` 생각해보니까 $n \le 200000$이므로 $O\left(n^2\right)$ 알고리즘은 시간 초과이다

`-` 내가 원하는건 정렬이다

`-` 굳이 정렬을 $O\left(n^2\right)$에 할 필요 없이 $O(n\log n)$ 알고리즘 사용해서 하면 된다

`-` 내장 함수 쓰면 되고 정렬 기준만 내가 만든 compare 함수를 사용하면 된다

`-` 일반적인 수 정렬은 수의 크기 비교지만 여기선 다른 대소관계를 정의했다 (비교 대상에 따라 크기가 달라짐)

`-` 만약 $b$가 $0$이라면 $at + b =0$이므로 해당 화재를 처음에 진압해서 손해 없이 다른 화재 현장으로 이동할 수 있다


```python
from functools import cmp_to_key


def compare(accident_left, accident_right):
    a_left, b_left = accident_left
    a_right, b_right = accident_right
    t_left_right = b_left + a_right * b_left + b_right
    t_right_left = b_right + a_left * b_right + b_left
    if t_left_right < t_right_left:
        return -1
    if t_left_right > t_right_left:
        return 1
    return 0
    

def accumulate_time(accidents):
    t = 0
    for a, b in accidents:
        t += a * t + b
        t %= MOD
    return t


def solution():
    global MOD
    n = int(input())
    MOD = 40000
    accidents = []
    for _ in range(n):
        a, b = map(int, input().split())
        if b == 0:
            continue
        accidents.append([a, b])
    accidents.sort(key=cmp_to_key(compare))
    total_time = accumulate_time(accidents)
    print(total_time)


solution()

# input
# 3
# 2 0
# 1 2
# 0 3
```

     3
     2 0
     1 2
     0 3
    

    5
    

## 쿼리와 쿼리

- 문제 출처: [백준 17082번](https://www.acmicpc.net/problem/17082)

`-` $L > R$이면 나머지 쿼리 놀이와 상관없이 최댓값은 $10^9$으로 고정이라 이런 상황을 최대한 피해야 된다

`-` 종이에 적힌 수 $M$개 중 최댓값은 $M$개 쿼리 놀이에서 선택된 범위를 모두 합친 후 해당 영역에서 계산한 최댓값과 같다

`-` 범위를 최대로 줄이기 위해 수열 $l$과 $r$을 오름차순 정렬하고 동일한 인덱스의 $l_i$와 $r_i$를 쿼리 게임으로 고르면 된다

`-` 어차피 모든 $l_i$에 대해 $r_j$를 매칭해야 된다

`-` $l_i$에 $r_j$를 매칭했는데 $r_j$보다 작은 $r_k$가 존재하면 $r_j$ 대신 $r_k$를 매칭해야 범위가 줄어들 수 있어서 이득이다

`-` 이제 $a_i$에서 고려할 인덱스 $i$가 고정된다

`-` 최대힙에 원소랑 인덱스랑 같이 넣는다 

`-` 두 원소를 교환할 때마다 교환된 결과를 최대힙에 넣는다

`-` 최댓값을 판단할 때 힙의 최댓값의 인덱스와 원소가 현재 수열에서 일치하는지 판단한다

`-` 일치하면 괜찮고 아니라면 pop한다 

`-` 일치할 때까지 반복한다

`-` 인덱스 고르는게 문제인게 단순하게 하면 최악의 경우 $O(NM)$이다

`-` 분리집합으로 관리하면 될 듯하다

`-` 아니다, 굳이 분리집합 쓸 필요없이 스위핑으로 하면 될 것 같다

`-` 가장 작은 첫 번째 $l_1$과 $r_1$부터 고르게 된다

`-` 그 다음 $l_2$에 대해 $l_2 > r_1$이면 $[l_2, r_2]$는 더 이상 $[l_1, r_1]$과 겹칠 수 없다

`-` 만약 $l_2 \le r_1$이면 오른쪽 인덱스를 $r_1$에서 $r_2$로 바꾸면 된다

`-` 즉, $r_1 + 1$부터 $r_2$까지 범위에 추가하면 된다

`-` 최종적으로 선택된 범위만 고려하면 된다 (나머지는 필요없다)

`-` 범위 선택은 $O(N+M)$이고 범위 안에 있는 원소를 힙에 추가하는 건 $O(N\log N)$이다

`-` 참고로 범위 선택 중 $L > R$인 경우가 나오면 모든 쿼리에서 최댓값은 $10^9$으로 고정된다 (원소만 바꿀 수 있지 인덱스를 바꿀 수는 없다)


```python
import heapq


def sweeping(left, right):
    indices = []
    has_inversion = False
    if left[0] > right[0]:
        has_inversion = True
        return indices, has_inversion
    for j in range(left[0], right[0] + 1):
        indices.append(j)
    for i, (l, r) in enumerate(zip(left[1:], right[1:]), start=1):
        if l > r:
            has_inversion = True
            break
        if l > right[i - 1]:
            start = l
        else:
            start = right[i - 1] + 1
        for j in range(start, r + 1):
            indices.append(j)
    return indices, has_inversion


def query(array, i, j, indices, max_heap):
    array[i], array[j] = array[j], array[i]
    if i in indices:
        heapq.heappush(max_heap, (-array[i], i))
    if j in indices:
        heapq.heappush(max_heap, (-array[j], j))
    while array[max_heap[0][INDEX]] != -max_heap[0][VALUE]:
        heapq.heappop(max_heap)
    print(-max_heap[0][VALUE])


def solution():
    global VALUE, INDEX
    N, M, Q = map(int, input().split())
    VALUE, INDEX = 0, 1
    array = list(map(int, input().split()))
    left = sorted(map(lambda x: int(x) - 1, input().split()))
    right = sorted(map(lambda x: int(x) - 1, input().split()))
    indices, has_inversion = sweeping(left, right)
    if has_inversion:
        for _ in range(Q):
            print(10**9)
        return
    max_heap = []
    for i in indices:
        heapq.heappush(max_heap, (-array[i], i))
    indices = set(indices)
    for _ in range(Q):
        i, j = map(lambda x: int(x) - 1, input().split())
        query(array, i, j, indices, max_heap)


solution()

# input
# 5 2 3
# -2 0 1 2 -1
# 1 2
# 4 2
# 2 3
# 4 5
# 1 5
```

     5 2 3
     -2 0 1 2 -1
     1 2
     4 2
     2 3
    

    2
    

     4 5
    

    1
    

     1 5
    

    2
    

## 숫자의 신

- 문제 출처: [백준 1422번](https://www.acmicpc.net/problem/1422)

`-` 사용할 $N$개의 수 중 $K$개는 문제 조건에 따라 배열의 수를 하나씩 사용해야 한다

`-` 나머지 $N - K$개로 어떤 수를 뽑아서 사용할지 생각해보자

`-` 일단 자릿수가 커야 좋다

`-` 자릿수가 동일하다고 해보자

`-` 그럼 수가 더 클수록 좋다

`-` 동일한 자릿수의 수들인 $x,y$에 대해 $x > y$라고 하자

`-` $y$를 사용하여 만든 수에서 $y$를 $x$로 대체하면 기존보다 더 큰 수를 만들 수 있으므로 $x$를 사용해야 한다

`-` 자릿수가 크면서 수의 값도 큰 것은 단순히 수의 값이 큰 것과 동일하다

`-` 배열에서 가장 큰 수를 $N - K + 1$개 사용하고 배열의 나머지 $K - 1$개의 수를 하나씩 사용하여 가장 큰 수를 만들어보자

`-` 눈치챘겠지만 `exchange argument` 문제이다

`-` 두 정수형 문자 $a,b$에 대해 $a + b$와 $b + a$중 정수로 변환했을 때 더 큰 값의 순서를 따라야 한다

`-` 즉, $a + b$를 정수로 변환한 것이 $b + a$를 정수로 변환한 것보다 크다면 $a$가 $b$보다 앞서야 한다   

`-` 정렬을 수행한 뒤 이들을 순서대로 연결하면 가장 큰 수를 얻을 수 있다


```python
from functools import cmp_to_key


def compare(a, b):
    ab = int(a + b)
    ba = int(b + a)
    if ab > ba:
        return 1
    if ab < ba:
        return -1
    return 0


def solution():
    K, N = map(int, input().split())
    array = [input() for _ in range(K)]
    array.extend([max(array, key=int)] * (N - K))
    array.sort(key=cmp_to_key(compare), reverse=True)
    max_ = int("".join(array))
    print(max_)


solution()

# input
# 3 4
# 1
# 10
# 100
```

     3 4
     1
     10
     100
    

    110100100
    

## 모닝커피 (Large)

- 문제 출처: [백준 12456번](https://www.acmicpc.net/problem/12456)

`-` [보석 도둑](https://www.acmicpc.net/problem/1202) 문제의 심화 버전이다

`-` 태그 보고 풀었다

`-` exchange argument 문제만 풀어서 그런지 여기에 매몰되서 풀이를 떠올리지 못했다

`-` 단순하게 생각하면 만족도가 높은 커피만 골라 마시면 될 것 같다

`-` 하지만 유통기한이 지난 커피는 못 마시므로 이를 고려해야 한다

`-` 굳이 유통기한이 긴 커피를 먼저 마실 필요가 없다

`-` $K$일에 코코아가 얻을수 있는 만족도 합계의 최대를 구해야 하므로 그 이후는 필요가 없다

`-` 따라서 유통기한이 $K$일을 넘는 커피에 대해 유통기한을 $K$로 변경하자

`-` 유통기한을 기준으로 내림차순으로 정렬하자

`-` 첫 번째 원소 $(s_1, c_1, t_1)$과 두 번째 원소 $(s_2, c_2, t_2)$를 고려하자

`-` 정렬에 의해 $t_1 \ge t_2$인데 일단 $t_1 > t_2$라고 해보자

`-` $t_1$은 가장 긴 유통기한이며 $t_2+1$일부터 $t_1$일까진 마실 수 있는 커피가 $1$번 커피뿐이다 (다른 커피는 유통기한이 지나서 못 마신다)

`-` 따라서 $t_2+1$일부터 $t_1$일까지 $1$번 커피를 $\min(c_1, t_1 - t_2)$잔 마실 수 있다

`-` 커피가 남았을 수도 있고 안 남았을 수도 있다

`-` 만약 $t_1=t_2$라면 어떻까?

`-` 그럼 유통기한이 $t_1$이 아닐 때까지 원소를 계속 확장하자

`-` $t_1=t_2 > t_3$라면 $t_3+1$일부터 $t_1$일까지 $1$번 또는 $2$번 커피를 마실 수 있다

`-` 이때 만족도가 높은 커피로 마셔야 한다

`-` 만족도가 낮은 커피를 마시는게 정답이라고 가정하자

`-` 만족도가 높은 커피를 마셨으면 만족도를 더 높일 수 있으니 모순이다

`-` 만약 만족도가 높은 커피를 초반에 마시는 걸로 배치했다면 이는 커피가 많아서 남는다는 것이다

`-` 그럼 바뀐 영역에 대해 재귀적으로 위의 논리를 적용하면 된다

`-` 이제 내용을 정리 해보자

`-` 유통기한이 $0$일인 더미 커피를 만들어두자

`-` 유통기한을 기준으로 내림차순 정렬하자

`-` 높은 만족도를 뽑아내기 위해 우선순위 큐를 사용할 것이다 (힙을 사용하자)

`-` 힙이 비어있으면 현재 가리키는 포인터의 원소를 힙에 넣자 (처음엔 첫 번째 원소를 가리킨다)

`-` 그렇지 않다면 $t_1$보다 작은 유통기한을 가진 커피가 나올 때까지 포인터를 오른쪽으로 한 칸씩 옮기며 힙에 넣자

`-` $t_1$보다 작은 유통기한을 $t_i$라고 하자

`-` 그럼 $t_i + 1$일부터 $t_1$일까진 힙에 들어있는 커피 중에서만 마실 수 있다

`-` 만족도가 높은 커피 $j$를 힙에서 꺼내자

`-` 그럼 커피를 $\min(c_j, t_1 - t_i)$잔 마실 수 있다

`-` 만약 $c_j$가 더 작다면 힙에 남아있는 커피 중 $c_j - t_1 + t_i$잔을 더 마실 수 있다

`-` 힙이 비어있지 않는 선에서 커피를 $c_j - t_1 + t_i$잔 더 마시자

`-` 만약 $c_j$가 더 크거나 같다면 마실 수 있는 커피가 $0$잔이므로 끝이다

`-` 이때 힙에 들어있는 원소 $(s_j, c_j, t_j)$는 $t_1 - t_i$잔 마신 상태이므로 $(s_j, c_j, t_j-t_1+t_i)$로 변경하자

`-` 이제 $i$번 커피를 힙에 넣고 위의 논리를 반복하면 된다

`-` 커피 배열의 마지막 원소까지 순회를 마쳤으면 여태까지 얻은 만족도가 정답이 된다

`-` 시간 복잡도는 $O(TN \log N)$이다 ($T$는 테스트 케이스의 개수)


```python
import heapq


def maximize_score(coffees):
    heap = []
    score = 0
    for coffee in coffees:
        if not heap or end_date == coffee[EXP]:
            heapq.heappush(heap, coffee)
            end_date = coffee[EXP]
            continue
        start_date = coffee[EXP]
        duration = end_date - start_date
        while heap and duration > 0:
            s, c, t = heapq.heappop(heap)
            d = min(c, duration)
            score -= d * s
            duration -= d
            if c <= d:
                continue
            heapq.heappush(heap, (s, c - d, t))
        heapq.heappush(heap, coffee)
        end_date = start_date
    return score


def solve_testcase(i):
    global EXP
    N, K = map(int, input().split())
    EXP = 2  # 유통기한
    coffees = [(0, 0, 0)]
    for _ in range(N):
        c, t, s = map(int, input().split())
        t = min(K, t)
        coffees.append((-s, c, t))
    coffees.sort(key=lambda x: -x[2])
    max_score = maximize_score(coffees)
    print(f"Case #{i}: {max_score}")


def solution():
    T = int(input())
    for i in range(1, T + 1):
        solve_testcase(i)


solution()

# input
# 3
# 2 3
# 2 2 2
# 3 3 1
# 2 3
# 1 3 2
# 1 3 1
# 5 5
# 5 5 1
# 4 4 2
# 3 3 3
# 2 2 4
# 1 1 5
```

     3
     2 3
     2 2 2
     3 3 1
    

    Case #1: 5
    

     2 3
     1 3 2
     1 3 1
    

    Case #2: 3
    

     5 5
     5 5 1
     4 4 2
     3 3 3
     2 2 4
     1 1 5
    

    Case #3: 15
    

## I Am Knowledge

- 문제 출처: [백준 28068](https://www.acmicpc.net/problem/28068)

`-` 문제 태그는 그리디와 정렬이다 (exchange argument인 듯?)

`-` 만약 $a_k = 0$이라면 무조건 먼저 읽는게 맞다

`-` 어차피 전부 읽어야 되는데 즐거움 손해를 보지 않고 책을 읽을 수 있고 추가적인 즐거움도 얻을 수 있다

`-` 그 다음엔 $b_k \ge a_k$인 책들을 고려하고 $a_k$를 기준으로 오름차순 정렬하자

`-` 앞의 책을 못 읽으면 뒤에 있는 나머지 책들도 못 읽는다 (소모하는 즐거움이 뒤로 갈수록 더 많기 때문이다)

`-` 이제 $a_k > b_k$인 책들을 읽어야 된다

`-` 책을 읽을수록 즐거움이 감소하므로 어떤 순서로 책을 읽을지가 중요하다

`-` 무지성으로 $a_k$가 큰 책을 먼저 읽으면 안 된다

`-` $R$을 지금 가지고 있는 즐거움의 수치라고 할 때 $R = 10000, A = (9000, 0), B = (2000, 1900)$라고 해보자

`-` 책을 읽는 순서로 $B \to A$는 가능하지만 $A\to B$는 불가능하다

`-` 또한 무지성으로 $b_k - a_k$가 큰 걸 먼저 읽으면 안 된다

`-` $R = 10000, A = (9000, 3000), B = (2000, 0)$이라면 $A \to B$는 가능하지만 $B\to A$는 불가능하다

`-` 기준을 뭘로 해야 될까?

`-` $A \to B$이든 $B \to A$이든 남게되는 즐거움을 동일하므로 이후에 읽는 책에 영향을 주지 않는다

`-` $A \to B$가 가능하려면 $R + b_A - a_A \ge a_B$여야 된다

`-` $B \to A$가 가능하려면 $R + b_B - a_B \ge a_A$여야 된다

`-` 이때 $R' = R + b_A - a_A + b_B - a_B$라고 하면 다음과 같이 표현할 수 있다

`-` $A \to B$가 가능하려면 $R'- b_B\ge 0$이어야 하고 $B \to A$가 가능하려면 $R' -b_A \ge 0$이어야 한다

`-` 이때 $R'$은 상수이므로 $b_k$가 큰 책을 먼저 읽으면 된다

`-` 모든 책을 읽는 과정을 $3$단계로 구분할 수 있다

`-` $1$단계: $a_k = 0$를 만족하는 책

`-` $O(N)$에 처리 가능하다

`-` $2$단계: $b_k  \ge a_k$를 만족하는 책

`-` $O(N \log N)$에 처리 가능하다

`-` $3$단계: $b_k < a_k$를 만족하는 책 

`-` $O(N \log N)$에 처리 가능하다

`-` 따라서 전체 알고리즘의 시간 복잡도는 $O(N \log N)$이다

`-` 참고로 $1$단계는 $2$단계에 포함시켜서 처리해도 된다 ($a_k=0$이면 $b_k \ge a_k$이다)


```python
def greedy(a_smaller, b_smaller):
    energy = 0
    for a, b in [*a_smaller, *b_smaller]:
        if a > energy:
            return 0
        energy += b - a
    return 1


def solution():
    N = int(input())
    a_smaller = []
    b_smaller = []
    for _ in range(N):
        a, b = map(int, input().split())
        if b >= a:
            a_smaller.append((a, b))
        else:
            b_smaller.append((a, b))
    a_smaller.sort()
    b_smaller.sort(key=lambda x: -x[1])
    answer = greedy(a_smaller, b_smaller)
    print(answer)


solution()

# input
# 3
# 1 3
# 0 2
# 1 1
```

     3
     1 3
     0 2
     1 1
    

    1
    

## Swap Space

- 문제 출처: [백준 12776번](https://www.acmicpc.net/problem/12776)

`-` [I Am Knowledge](https://www.acmicpc.net/problem/28068) 문제와 거의 같은 문제이다

`-` $a \le b$인 파일들을 먼저 리포맷한 후 $a > b$인 파일들을 리포맷하자

`-` $a \le b$인 파일 중 $a$가 가장 작은 것부터 리포맷하고 다 했으면 $a > b$인 파일 중 $b$가 가장 큰 것부터 리포맷하자

`-` 만약 리포맷할 때 용량이 부족하다면 차이만큼 용량을 추가로 구매하면 된다

`-` 정렬 기준이 다르기에 $a \le b$인 파일들과 $a > b$인 파일들을 분리하여 처리하는 걸 생각하는 것이 중요하다


```python
def greedy(a_smaller, b_smaller):
    free_space = 0
    extra_space = 0
    for a, b in [*a_smaller, *b_smaller]:
        required_space = max(a - free_space, 0)
        extra_space += required_space
        free_space += required_space - a + b
    return extra_space


def solution():
    N = int(input())
    a_smaller = []
    b_smaller = []
    for _ in range(N):
        a, b = map(int, input().split())
        if b >= a:
            a_smaller.append((a, b))
        else:
            b_smaller.append((a, b))
    a_smaller.sort()
    b_smaller.sort(key=lambda x: -x[1])
    answer = greedy(a_smaller, b_smaller)
    print(answer)


solution()

# input
# 4
# 2 2
# 3 3
# 5 1
# 5 10
```

     4
     2 2
     3 3
     5 1
     5 10
    

    5
    

## 정수

- 문제 출처: [백준 1040번](https://www.acmicpc.net/problem/1040)

`-` Digit DP 문제라고 한다

`-` $N$보다 크거나 같은 수 중에서 $K$개의 서로 다른 숫자로 이루어진 수 중 가장 작은 수를 출력하면 된다

`-` 일단 정답은 최소 $K$자리이다

`-` 만약 $d(N) < K$라면 자릿수를 늘려야한다 ($d(N)$은 $N$의 자릿수를 나타낸다)

`-` 서로 다른 숫자 $K$개로 이루어진 $K$자리의 수 중 최솟값을 출력하자

`-` 이 경우 `"1023456789"[:K]`가 정답이 된다

`-` 이제 $d(N) \ge K$라고 하자

`-` 이론상 최적의 정답은 $N$이다

`-` 아니라면 값을 키워야한다

`-` 이때 가능한 값을 작게 키워야 하니 일의 자리의 수를 $9$까지 키우자

`-` 어떤 수 $x$에 대하여 서로 다른 숫자의 개수를 $R$이라고 하자

`-` 값을 키우면서 $R=K$인지 확인하자

`-` 만약 $R=K$라면 그때의 $x$가 정답이다

`-` 만약 일의 자리의 수를 $9$까지 키웠는데도 $R=K$인 상황이 없었다면 십의 자리의 수를 키워야한다

`-` 이때 십의 자리의 수를 키우면 일의 자리의 수는 마음대로 설정할 수 있다

`-` $R=K$라면 일의 자리의 수를 이미 사용한 숫자 중 가장 작은 걸로 하면 정답이다

`-` $R=K-1$이면 일의 자리의 수를 사용하지 않은 숫자 중 최솟값으로 하자 (없을 수 없다)

`-` 아니라면 십의 자리의 수를 $9$까지 키우자

`-` 그래도 안되면 백의 자리의 수를 $9$까지 키우자

`-` $R=K$라면 십의 자리의 수를 사용한 숫자 중 최솟값으로 설정하자

`-` 그러면 이는 십의 자리를 조작하는 상황에서 $R=K$인 경우와 같다 (재귀 함수를 실행하는 것이라 생각해도 좋다)

`-` $R=K-1$이면 십의 자리에 $0$을 넣고 재귀 함수를 실행하자

`-` $R=K-2$라면 사용안 한 숫자 중 최솟값을 이어붙이고 재귀 함수를 실행하자

`-` 이를 일반화 해보자

`-` 내 마음대로 조작할 수 있는 자릿수의 개수를 $U$라 할 때 $R < K < R + U$이면 $0$을 넣고 재귀 함수를 실행하면 된다 (단, $U$는 하나 줆)

`-` 조작할 수 있는 자릿수가 줄거나 늘어남에 따라 $U$를 변경해줘야 한다

`-` $R = K$이고 $U=0$이면 현재 수인 $x$를 반환하면 된다

`-` $R=K$이고 $U > 0$이면 사용한 숫자 중 최솟값을 이어붙이고 재귀 함수를 실행하자

`-` $R+U = K$이고 $U>0$이면 사용하지 않은 숫자 중 최솟값을 이어붙이고 재귀 함수를 실행하자 

`-` $R > K$ 또는 $R + U < K$라면 현재 조작하고 있는 자릿수의 값을 키우거나 이보다 한 단계 더 큰 자릿수를 조작해야 한다

`-` 현재 조작하고 있는 자릿수의 값이 $9$인 경우 이보다 큰 자릿수의 값을 키워야 한다

`-` 그런데 그 값도 $9$인 경우 단순하게 처리할 수 없다

`-` 이런 경우 $x+1$을 수행한 뒤 마지막 자릿수를 버리면 손쉽게 수행할 수 있다

`-` 이때 $U$는 기존보다 $1$ 증가한다

`-` 정수를 문자열로 바꾸는 건 $O\left(\log^2 N\right)$이고 재귀 횟수는 $O\left(\log N\right)$이므로 전체 알고리즘의 시간 복잡도는 $O\left(\log^3 N\right)$이다

`-` 계속 틀려서 반례 봤다

`-` 조건문을 이상하게 구현해서 무한 루프에 빠졌다

`-` 고쳤더니 정답이다! (일반화된 로직을 구현했기에 $d(N) < K$여도 상관없다)

`-` Digit DP 문제인데 나는 그리디로 풀었다


```python
from collections import Counter


def mex(bitmask):
    for i in range(10):
        i = str(i)
        if i not in bitmask:
            return i


def recursive(x, U, K):
    bitmask = Counter(x)
    R = len(bitmask)
    if R == K and U == 0:
        return x
    if R == K:
        return recursive(x + min(bitmask), U - 1, K)
    if R < K < R + U:
        return recursive(x + "0", U - 1, K)
    if R == K - U:
        return recursive(x + mex(bitmask), U - 1, K)
    last = x[-1]
    if last != "9":
        return recursive(x[:-1] + str(int(last) + 1), U, K)
    return recursive(str(int(x) + 1)[:-1], U + 1, K)


def solution():
    N, K = map(int, input().split())
    N = str(N)
    d = len(N)
    answer = recursive(N, 0, K)
    print(answer)


solution()

# input
# 886459817958609774 4
```

     886459817958609774 4
    

    886460000000000000
    

## 라면 사기 (Small)

- 문제 출처: [백준 18185번](https://www.acmicpc.net/problem/18185)

`-` $10$개월 전에 풀다 포기한 문제를 다시 풀어보자 (질문 게시판도 봤는데 못풀었음)

`-` 티어에 비해 쉬운 그리디 문제라고 하는데 나한테는 어렵다...

`-` $1\sim N$번 공장에서 최소의 비용으로 라면을 구매해야 한다

`-` $1 \sim i-1$번 공장에선 라면을 모두 구매했고 이제 $i$번 공장에서 라면을 구매해야 한다고 해보자

`-` $i$번 공장에서 라면을 $A_i$개 구매할 때 $1,2,3$번의 방법을 사용할 수 있다

`-` 뭐가 됐든 총 $A_i$개의 라면을 구매해야 한다

`-` 방법 $2,3$에 의해 $i$번 공장에서 라면을 구매할 때 $i+1,i+2$번 공장도 같이 고려할 수 있어야 한다

`-` $n = N - i + 1$일 때 $n=1$이면 $A_i$개의 라면은 방법 $1$로만 구매할 수 있다

`-` $n=2$라면 $\min(A_i, A_{i+1})$개의 라면은 방법 $2$로 구매하고 남은 개수는 방법 $1$로 구매하는 게 이득이다

`-` $n=3$이라면 $\min(A_i, A_{i+1}, A_{i+2})$개의 라면은 방법 $3$으로 구매하자

`-` 이때 $A_{i+1}=0$이 됐다면 남은 라면은 방법 $1$로만 구매할 수 있다

`-` 그렇지 않으면 $i,i+1$번 공장에서 방법 $2$로 라면을 최대로 구매한 뒤 남은 라면을 방법 $1$로 구매하자

`-` 이제 $n > 3$이라고 하고 이를 일반화하자

`-` 만약 $A_i \ge A_{i+1}$이라면 $A_i - A_{i+1}$개의 라면은 방법 $1$로만 구매할 수 있다

`-` 나머지 $A_{i+1}$개의 라면은 $i,i+1$번 공장에서 방법 $2$로 구매하는 게 각 공장에서 방법 $1$로 구매하는 것보다 이득이다

`-` 이때 $A_{i+2} \ge A_{i+1}$이라면 $i,i+1,i+2$번 공장에서 $A_{i+1}$개의 라면을 방법 $3$으로 구매하는 게 더 이득이다

`-` 근데 방법 $2$만 사용하고 $i+2$번 공장은 추후에 다른 공장과 연계해서 활용하는 게 이득이라 생각할 수도 있다

`-` 만약 $i+2$번 공장의 $A_{i+2}$개의 라면을 방법 $1$로만 구매하면 손해이다

`-` 만약 $i+2$번 공장과 $i+3$번 공장에서 $A_{i+2}$개의 라면을 방법 $2$로 구매하면 기존과 비용 차이가 없다

`-` 방법 $3$도 마찬가지이다

`-` $A_{i+2} < A_{i+1}$이라면 $A_{i+2}$개의 라면은 방법 $3$으로 구매하고 $A_{i+1} - A_{i+2}$개의 라면은 방법 $2$로 구매하자

`-` 이제 $A_i < A_{i+1}$라고 해보자

`-` 단순히 $\min(A_i, A_{i+1}, A_{i+2})$개의 라면을 $i,i+1,i+2$번 공장에서 방법 $3$으로 구매하고 시작할 수도 있다

`-` 하지만 그러면 안 되는게 $A=\{1,2,1,1\}$ 같은 경우에서 최소 비용을 보장하지 못한다 

`-` 만약 $A_{i+1} > A_{i+2}$라면 일단 $\min(A_i, A_{i+1} - A_{i+2})$개의 라면을 $i,i+1$번 공장에서 방법 $2$로 구매하자

`-` 연속된 공장 $4$곳에서 라면을 $1$개씩 사야할 때 방법 $3$과 방법 $1$로 한 번씩 구매하면 비용이 $10$원이 들고 방법 $2$로 두 번 구매해도 비용이 $10$원이 든다

`-` 비용이 동일하니 방법 $2$로 구매한 뒤 다른 방법으로 라면을 살 수 있을지 판단하는 것이다 ($i+2$번 공장의 라면은 건들지 않았기에 확장성이 더 있다)

`-` 그러고 나면 $i,i+1,i+2$번 공장에서 가능한 만큼 방법 $3$으로 구매하면 된다

`-` 엄밀한 증명은 아니다 (애초에 증명이 맞긴 할까?)

`-` 전체 알고리즘의 시간 복잡도는 $O(N)$이다


```python
def minimize_cost(array):
    cost = 0
    for i, a_i in enumerate(array):
        if a_i == 0:
            continue
        cost += buy_ramyeon(array, i)
    return cost


def buy_ramyeon(array, i):
    n = len(array) - i
    if n == 1:
        cost = 3 * array[i]
        array[i] = 0
        return cost
    if n == 2:
        m = min(array[i], array[i + 1])
        cost = 5 * m + 3 * (array[i] - m)
        array[i] = 0
        array[i + 1] -= m
        return cost
    if n == 3:
        m1 = min(array[i], array[i + 1], array[i + 2])
        m2 = min(array[i], array[i + 1]) - m1
        cost = 7 * m1 + 5 * m2 + 3 * (array[i] - m1 - m2)
        array[i] = 0
        array[i + 1] -= m1 + m2
        array[i + 2] -= m1
        return cost
    if array[i] >= array[i + 1]:
        m = min(array[i + 1], array[i + 2])
        cost = 3 * (array[i] - array[i + 1]) + 7 * m + 5 * (array[i + 1] - m)
        array[i] = 0
        array[i + 1] = 0
        array[i + 2] -= m
        return cost
    cost = 0
    if array[i + 1] > array[i + 2]:
        m = min(array[i], array[i + 1] - array[i + 2])
        array[i] -= m
        array[i + 1] -= m
        cost += 5 * m
    m = min(array[i], array[i + 1], array[i + 2])
    cost += 7 * m + 5 * (array[i] - m)
    array[i + 1] -= array[i]
    array[i] = 0 
    array[i + 2] -= m
    return cost


def solution():
    N = int(input())
    array = list(map(int, input().split()))
    min_cost = minimize_cost(array)
    print(min_cost)


solution()

# input
# 6
# 4 5 6 1 9 5
```

     6
     4 5 6 1 9 5
    

    74
    

`-` 아름다운 증명은 [여기](https://youngyojun.github.io/contest/review/2020/02/15/iamcoder-2019-yearend-contest/)를 참고하자 (진짜 아름다움)

## 라면 사기 (Large)

- 문제 출처: [백준 18186번](https://www.acmicpc.net/problem/18186)

`-` [라면 사기 (Small)](https://www.acmicpc.net/problem/18185) 문제와 달리 $N$의 제한이 더 크고 라면 구매 방법의 비용이 가변적이다

`-` 만약 $C \ge B$라면 모든 공장에서 라면을 $1$번 방법으로 사는 게 이득이다

`-` 만약 $C < B$라면 [라면 사기 (Small)](https://www.acmicpc.net/problem/18185) 문제와 동일하게 해결할 수 있다

`-` 어차피 알고리즘의 시간 복잡도는 $O(N)$이므로 $N$이 최대 $10^6$이어도 제한 시간 안에 동작한다

`-` 날먹 개꿀!


```python
def minimize_cost(array):
    cost = 0
    a = B
    b = B + C
    c = B + 2 * C
    for i, a_i in enumerate(array):
        if a_i == 0:
            continue
        cost += buy_ramyeon(array, i, a, b, c)
    return cost


def buy_ramyeon(array, i, a, b, c):
    n = len(array) - i
    if n == 1:
        cost = a * array[i]
        array[i] = 0
        return cost
    if n == 2:
        m = min(array[i], array[i + 1])
        cost = b * m + a * (array[i] - m)
        array[i] = 0
        array[i + 1] -= m
        return cost
    if n == 3:
        m1 = min(array[i], array[i + 1], array[i + 2])
        m2 = min(array[i], array[i + 1]) - m1
        cost = c * m1 + b * m2 + a * (array[i] - m1 - m2)
        array[i] = 0
        array[i + 1] -= m1 + m2
        array[i + 2] -= m1
        return cost
    if array[i] >= array[i + 1]:
        m = min(array[i + 1], array[i + 2])
        cost = a * (array[i] - array[i + 1]) + c * m + b * (array[i + 1] - m)
        array[i] = 0
        array[i + 1] = 0
        array[i + 2] -= m
        return cost
    cost = 0
    if array[i + 1] > array[i + 2]:
        m = min(array[i], array[i + 1] - array[i + 2])
        array[i] -= m
        array[i + 1] -= m
        cost += b * m
    m = min(array[i], array[i + 1], array[i + 2])
    cost += c * m + b * (array[i] - m)
    array[i + 1] -= array[i]
    array[i] = 0 
    array[i + 2] -= m
    return cost


def solution():
    global B, C
    N, B, C = map(int, input().split())
    array = list(map(int, input().split()))
    if C >= B:
        print(B * sum(array))
        return
    min_cost = minimize_cost(array)
    print(min_cost)


solution()

# input
# 5 3 2
# 1 1 1 0 2
```

     5 3 2
     1 1 1 0 2
    

    13
    


```python

```
