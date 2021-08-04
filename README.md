# baekgeonwoo.github.io
# 모각코
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

# 2021 07 21

오늘은 MoM(Median of Medians) 알고리즘에 대해서 배우고 MoM 알고리즘의 수행시간 분석을 귀납법을 통해 해보았습니다. MoM 알고리즘은 quick select 알고리즘이 worst case일때 수행시간이 O(n^2)이 되는 것을 보완한 알고리즘으로 quick select 알고리즘에서 pivot을 랜덤하게 정해서 수행시간이 길어지는 문제를 pivot을 k번째 값을 찾는데 좀더 효율적이도록 고름으로써 수행시간을 줄인 알고리즘입니다. 먼저 값들을 5개씩 자른 뒤 5개씩 자른 값들의 중간값들의 중간값을 구합니다.(이래서 알고리즘 이름이 median of medians입니다.) 이 중간값을 m이라고 부르겠습니다. 즉, m이 pivot이 되는것입니다. m보다 작은 값들을 A라는 리스트에 m보다 큰 값들을 B라는 리스트에 저장한 뒤 quick select 알고리즘처럼 k번째 값을 구합니다. 이러면 값의 개수를 n이라고 했을 떄 A, B의 크기가 n/4이상 3n/4이하가 되어 A또는 B에 대부분의 값이 몰려 수행시간이 길어지는 경우를 방지할 수 있게 됩니다. 수행시간을 분석한 결과 MoM알고리즘의 값이 n개일 때의 수행시간을 T(n) 이라 하면 T(n) = T(3n/4) + T(n/5) + 11n/5라는 식을 구할 수 있었습니다. T(1) = 1이므로 x<n 일때 T(x) <= cx이 성립한다고 가정하면 T(n) = T(3n/4) + T(n/5) + 11n/5 <= 3cn/4 + cn/5 + 11n/5 <= cn 에서 44<=c 임을 구핤할 수 있으며 T(n) <= 44n이 1일때 성립하고 x<n일때 성립한다고 가정했을 때 x=n일때 성립하는 것이 증명되었으므로 T(n) <= 44n이 성립합니다. 이로써 값이 n개일때 MoM 알고리즘의 수행시간이 O(n)이라는 것을 증명할 수 있었습니다. 아래는 제가 구현한 MoM 알고리즘 코드입니다.

def find_median_five(A):
	n = len(A)
	if n in [1,2]:
		return A[0]
	elif n in [3,4]:
		A.remove(max(A))
		return max(A)
	else:
		if A[0] > A[1]:
			s1, l1 = A[1], A[0]
		else:
			s1, l1 = A[0], A[1]
		if A[2] > A[3]:
			s2, l2 = A[3], A[2]
		else:
			s2, l2 = A[2], A[3]
		r = A[4]
		if l1 > l2:
			if s1 > r:
				if s1 > l2:
					if l2 > r:
						return r
					else:
						return l2
				else:
					return s1
			else:
				if l2 > r:
					return r
				else:
					if l2 > s1:
						return l2
					else:
						return s1
		else:
			if s2 > r:
				if s2 > l1:
					if l1 > r:
						return r
					else:
						return l1
				else:
					return s2
			else:
				if l1 > r:
					return r
				else:
					if l1 > s2:
						return l1
					else:
						return s2
	
def MoM(A, k): # L의 값 중에서 k번째로 작은 수 리턴
	if len(A) == 1: # no more recursion
		return A[0]
	i = 0
	S, M, L, medians = [], [], [], []
	while i+4 < len(A):
		medians.append(find_median_five(A[i: i+5]))
		i += 5
		
	if i < len(A) and i+4 >= len(A): # 마지막 그룹으로 5개 미만의 값으로 구성
		medians.append(find_median_five(A[i:]))
	
	mom = MoM(medians, len(medians)//2)
	for v in A:
		if v < mom:
			S.append(v)
		elif v > mom:
			L.append(v)
		else:
			M.append(v)

	if len(S) >= k : return MoM(S, k)
	elif len(S) + len(M) < k: return MoM(L, k - len(S) - len(M))
	else: return mom

n, k = map(int, input().split())
A = list(map(int, input().split()))
print(MoM(A, k))

# 2021 07 28

오늘은 분할정복법과 분할정복법을 이용한 피보나치수 계산, 카라츠바 알고리즘에 대해서 배웠습니다. 분할정복법이란 큰 문제를 작은 문제로 분할해서 재귀적으로 해결하는 방법을 말합니다. 간단한 예시를 들면 리스트에서 최댓값을 찾을 때 리스트를 절반으로 잘라 2개의 리스트를 만들고 각각의 리스트의 최댓값을 구한 뒤 구한 값들을 비교하여 전체 리스트의 최댓값을 구하는 방법입니다. 이를 이용하여 n번째와 n-1번째 피보나치수를 구해보면 [fn fn-1]T = [1 1/1 0] [fn-1 fn-2]T인 것을 이용하여 [fn fn-1]T = [1 1/1 0]^n-1 [f1 f0]T 임을 구할 수 있고 n번의 행렬곱셈만으로 n번째와 n-1번째 피보나치수를 구할 수 있음을 배웠습니다. 마지막으로 카라츠바 알고리즘은 카라츠바라는 사람이 고안한 알고리즘으로 큰수의 곱셈을 할 때 수들을 절반씩 나누어(자리수 유지를 위해 나중에 10^n을 곱해줍니다. 여기서 n은 수의 길이의 절반) 수들을 u, v라 하면 u = 10^n*x + y, v = 10^n*z + w라 할 수 있고 u*v = x*z*10^2n + (x*w+y*z)*10^n + y*w = x*z*10^2n + ((x+y)(z+w)-x*z-y*w)*10^n + y*w임을 이용해서 곱셈의 쵯수를 x*z*10^2n + (x*w+y*z)*10^n + y*w = x*z*10^2n에서 x*z, x*w, y*z, y*w로 4번이었던 것을 x*z, (x+y)*(z+w), y*w로 3번으로 줄여 수행시간을 줄이는 방법입니다. 이런식으로 u또는 v의 길이가 1이 될 때까지 이 알고리즘을 재귀적으로 반복하면 O(n^2)에서 O(n^log23)(로그2의 3)로 수행시간을 줄일 수 있습니다.
아래는 제가 작성한 코드입니다.
def mult(u, v):
	n1, n2 = len(str(u)), len(str(v))
	if n1 == 1 or n2 == 1:
		return u*v
	n = min(n1,n2)//2
	x, y = int(str(u)[:-n]), int(str(u)[-n:])
	z, w = int(str(v)[:-n]), int(str(v)[-n:])
	xz, yw = mult(x,z), mult(y,w)
	return 10**(2*n)*xz + 10**n*(mult(x+y,z+w)-xz-yw) + yw
	
u = int(input())
v = int(input())
print(mult(u,v))

# 2021 08 04
이번에는 정렬 알고리즘에 대해서 배웠습니다. 기본적인 정렬 알고리즘(쉽고 느린)은 3종류가 있는데 selection, bubble 그리고 insertion이 있습니다. selection 정렬 알고리즘은 리스트에서 [:]에서 최댓값을 구해 맨 뒤에 넣고 [:-1]에서 최댓값을 구해 인덱스 -2에 넣는 것을 반복하는데 이 과정을 n-1번 반복함으로써 리스트를 정렬할 수 있습니다. 그 다음 bubble 정렬 알고리즘은 앞에서부터 2개씩 비교하는데 인덱스로 0과 1을 비교하여 0이 더 크면 1에 두고 그 다음 1과 2를 비교하여 큰 것을 뒤에 놓는 것을 반복하여 최종적으로 맨 뒤에 가장 큰 값을 넣고 [:-1]에서 이를 반복하고, [:-2] ... 식으로 이 과정을 n-1번 반복하여 리스트를 정렬합니다. insertion 정렬 알고리즘은 A라는 리스트가 있다고 하면 A[0]으로 새로운 리스트를 만든 뒤(이를 B라 합니다) A[1]과 B의 요소들을 비교하여 B에서 A[1]이 들어갈 위치를 B의 뒤에서부터 찾아 A[1]을 넣습니다. A[2], A[3]...에 대해서 이를 반복하면 A를 오름차순 정렬한 리스트가 B가 됩니다. 이 세 알고리즘은 모두 수행시간이 O(n^2)입니다.
그 다음으로 가장 빠른 정렬 알고리즘인 quick sort 알고리즘에 대해서 배웠습니다. worst case의 수행시간은 앞의 세 정렬알고리즘과 같이 O(n^2)이지만 평균적으로 가장 빠른 알고리즘입니다. quick sort 알고리즘은 앞서 배웠던 quick select알고리즘을 이용한 알고리즘인데 리스트 A가 있다고 하면 A[0]을 pivot으로 잡고 pivot p보다 작은 값들을 S에 같은 값들을 M에 큰 값들을 L에 넣고,  quick sort 알고리즘을 S와 L에 적용하고 M과 더하여 반환합니다. 즉, return quick_sort(S)+M+quick_sort(L)합니다. 그리고 만약 A의 크기가 1 이하면 A를 반환합니다. 이를 반복하면 A가 정렬됩니다.
아래는 실습겸 사용자로부터 리스트의 요소 개수 n을 받고 리스트에 들어갈 값을 n번 입력받아 리스트 L을 만들고 L을 quick sort 알고리즘으로 정렬하여 반환하는 코드를 작성한 것입니다.
def quick_sort(L):
	if len(L) <= 1:
		return L
	p = L[0]
	A, B, M = [], [], []
	for x in L:
		if p>x: A.append(x)
		elif p<x: B.append(x)
		else: M.append(x)
	return quick_sort(A) + M + quick_sort(B)

n = int(input())
L = []
for _ in range(n):
	L.append(int(input()))
print(quick_sort(L))

