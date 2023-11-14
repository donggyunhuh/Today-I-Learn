# 퀵 정렬 (Quick Sort)

퀵 정렬은 분할 정복 기법을 기반으로 한다. 

이 정렬 방식의 핵심은 '피벗(pivot)'이라는 기준 값을 선정하고, 이를 기준으로 리스트를 두 부분으로 나누는 것이다. 한 부분에는 피벗보다 작은 모든 요소를, 다른 부분에는 피벗보다 큰 모든 요소를 배치한다.

이 과정을 재귀적으로 반복하여 전체 리스트를 정렬하는 것이 목표다.

## 퀵 정렬의 기본 과정
1. **피벗 선택**: 리스트에서 하나의 요소를 피벗으로 선정한다. 보통 리스트의 첫 번째나 마지막 요소를 선택한다.

2. **분할**: 피벗을 기준으로 리스트를 두 부분으로 나눈다. 피벗보다 작은 모든 요소들은 피벗의 왼쪽에, 큰 모든 요소들은 오른쪽에 위치하도록 한다.

3. **재귀적 정렬**: 분할된 두 부분 리스트에 대해 같은 과정을 재귀적으로 반복한다.

## 자바로 구현한 퀵 정렬 코드
```java
public class QuickSort {
    // 퀵 정렬을 수행하는 메소드
    void quickSort(int arr[], int begin, int end) {
        if (begin < end) {
            int partitionIndex = partition(arr, begin, end);

            // 재귀적으로 서브 배열 정렬
            quickSort(arr, begin, partitionIndex-1);
            quickSort(arr, partitionIndex+1, end);
        }
    }

    // 분할을 수행하는 메소드
    private int partition(int arr[], int begin, int end) {
        int pivot = arr[end];
        int i = (begin-1);

        for (int j = begin; j < end; j++) {
            if (arr[j] <= pivot) {
                i++;

                int swapTemp = arr[i];
                arr[i] = arr[j];
                arr[j] = swapTemp;
            }
        }

        int swapTemp = arr[i+1];
        arr[i+1] = arr[end];
        arr[end] = swapTemp;

        return i+1;
    }

    // 배열을 출력하는 메소드
    static void printArray(int arr[]) {
        for (int i=0; i<arr.length; ++i)
            System.out.print(arr[i] + " ");
        System.out.println();
    }

    // 메인 메소드
    public static void main(String args[]) {
        int arr[] = {10, 7, 8, 9, 1, 5};
        int n = arr.length;

        QuickSort ob = new QuickSort();
        ob.quickSort(arr, 0, n-1);

        System.out.println("정렬된 배열: ");
        printArray(arr);
    }
}
```
이 코드는 기본적인 퀵 정렬 알고리즘을 자바로 구현한 것이다.

quickSort 메소드는 재귀적으로 리스트를 정렬하고, partition 메소드는 피벗을 기준으로 리스트를 두 부분으로 나눈다. 

main 메소드에서는 예제 배열을 생성하고, 정렬 과정을 실행한 후 결과를 출력한다.