---
title: Other Algorithms
categories:
  - algorithm
tags:
  - Algorithm
date: 2019-10-13T13:00:00+09:00
toc: false
use_math: true
---

## 피보나치 수열

### 재귀방식  -  $O(2^n)$

재귀는 연속 함수 호출로 인한 스택 오버플로우가 발생할 가능성이 높다.

```java
public int recurFibo(int i) {
    if (i <= 1) {
        return i;
    } else {
        return recurFibo(i - 2) + recurFibo(i - 1);
    }
}
```

### DP 방식  -  $O(n)$

```java
public int getDpFibo(int num) {
    int[] fiboDpArray = new int[num + 1];
    
    fiboDpArray[0] = 0;
    fiboDpArray[1] = 1;
    
    if (num > 1) {
        for (int i = 2; i <= num; ++i) {
            fiboDpArray[i] = fiboDpArray[i - 2] + fiboDpArray[i - 1];
        }
    }
}
```

### 반복문 방식  -  $O(n)$

```java
public int getLoopFibo(int num) {
    if (num <= 1) {
        return 1;
    } else {
        int a = 0;
        int b = 1;
        int sum = 0;
        for (int i = 2; i <= num; ++i) {
            sum = a + b;
            a = b;
            b = sum;
        }
        return sum;
    }
}
```



## 10진수를 n진수로 변환

```java
public String toChange(int value, int i) {
    String result = "";
    
    while (value != 0) {
        if ((value % i) < 10) {
            result = (value % i) + result;
            value /= i;
        } else {
            int temp = (char)((value % i) + 55); // 65 : A
            result = Integer.toString(temp) + result;
        }
    }
    return result;
}
```



## Quick Selection

k번째로 큰 수를 구할 때 $O(n)$의 시간복잡도로 구할 수 있는, 퀵 소트의 원리를 응용한 방법이다. 불필요한 부분의 정렬을 줄임으로써 전체를 정렬해서 k번째로 큰 수를 구하는것 보다 훨씬 빠르게 구할 수 있다.

```java
public int quickSelection(int[] nums, int left, int right, int k) {
    int pivot = partition(nums, left, right);
    
    if (pivot == k) return nums[pivot];
    else if (pivot > k) quickSelection(nums, left, pivot - 1, k);
    else if (pivot < k) quickSelection(nums, pivot - 1, right, k);
}

public int partition(int[] nums, int left, int right) {
    int pivot = nums[left];
    int i = left + 1;
    int j = right;
    
    while (i <= j) {
        while (nums[i] <= pivot && i <= right) i++;
        while (nums[j] >= pivot && j >= (left + 1)) j--;
        if (i <= j) swap(nums, i, j);
    }
    swap(nums, left, j);
    return j;
}
```

## 중위표기법을 후위표기법으로

1. 피연산자(a,b,c)는 출력한다. 
2. 연산자는 앞 연산자(스택의 맨 위)를 살펴서 출력하거나 대기한다(스택에 넣는다, 대기 된 자료들은 나중에 대기 된 연산자가 먼저 나온다, LIFO, 스택을 이용) 
3. 연산자의 대기(스택에 push)여부는 연산자간의 우선순위에 따른다.

스택에는 연산자만 사용하고, 피연산자는 바로바로 출력한다.

먼저, 스택의 자료를 위해 연산자에 우선순위를 둔다. '*', '/', '+', '-', '(', ')'

스택의 top보다 다음 연산자의 우선순위가 클 경우 push 하고, 그렇지 않을 경우, pop한다. 

(우선순위가 같다면, 먼저 스택에 들어온 연산자를 먼저 내보내야하기 때문에 클 경우만 push를 한다.)

그리고, 알다시피 괄호는 우선적으로 처리해줘야하기 때문에, 괄호가 닫히면 이전 괄호까지 pop을 해준다.

마지막으로 쌓인 스택을 비워주면 끝이다.

```java
private void solve() {
    char[] s = sc.readLine().toCharArray();
    int len = s.length;
 
    Stack<Character> stack = new Stack<Character>();
    StringBuilder sb = new StringBuilder();
 
    for (int i = 0; i < len; i++) {
        int p = priority(s[i]);
        char ch = s[i];
 
        switch (ch) {
            case '+':
            case '-':
            case '*':
            case '/':
                while (!stack.isEmpty() && priority(stack.peek()) >= p) {
                    sb.append(stack.pop());
                }
                stack.push(ch);
                break;
            case '(':
                stack.push(ch);
                break;
            case ')':
                while (!stack.isEmpty() && stack.peek() != '(') {
                    sb.append(stack.pop());
                }
                stack.pop();
                break;
            default:
                sb.append(ch);
        }
    }
 
    while (!stack.isEmpty()) {
        sb.append(stack.pop());
    }
 
    System.out.println(sb.toString());
}
 
public static int priority(char ch) {
    switch (ch) {
        case '*':
        case '/':
            return 2;
        case '+':
        case '-':
            return 1;
        case '(':
        case ')':
            return 0;
    }
    return -1;
}
```

