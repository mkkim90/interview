# 접근법 

재귀적 해법은 말 그대로, 부분문제에 대한 해법을 통해 완성됩니다. 
주어진 문제를 부분 문제로 나누는 방법도 여러가지가 있습니다.
- 상향식 접근법 : bottom-up
- 하향식 접근법 : top-down
- 반반 접근법 : half-and-half

# 재귀적 해법 vs 순환적 해법

재귀적 알고리즘을 사용하면 공간 효율성이 나빠질 수 있다. 재귀 호출이 한 번 발생할 때마다 스택에 새로운 층이 추가해야 합니다.

재귀의 깊이가 n일때 O(n) 만큼의 메모리를 사용 -> 이런 이유로, 재귀적 알고리즘을 순환적으로 구현하는 것이 더 나을 수 있습니다.
하지만 때로는 순환적으로 구현된 코드가 더 복잡할 수 있으므로, 두 방법(재귀적해법, 순환적 해법) 사이의 타협점을 찾는 것이 필요합니다.

# 동적계획법 & 메모리제이션

동적프로그래밍은 재귀적 알고리즘과 반복적으로 호출되는 부분문제를 찾아내는 것이 관건입니다. 이를 찾은 뒤 나중을 위해 현재 결과를 캐시에 저장해 놓습니다.
하기 두가지 조건으로 동적프로그래밍을 고려해볼수 있습니다.
- 작은 문제가 반복이 일어나는 경우
- 같은 문제를 구할 때마다 정답이 같을 경우



동적프로그래밍의 간단한 예시 

`n 번째 피보나치 수를 찾는 겻`

### 재귀 (시간복잡도 `O(2^n)`)


```
int fibonacci(int I) {
	If ( i == 0 ) return 0;
	If ( i == 1 ) return 1;
	return fibonacci(i -1) + fibonacci(i-2);
}
```

### 하향식 동적 프로그래밍(메모이제이션) (시간복잡도 `O(n)`)
```
int fibonacci(int n) {
	return fibonacci(n, new int[n+1]);
}

int fibonacci(int i, int[] memo) {
	if ( i == 0 || i == 1 ) return i;
	if ( memo[i] == 0 ) {
		memo[i] = fibonacci(i-1, memo) + fibonacci(i-2, memo);
	}
	return memo[i];
}
```

### 상향식 동적 프로그래밍 (시간복잡도 `O(n)`)
```
int fibonacci(int n) {
	if ( n == 0 ) return 0;
	else if ( n == 1 ) return 1;
	int[] memo = new int[n];
	memo[0] = 0;
	memo[1] = 1;
	for ( int I = 2; i < n; I++ ) {
		memo[i] = memo[i-1] + memo[i-2];
	}
	return memo[n-1] + memo[n-2];
}
```

