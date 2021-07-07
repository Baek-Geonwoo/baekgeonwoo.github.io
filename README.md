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
