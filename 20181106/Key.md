1.
略


2.
略


3.
```c
#include <stdio.h>
const int MOD = 100007;
int main() {
	int N;
	int fib1 = 1, fib2 = 1, fib = 1;
	int i;
	scanf("%d", &N);
	for (i = 3; i <= N; ++i) {
		fib = (fib1 + fib2) % MOD;
		fib1 = fib2;
		fib2 = fib;
	}
	printf("%d", fib);
	return 0;
}
```
设走上第N级台阶的走法数为f(N)。我们知道，走上第N级台阶只有两种可能，要么是从第N−1级台阶跨1级，要么是从第N−2级台阶跨2级。所以得出： `f(N)=f(N−1)+f(N−2),N≥3`

可以看出，这一公式正是斐波那契数列的形式，可以轻易写出。

但需要注意一点，1≤N≤1000，当N稍大时，f(N)会超出int所能表示的范围，而这也正是题目中要求输出走法数目对100007取余后的结果的原因。所以我们不能直接求出f(N)后，再对100007取余；而应当在迭代的过程中就做取余的运算。而取余具有如下的性质： `(a+b) % c =((a % c)+(b % c)) % c`


4.
```c
#include <stdio.h>
const int MOD = 100007;
int main() {
	int N, K;
	int i, j;
	scanf("%d%d", &N, &K);
	N--;
	int fib[1001] = {1, 1};
	for (i = 1; i <= K; ++i)
		fib[i] = 1;
	for (i = 2; i <= N; ++i) {
		for (j = i - 1; j >= i - K && j > 0; --j)
			fib[i] = (fib[i] + fib[j]) % MOD;
	}
	printf("%d", fib[N]);
	return 0;
}
```
此题类似与第3题，但又是对第3题的扩展。同样，设走上第N级台阶的走法数为f(N)。我们知道，走上第N级台阶只有K种可能：从第N−1级台阶跨1级，从第N−2级台阶跨2级，…，从第N−K级台阶跨K级。那么很容易得出： `f(N)=Σf(N−i)，1<=i<=K`


5.
```c
#include <stdio.h>
int main() {
	int n;
	int i;
	int res = 2;
	scanf("%d", &n);
	for (i = 2; i <= n; ++i)
		res = 2 * res + 2;
	printf("%d", res);
	return 0;
}
```
对于这题，如果理解了汉诺塔的递归思想，应当很容易解出。

设f(n)为2n个原盘从A柱移动到C柱所需的最小移动次数。这个过程可分为三步：首先，将A柱上面的2(n−1)个原盘从A柱移动到B柱，这一步所需的最小移动次数为f(n−1)；然后，将A柱下面剩余的2个原盘依次移动到C柱，这一步所需要的最小移动次数为2；最后，将B柱的2(n−1)个原盘从B柱移动到C柱，这一步所需的最小移动次数为f(n−1)。故，可推出： `f(n)=2f(n−1)+2`


6.
```c
#include <stdio.h>
int moveN_HanoiFromSrcToDest(const char* src, const char* mid,
                             const char* dest, int n) {
	int num = 0;
	if (n == 0)
		return 0;
	num += moveN_HanoiFromSrcToDest(src, mid, dest, n - 1);
	printf("Move %d from %s to %s\n", n, src, mid);
	++num;
	num += moveN_HanoiFromSrcToDest(dest, mid, src, n - 1);
	printf("Move %d from %s to %s\n", n, mid, dest);
	++num;
	num += moveN_HanoiFromSrcToDest(src, mid, dest, n - 1);
	return num;
}
int main() {
	int n;
	scanf("%d", &n);
	printf("%d", moveN_HanoiFromSrcToDest("A", "B", "C", n));
	return 0;
}
```
对于这题，如果理解了汉诺塔的递归思想，应当很容易解出。

设f(n)为n个原盘从A柱移动到C柱所需的步骤数。想将n个原盘从A柱移动到C柱，这个过程可分为5步：首先，将A柱上面的n−1个原盘从A柱移动到C柱，这一步所需的移动步数为f(n−1)；然后，将A柱最下面的原盘从A柱移动到B柱，这一步所需的移动步数为1；然后，将C柱的n−1个原盘从C柱移动到A柱，这一步所需的移动步数为f(n−1)；然后，将B柱的1个原盘从B柱移动到C柱，这一步所需的移动步数为1；最后，将A柱的n−1个原盘从A柱移动到C柱，这一步所需的移动步数为f(n−1)。

函数moveN_HanoiFromSrcToDest接受四个参数，分别是src，mid，dest和n，表示将n个原盘从名为src的柱子移动到名为dest的柱子，而名为mid的柱子，则是移动过程中可供临时暂存原盘的柱子。该函数的功能是，输出n个原盘从名为src的柱子移动到名为dest的柱子的步骤，并返回移动的步骤数。

我在移动的过程中顺便求了所需的步骤数。也可以根据递推公式，直接算出步骤数： `f(n)=3f(n−1)+2`


7.
```c
int BinomialCoefficient(int n, int k) {
	if (k > n / 2)
		k = n - k;
	if (k == 0 || n == 1)
		return 1;
	if (k == n)
		return 1;
	return BinomialCoefficient(n - 1, k - 1) +
	       BinomialCoefficient(n - 1, k);
}
```
在宏观上把握递归的思想，会更容易理解递归。
