# DAA_assignment3
# DAA_assignment3
#include <iostream>
#include <ctime> 
using namespace std;


void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}


void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; ++i) {
        int minIndex = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        int temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}

void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int L[n1], R[n2];

  for (int i = 0; i < n1; ++i) L[i] = arr[left + i];
    for (int i = 0; i < n2; ++i) R[i] = arr[mid + 1 + i];

   int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; ++j) {
        if (arr[j] <= pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

void resetArrays(int best[], int average[], int worst[], int bestOriginal[], int avgOriginal[], int worstOriginal[], int size) {
    for (int i = 0; i < size; ++i) {
        best[i] = bestOriginal[i];
        average[i] = avgOriginal[i];
        worst[i] = worstOriginal[i];
    }
}

void measureSortTime(void (*sortFunc)(int[], int), int arr[], int size, const char* sortName, const char* caseType) {
    clock_t start = clock();
    sortFunc(arr, size);
    clock_t end = clock();
    double timeTaken = double(end - start) / CLOCKS_PER_SEC;
    cout << sortName << " on " << caseType << " Case took: " << timeTaken << " seconds." << endl;
}

void measureSortTimeMergeSort(void (*sortFunc)(int[], int, int), int arr[], int left, int right, const char* sortName, const char* caseType) {
    clock_t start = clock();
    sortFunc(arr, left, right);
    clock_t end = clock();
    double timeTaken = double(end - start) / CLOCKS_PER_SEC;
    cout << sortName << " on " << caseType << " Case took: " << timeTaken << " seconds." << endl;
}

int main() {
    const int size = 1000;

  int bestOriginal[size];
    for (int i = 0; i < size; ++i) bestOriginal[i] = i;
    
   int avgOriginal[size] = {3, 1, 4, 5, 2}; 
    
   int worstOriginal[size];
    for (int i = 0; i < size; ++i) worstOriginal[i] = size - i;

   int bestCaseArray[size], averageCaseArray[size], worstCaseArray[size];

  resetArrays(bestCaseArray, averageCaseArray, worstCaseArray, bestOriginal, avgOriginal, worstOriginal, size);
    measureSortTime(bubbleSort, bestCaseArray, size, "Bubble Sort", "Best");
    measureSortTime(bubbleSort, averageCaseArray, size, "Bubble Sort", "Average");
    measureSortTime(bubbleSort, worstCaseArray, size, "Bubble Sort", "Worst");

   resetArrays(bestCaseArray, averageCaseArray, worstCaseArray, bestOriginal, avgOriginal, worstOriginal, size);
    measureSortTime(selectionSort, bestCaseArray, size, "Selection Sort", "Best");
    measureSortTime(selectionSort, averageCaseArray, size, "Selection Sort", "Average");
    measureSortTime(selectionSort, worstCaseArray, size, "Selection Sort", "Worst");

  resetArrays(bestCaseArray, averageCaseArray, worstCaseArray, bestOriginal, avgOriginal, worstOriginal, size);
    measureSortTimeMergeSort(mergeSort, bestCaseArray, 0, size - 1, "Merge Sort", "Best");
    measureSortTimeMergeSort(mergeSort, averageCaseArray, 0, size - 1, "Merge Sort", "Average");
    measureSortTimeMergeSort(mergeSort, worstCaseArray, 0, size - 1, "Merge Sort", "Worst");

   resetArrays(bestCaseArray, averageCaseArray, worstCaseArray, bestOriginal, avgOriginal, worstOriginal, size);
    measureSortTimeMergeSort(quickSort, bestCaseArray, 0, size - 1, "Quick Sort", "Best");
    measureSortTimeMergeSort(quickSort, averageCaseArray, 0, size - 1, "Quick Sort", "Average");
    measureSortTimeMergeSort(quickSort, worstCaseArray, 0, size - 1, "Quick Sort", "Worst");

  return 0;
}
