# 정렬 알고리즘 성능 분석 with C

201901730 허동균

## 목차

- 개요
- 알고리즘 코드
- 알고리즘 수행시간 기록
- 알고리즘 성능 분석(그래프)
- 결론
- log₂(수행시간) 그래프 분석 및 결론

## 개요

수업시간에 배운 정렬알고리즘에 대한 성능 분석을 진행하였다.

선택정렬, 삽입정렬, 버블정렬, 쉘정렬, 힙정렬, 퀵정렬을 대상으로 선정하였다.

입력 데이터 수를 정하고 이에 따른 정렬 알고리즘 수행 시간을 측정했다.

그래프로 나타내고 알고리즘 성능 분석을 실시했다.

그래프와 수행시간은 직접 만들고 기록한 것이다.

c언어로 구현했으며, _c언어로 쉽게 풀어쓴 자료구조_ 교재를 참고했다.

## 알고리즘 코드

- 기본 헤더파일, SWAP, 배열 정의, 시간 측정 위한 변수 선언, 힙 정렬 구성요소 선언

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_SIZE 256

//데이터의 개수 지정
#define SWAP(x, y, t) ((t) = (x), (x) = (y), (y) = (t)) // SWAP함수 설정
int original[MAX_SIZE];                                 //랜덤함수로 만든 데이터를 저장할 원본 배열
int list[MAX_SIZE];                                     //각 정렬 알고리즘에서 사용할 데이터 배열
int n;                                                  //데이터의 개수를 받는 전역변수 설정
int arrry[MAX_SIZE];

clock_t start, finish, used_time = 0;
//실행 시간 측정을 위한 변수
void Heap_Sort(int[], int);
void Build_Max_Heap(int[], int);
void Max_Heapify(int[], int, int);
```

<br/>

- 선택정렬

```c
void selection_sort(int arrry[], int n)
{
    int i, j, least, tmp;

    printf("선택 정렬 중... ");
    for (i = 0; i < n - 1; i++)
    {
        least = i;
        for (j = i + 1; j < n; j++)
            if (arrry[j] < arrry[least])
                least = j;
        SWAP(arrry[i], arrry[least], tmp);
    }
}
```

<br/>

- 삽입정렬

```c
void insertion_sort(int arrry[], int n)
{
    int i, j, key;
    printf("삽입 정렬 중... ");
    for (i = 1; i < n; i++)
    {
        key = arrry[i];
        for (j = i - 1; j >= 0 && arrry[j] > key; j--)
            arrry[j + 1] = arrry[j];
        arrry[j + 1] = key;
    }
}
void inc_insertion_sort(int arrry[], int first, int last, int gap)
{
    int i, j, key;
    for (i = first + gap; i <= last; i = i + gap)
    {
        key = arrry[i];
        for (j = i - gap; j >= first && key < arrry[j]; j = j - gap)
            arrry[j + gap] = arrry[j];
        arrry[j + gap] = key;
    }
}
```

<br/>

- 버블정렬

```c
void bubble_sort(int arrry[], int n)
{
    int i, j, tmp;
    printf("버블 정렬 중... ");
    for (i = n - 1; i > 0; i--)
    {
        for (j = 0; j < i; j++)
            if (arrry[j] > arrry[j + 1])
                SWAP(arrry[j], arrry[j + 1], tmp);
    }
}
```

<br/>

- 쉘정렬

```c
void shell_sort(int arry[], int n)
{
    int i, gap;
    printf("쉘 정렬 중... ");
    for (gap = n / 2; gap > 0; gap = gap / 2)
    {
        if ((gap % 2) == 0)
            gap++;
        for (i = 0; i < gap; i++)
            inc_insertion_sort(arrry, i, n - 1, gap);
    }
}
```

<br/>

- 힙정렬

```c
void Heap_Sort(int arrry[], int n)
{

    int *temp;
    int i;
    printf("힙 정렬 중... ");
    Build_Max_Heap(arrry, n); // 먼저 힙을 만든다.
    for (i = n - 1; i >= 0; i--)
    {
        SWAP(arrry[0], arrry[i], temp); // 부모노드와 마지막 노드와 SWAP
        n--;                            // 부모노드를 삭제.
        Max_Heapify(arrry, 0, n);       // 힙 유지 실시.
    }
}

// Build_Max_Heap 함수
void Build_Max_Heap(int arrry[], int length)
{

    int parent_position;

    // 리프 노드를 제외한 맨 마지막 노드부터 시작.
    // 배열은 0부터 시작하므로 length/2에 -1을 해줘야함.
    for (parent_position = length / 2 - 1; parent_position >= 0; parent_position--)
    {
        Max_Heapify(arrry, parent_position, length); // 힙 유지 실시.
    }
}

// Max_heapify 함수
void Max_Heapify(int arrry[], int parent_position, int heap_size)
{

    int *temp;
    int left, right, largest;

    left = 2 * parent_position + 1;
    right = 2 * parent_position + 2;

    if ((left < heap_size) && (arrry[left] > arrry[parent_position])) // 왼쪽 자식 노드와 부모노드 비교.
        largest = left;
    else
        largest = parent_position;

    if ((right < heap_size) && (arrry[right] > arrry[largest])) // 오른쪽 자식노드와 이전에 얻은 제일 큰 노드 값과 비교.
        largest = right;

    if (largest != parent_position)
    {
        SWAP(arrry[parent_position], arrry[largest], temp); // 값이 큰 노드를 부모노드로 SWAP.
        Max_Heapify(arrry, largest, heap_size);             // 다시 부모노드를 대상으로 Heapify를 실시.
    }
}
```

<br/>

- 퀵정렬

```c
int partition(int arrry[], int left, int right)
{
    int pivot = arrry[left], tmp, low = left, high = right + 1;

    do
    {
        do
            low++;
        while (low <= right && arrry[low] < pivot);

        do
            high--;
        while (high >= left && arrry[high] > pivot);
        if (low < high)
            SWAP(arrry[low], arrry[high], tmp);
    } while (low < high);

    SWAP(arrry[left], arrry[high], tmp);
    return high;
}
void quick_sort(int arrry[], int left, int right)
{
    if (left < right)
    {
        int q = partition(arrry, left, right);
        quick_sort(arrry, left, q - 1);
        quick_sort(arrry, q + 1, right);
    }
}
```

<br/> 

- 실행시간을 측정 및 측정
```c
void CalcTime(void)
{
    used_time = finish - start;
    printf("완료!\n소요 시간 : %f sec\n\n", (float)used_time / CLOCKS_PER_SEC);
}
```

<br/>

- 원본 배열을 복사하는 함수

```c
void CopyArr(void)
{
    int i;
    for (i = 0; i < n; i++)
        list[i] = original[i];
}
```

<br/>

- 정렬된 데이터를 만드는 함수

```c
void sortedarry(void)
{
    int i;
    for (i = 0; i < n; i++)
    {
        for (int j = i; j < n; j++)
        {
            arrry[i] = list[j];
        }
    }
    arrry[i] = list[i];
}
```

<br/>

- 역 정렬 데이터를 만드는 함수

```c
void reverseArray(int *arrry, int n)
{
    int temp;

    for (int i = 0; i < n / 2; i++)
    {
        temp = arrry[i];
        arrry[i] = arrry[(n - 1) - i];
        arrry[(n - 1) - i] = temp;
    }
}
```

<br/>

- 랜덤 데이터인 경우 main 함수

```c
void main()
{
    int i;

    n = MAX_SIZE;
    for (i = 0; i < n; i++)
        original[i] = rand(); // 랜덤 수 배열

    printf("데이터의 개수 : %d\n\n", n);

    CopyArr();
    start = clock();
    selection_sort(list, n);
    finish = clock();
    CalcTime();

    CopyArr();
    start = clock();
    insertion_sort(list, n);
    finish = clock();
    CalcTime();

    CopyArr();
    start = clock();
    bubble_sort(list, n);
    finish = clock();
    CalcTime();

    CopyArr();
    start = clock();
    shell_sort(list, n);
    finish = clock();
    CalcTime();

    CopyArr();
    start = clock();
    printf("퀵 정렬 중... ");
    quick_sort(list, 0, n);
    finish = clock();
    CalcTime();


    CopyArr();
    start = clock();
    Heap_Sort(list, n);
    finish = clock();
    CalcTime();

}
```

<br/>

- 정렬 데이터인경우 main 함수(일부 생략)

```c
void main()
{
    int i;

    n = MAX_SIZE;
    for (i = 0; i < n; i++)
        original[i] = rand();

    printf("데이터의 개수 : %d\n\n", n);

    CopyArr(); // 정렬된 데이터를 만들기 위해 퀵정렬을 사용
    printf(" 정렬 중...... ");
    printf(" 완료! \n");
    printf("\n");
    quick_sort1(list, 0, n);

    printf("------- 정렬된 경우 -------\n");
    printf("\n");

    sortedarry(); // 정렬된 데이터 arrry배열
    start = clock();
    selection_sort(arrry, n);
    finish = clock();
    CalcTime();
    ...
}
```

<br/>

- 역 정렬 데이터인 경우 main 함수(일부 생략)

```
void main()
{
    int i;

    n = MAX_SIZE;
    for (i = 0; i < n; i++)
        original[i] = rand();

    printf("데이터의 개수 : %d\n\n", n);

    CopyArr();
    printf(" 정렬 중...... ");
    printf(" 완료! \n");
    printf("\n");
    quick_sort1(list, 0, n);

    printf("------- 역 정렬된 경우 -------\n");
    printf("\n");

    sortedarry();
    reverseArray(arrry, n); // 정렬된 데이터를 역 정렬
    start = clock();
    selection_sort(arrry, n);
    finish = clock();
    CalcTime();
    ...
}
```

## 알고리즘 수행시간 기록

- 입력 데이터 수 [2, 4, 8, 16, 32, ..., 1048576] (과제는 32부터였지만, 확인차 2부터 진행함)

- 시간은 평균값으로 기록하려 노력했다. (대략 10번씩 측정)

- 랜덤 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/exel_arrangement/random.png?raw=true" height="651px" width="777px"></p>

<br/>

- 정렬 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/exel_arrangement/sorted.png?raw=true" height="651px" width="777px"></p>
<br/>

- 역 정렬 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/exel_arrangement/reverse.png?raw=true" height="651px" width="777px"></p>
<br/>

## 알고리즘 분석

### 그래프

### 랜덤 데이터인 경우

<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/ramdom_case.png?raw=true" height="450px" width="800px"></p>
<br/>

- BEST 정렬 : 퀵정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/random_best_quick.png?raw=true" height="450px" width="800px"></p>
<br/>

- WORST 정렬 : 버블정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/random_worst_bubble.png?raw=true" height="450px" width="800px"></p>
<br/>

### 정렬 데이터인 경우

<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/sorted_case.png?raw=true" height="450px" width="800px"></p>
<br/>

- BEST 정렬 : 삽입정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/sorted_best_insert.png?raw=true" height="450px" width="800px"></p>
<br/>

- WORST 정렬 : 버블정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/sorted_worst_bubble.png?raw=true" height="450px" width="800px"></p>
<br/>

### 역 정렬 데이터인 경우

<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/reverse_case.png?raw=true" height="500px" width="895px"></p>
<br/>

- BEST 정렬 : 삽입정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/reverse_best_insert.png?raw=true" height="492px" width="895px"></p>
<br/>

- WORST 정렬 : 버블정렬
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/graph/reverse_worst_bubble.png?raw=true" height="492px" width="895px"></p>
<br/>

## 결론

- **랜덤 데이터일 경우에는 퀵정렬 알고리즘이 수행시간이 제일 짧았고, 버블 정렬이 제일 수행시간이 가장 길었다.**

- **정렬 데이터일 경우에는 삽입정렬 알고리즘이 수행시간이 제일 짧았고, 버블 정렬이 제일 수행시간이 가장 길었다.**

  삽입정렬 알고리즘 특성상 정렬된 데이터에서의 수행시간이 짧았고, 버블정렬은 정렬여부와 상관없이 인접한 두 원소를 비교하므로 수행시간은 가장 길었다.

- **역 정렬 데이터일 경우에는 삽입정렬 알고리즘이 수행시간이 제일 짧았고 버블 정렬이 제일 수행시간이 가장 길었다.**

  역순이지만 정렬되어 있으므로 삽입정렬 알고리즘이 가장 수행시간이 짧았으며, 버블정렬은 정렬여부와 상관없이 인접한 두 원소를 비교하므로 수행시간은 가장 길었다.

## 각 정렬 알고리즘의 시간복잡도

<br/>

> 1.  선택정렬 : O(n²)

> 2.  삽입정렬 : O(n²)

> 3.  버블정렬 : O(n²)

> 4.  쉘 정렬 : O(n^1.5)

> 5.  힙 정렬 : O(nlog₂n)

> 6.  퀵정렬 : O(nlog₂n)

<br/>

## 선택정렬, 삽입정렬, 버블정렬

- 시간복잡도가 O(n²)이므로 수행시간에 log₂를 취한 결과를 기록하고, 그래프를 만들어 보았다.

> 과연 선형적으로 나타날까?

<br/>

- 랜덤 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/log2/random_log2.png?raw=true" height="416px" width="954px"></p>

> 선형적으로 나타난다!
> <br/>

- 정렬된 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/log2/sorted_log2.png?raw=true" height="416px" width="954px"></p>

> 버블정렬, 선택정렬이 선형적으로 나타난다!

> 삽입정렬은 데이터양이 적은 경우에는 비선형적이다가 점차 선형적 모습을 띈다.
> <br/>

- 역 정렬된 데이터인 경우
<p align="center"><img src="https://github.com/donggyunhuh/Sort_Time/blob/master/log2/reverse_log2.png?raw=true" height="416px" width="954px"></p>

> 버블정렬, 선택정렬이 선형적으로 나타난다!

> 삽입정렬은 데이터양이 적은 경우에는 비선형적이다가 점차 선형적 모습을 띈다.
> <br/>

### 결론

- **수행시간에 log₂를 취한 결과를 나타내고, 그래프를 만들어 보았을때, 시간복잡도가 O(n²)이므로 그래프는 선형적 모습을 띈다!**
