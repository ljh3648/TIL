# 퀵 정렬
![퀵 정렬](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

---
백준 알고리즘 2751번을 문제를 풀면서 퀵 정렬에 대한 개념을 공부하게 되었다.

배열의 갯수가 1부터 최대1,000,000의 배열을 오름차순으로 정렬하는 문제였는데, 이 문제에서 시간 복잡도가 O(n log n)을 요구하였다. 내가 흔히 사용하는 정렬 방식은 버블정렬이였는데 버블정렬으로는 시간초과로 인해 오답으로 평가되기 때문에 이보다 빠른 방식의 정렬 방식을 채택해야 했다.

정렬 방식중 제일 효율적이고 빠른 정렬 방식이 퀵 정렬 방식이라는 것을 알고 있었고, C 기본 라이브러리에는 퀵 정렬 함수(qsort)가 있었기 때문에 이 방식으로 문제를 해결해 나갔는데, 이번 기회에 퀵 정렬에 대해서 분석하고 직접 구현할 수 있도록 공부하였다.

---
퀵 정렬은 분할정복 방법을 사용하여 정렬을 시도하는데, 분할 방식중 가장 대표적인 방식은 **로무토(Lomuto)** 방식과 **호어(Hoare)** 분할 방식이 있다.

## Lomuto 분할 방식

```c
int partition(int arr[], int low, int high) {
    int pivot = arr[high];    // pivot
    int i = (low - 1);  // Index of smaller element

    for (int j = low; j <= high - 1; j++) {
        // If current element is smaller than or
        // equal to pivot
        if (arr[j] <= pivot) {
            i++;    // increment index of smaller element
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}
```

로무토 방식의 정렬방법은 피봇은 전달받은 배열에서 마지막에 위치한 배열의 값이 피봇의 값이 되고 이 피봇의 기준값으로 j가 배열의 처음부터 검사해 나가며 크기를 비교한다. 만약 검사한 값이 피봇의 값보다 작다면 i번째 배열의 위치가 j의 값과 스왑된다.

마지막으로 i+1번째의 값과 피봇의 값과 변경이 되면서 i + 1 값을 반환한다.

```c
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        /* pi is partitioning index, arr[p] is now
           at right place */
        int pi = partition(arr, low, high);

        // Separately sort elements before
        // partition and after partition
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```
분할된 값은 다시 재귀함수를 통해서 원소가 한개 남을 때까지 재귀한다.

## Hoare 분할방식

```c
int partition(int arr[], int low, int high) {
    int pivot = arr[low];
    int i = low - 1, j = high + 1;

    while (1) {
      
        // Find leftmost element greater
        // than or equal to pivot
        do {
            i++;
        } while (arr[i] < pivot);

        // Find rightmost element smaller 
        // than or equal to pivot
        do {
            j--;
        } while (arr[j] > pivot);

        // If two pointers met
        if (i >= j)
            return j;

        // Swap arr[i] and arr[j]
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

호어 분할 방식은 i와 j가 서로를 향해서 다가가고 서로가 크로스된 상황에서 함수가 종료가 된다. i가 지나가는 배열에 피벗보다 크거나 같은값을 찾아 대기하고, j가 지나가는 배열에 피벗보다 작거나 같은값을 찾아 스왑하는 과정을 거친다.

---
각 분할 방식마다 장단점의 차이가 있고, 상황에 맞는 분할 방식을 채택하는게 현명한거 같다.



