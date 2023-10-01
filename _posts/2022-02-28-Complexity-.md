---
layout: post
title: 복잡도
subtitle: Big-O, Time Complexity, Space Complexity
categories: 알고리즘
tags: [Algorithm, Complexity]
---

## 복잡도
알고리즘이란 어떤 작업을 수행하기 위해 입력을 받아 원하는 출력을 만들어내는 과정을 기술한 것으로, 어떤 목적을 달성하거나 결과물을 만들어내기 위해 거쳐야 하는 일련의 과정들을 의미한다.  
예를들어, 서울에서 부산까지 이동한다 했을 때, 이동 루트는 굉장히 다양한데 자동차 네비게이션 알고리즘의 경우 **시간 복잡도가 가장 낮은 알고리즘을 선택하여** 출발지와 목적지 두 지점 간의 최단 경로나 최단 시간이 걸리는 경로를 제공한다.  

- 알고리즘의 실행시간
    - 입력값의 크기에 대한 함수
        - 입력값의 크기에 따른 알고리즘의 실행 시간은 입력값의 크기가 늘어나면 실행 시간도 늘어난다. 예를들어, GPS가 길을 찾아야 할 때 모든 골목길을 제외하고 고속도로 길만 찾는다면 찾는 시간이 단축될 것이다.
    - 실행시간의 성장률 (rate of growth)
        - 입력값의 크기에 따라 이 함수가 얼마나 빨리 커지는지 알아보는 것 
        - 이 때 중요하지 않은 상수와 계수들을 제거하면 성장률에 집중할 수 있는데 이것을 점근적 표기법 (Asymptotic notation)이라 부른다.
            - 점근적 : 가장 큰 영향을 주는 항만 계산하겠다.

- 점근적 표기법은 다음 세가지로, 시간복잡도를 나타내는 데 사용된다.
    - 오메가 표기법 (Big-Ω Notation) : **최상의 경우**
    - 세타 표기법 (Big-θ Notation) : **평균의 경우**
    - 빅오 표기법 (Big-O Notation) : **최악의 경우**

    평균인 세타 표기법은 평가하기가 까다로워 사용을 잘 안하고, 최악의 경우인 빅오 표기법을 사용한다.

## 빅오 노테이션 (Big-O Notation)
빅오표기법은 불필요한 연산을 제거하여 알고리즘 분석을 쉽게 할 목적으로 사용된다.  
다음 규칙을 사용하여 복잡도를 빅오표기법으로 표기할 수 있다.  
- 숫자는 다 빼기
- 가장 증가율이 높은 수식만 남기기

Big-O로 측정되는 복잡성에는 시간과 공간이 있는데
- 시간복잡도는 입력된 N의 크기에 따라 실행되는 조작의 수를 나타낸다.
- 공간복잡도는 알고리즘이 실행될 때 사용하는 메모리의 양을 나타낸다.
    - 현재는 메모리의 발전으로 중요도가 낮아짐.

예를들어 함수의 복잡도가 다음과 같을 때,
- `f(n) = 4` : n에 어떠한 값이 들어와도 도출되는 결과는 4로 이 경우 시간 복잡도는 O(1)
- `f(n) = 2n + 5n + 3` : 가장 증가율만 높은 수식만 남기고, 숫자는 다 빼버리면 시간복잡도는 O(n) 
- `f(n) = 3n² + n + 5` : 가장 증가율만 높은 수식만 남기고, 숫자는 다 빼버리면 시간복잡도는 O(n²)
- `f(n) = 4n + log(n) + 3` : 마찬가지로 O(n)

코드를 통해 시간 및 공간 복잡도를 알아보자.
-   ``` java
    boolean isFirstTwoMatch(int[] elements) {
        return elements[0] == elements[1]
    }
    ```
    - 시간복잡도 : O(1), n의 크기가 변동되어도 실행되는 횟수는 똑같음.
    - 공간복잡도 : O(1), 메모리의 양도 늘어나지 않음(새 배열을 만든다거나 하지 않음).  
    
-   ``` java
    int sum(int[] elements) {
        int sum = 0;
        for (int number : elements) {
            sum += number;
        }
        return sum;
    }
    ```
    - 시간복잡도 : O(n), n의 크기가 늘어나면 for문 실행 횟수도 늘어남.
    - 공간복잡도 : O(1), n의 크기가 늘어나도 새 메모리 공간을 쓰지 않음.

-   ```java
    int factorial(int number) {
        if (number <= 2) {
            return number;
        }

        return number * factorial(number -1);
    }
    ```
    - 시간복잡도 : O(n), n의 크기가 늘어나면 재귀횟수가 늘어남.
    - 공간복잡도 : O(n), 재귀함수는 n만큼 수행되어 변수 n이(number) n개만큼 만들어짐.

-   ```java
    int findNumberIndex(int numberToFind, int[] sortedNumbers) {
        int low = 0;
        int high = sortedNumbers.length -1;
        int index = 0;

        while (low <= high) {
            int mid = (low + high) / 2;
            int midNumber = sortedNumbers[mid];

            if (midNumber < numberToFind) {
                low = mid + 1;
            } else if (midNumber > numberToFind) {
                high = mid + 1;
            } else if (midNumber == numberToFind) {
                index = mid;
                break;
            }
        }
        return index;
    }
    ```
    - 시간복잡도 : O(logN), 한번 while문 돌때마다 순회할 숫자가 반으로 줄어듬.
    - 공간복잡도 : O(1), n이 늘어난다해서 메모리 양이 변화하지 않음.

### 문제
> numbers라는 int형 배열이 있다. 해당 배열에 들어있는 숫자들은 오직 한 숫자를 제외하고는 모두 두 번씩 들어있다. 오직 한 번만 등장하는 숫자를 찾는 코드를 작성하라.

1. List 활용
``` java
private int solution1(int[] numbers) {
    List<Integer> numberList = new ArrayList<>();
    for (int number : numbers) {
        if (numberList.contains(number)) {
            // remove(Object), remove(int)-> 인덱스 제거
            numberList.remove((Integer) number);  
        } else {
            numberList.add(number);
        }
    }

    return numberList.get(0);
}
```  

- numbers를 순회하면서 numberList에 값이 있으면 제거하고 값이 없으면 추가하는 방식으로 for문을 돌면 한 번만 등장하는 숫자를 찾을 수 있다.
- 시간복잡도 : `O(n) * O(n) = O(n²)`, List의 contains는 내부적으로 for문을 한번 더 도는 형태이기 때문에 저 코드에서 실제로는 이중 for문을 수행하게된다.
- 공간복잡도 : `O(n)`

2. HashMap 활용
```java
private int solution2(int[] numbers) {
    HashMap<Integer, Integer> numbersMap = new HashMap<>();

    for (int num : numbers) {
        // getOrDefault : num과 매핑되는 값을 가져오고 없으면 defaultValue를 반환
        numbersMap.put(num, numbersMap.getOrDefault(num, 0) + 1);
    }

    for (int num : numbers) {
        if (numbersMap.get(num) == 1) {
            return num;
        }
    }

    return 0;
}
```

- HashMap을 만들어 배열에 들어있는 숫자를 키에 넣고, 값으로는 숫자의 등장 횟수를 저장하고, 숫자의 등장 횟수가 1인 키 값을 찾아서 리턴하는 방법이다.
- 시간복잡도 : `O(n) + O(n) = O(2n) = O(n)`, for문 두번으로 O(2n) 이지만 숫자는 제외하고 최종 시간 복잡도는 `O(n)`
- 공간복잡도 : `O(n)`

3. 비트연산자 활용
```java
private int solution3(int[] numbers) {
    int uniqueNumber = 0;

    for (int num : numbers) {
        uniqueNumber ^= num;
    }

    return uniqueNumber;
}
```

- `XOR` 연산자(`^`)를 사용하는 방법으로 비트 연산자는 두 비트가 서로 다를 때 1을 반환하며 연산 순서는 상관이 없는 것을 이용한 방법이다.
    - `5 ^ 0 = 101 ^ 000 = 101 => 5`
    - `1 ^ 5 ^ 1 = (1 ^ 1) ^ 5 = 0 ^ 5 => 5`
- 시간복잡도 : O(n), numbers의 크기에 따라 계산 횟수가 증가
- 공간복잡도 : O(1), numbers의 크기에 상관 없이 사용하는 메모리는 일정.