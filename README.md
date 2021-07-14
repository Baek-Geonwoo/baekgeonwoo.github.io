# baekgeonwoo.github.io
# 2021 07 07

오늘은 재귀 알고리즘에 대해서 배웠습니다. a부터 b까지 더하는 함수를 만드는데 함수 이름을 sum이라 하고 sum(a,b)에서 return sum(a,b-1)+b 형식으로 재귀를 하나만 사용하는 방법과
sum(a,b)에서 m = (a+b)//2 라고 한 뒤 sum(a,m)+sum(m+!,b) 하는 방법 두가지를 배웠는데 두 가지 방법 모두 시간복잡도는 Big-O 방식으로 O(n)이었습니다.
그리고 재귀는 return 부분 즉, 재귀함수를 호충하는 부분과 재귀를 끝내는 부분이 중요하다는 것을 배웠습니다.
그 다음 백준문제 1769번을 풀었는데 숫자로 이루어진 문자열을 받아서 3의 배수인지 확인하는 문제였습니다. 문자열을 받아서 1자리가 될때까지 각 자리수를 더한 뒤
이러한 변환을 한 횟수와 3의 배수인지 아닌지를 출력하는 문제였습니다. 처음에는 문자열만을 함수의 매개변수로 사용해서 try-finally 문을 사용하여 두 결과의 순서가 반대로 나와서 고생했지만
재귀함수의 매개변수로 변환횟수를 추가하여 해결할 수 있었습니다. 총 5번 재출만에 통과했습니다. 아래는 제가 작성한 문제코드입니다.
def rec(string, count):
    if len(string) > 1:
        a = map(int, list(string))
        s = 0
        for n in a:
            s += n
        return rec(str(s), count+1)
    else:
        return int(string), count
string = input()
s, c = rec(string, 0)
print(c)
if s%3==0:
    print("YES")
else:
    print("NO")

# 2021 07 14

오늘은 quick select 알고리즘에 대해서 공부했습니다. 선택문제는 특수한 경우와 일반적인 경우로 나눌 수 있는데 특수한 경우는 n개의 값 중에서 최댓값과 최솟값을 구하는 경우와 두번째로 작은값, 큰값을 구하는 것입니다. 이 경우 n-1번 비교를 하여 최댓값, 최솟값을 구할 수 있고, 다시 n-2번 비교하여 그 다음 작거나 큰 값을 구할 수 있습니다. 하지만 이 경우 시간이 오래 걸리고 특수한 경우이기 때문에 일반적이지 않다는 단점이 있습니다. 그 다음으로 일반적인 경우는 n개의 값 중에서 k번째로 작은 값, 큰 값을 구하는 경우인데 k번째로 큰 값을 구하는 경우 작은 값을 구하는 경우와 반대로 하면 됩니다. 먼저 n개의 값들이 L이라는 리스트에 있다고 하고, pivot이라고 기준이 되는 값 p를 정하고(아무거나 정해도 되는데 저는 맨 왼쪽에 있는 값으로 정했습니다) 이 값보다 작은 값들은 A라는 리스트에, p와 같은 값들은 M이라는 리스트에, p보다 큰 값들은 B라는 리스트를 만들어서 넣습니다. 그 다음 k가 A의 크기보다 작거나 같으면 A안에 k번째로 작은 수가 있는 것이므로 재귀적으로 L에 A를 넣고 계속 진행합니다. k가 len(A)+len(M)보다 큰 경우 k번째로 작은 값은 B에 있는 것이므로 재귀적으로 L을 B로 하여 같은 과정을 진행합니다. 앞의 두 경우가 아니면 k번쨰로 값은 M에 있는 것이므로 p를 반환합니다.
저는 n개의 값 중에서 k번쨰로 작은 값을 구하는 문제를 풀었는데 문제의 첫 번째 줄에 n과 k가 주어지고 두 번째 줄에 n개의 값들이 주어집니다. 앞에서 설명한 알고리즘을 코드로 구현하여 문제를 해결했고, 아래는 그 코드입니다.
def quick_select(L,k):
	p = L[0]
	A, B, M = [], [], []
	for x in L:
		if p>x: A.append(x)
		elif p<x: B.append(x)
		else: M.append(x)
	if len(A) >= k: return quick_select(A,k)
	elif len(A) + len(M) < k: return quick_select(B, k-len(A)-len(M))
	else: return p

n, k = [int(e) for e in input().split()]
L = [int(e) for e in input().split()]
print(quick_select(L,k))
