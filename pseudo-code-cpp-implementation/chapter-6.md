##### Max Heapify :
```cpp
// Given a max heap with root not satisfying max-heap property
// Updates the given max-heap in places such that all nodes satisfy the max-heap propery
void max_heapify(vector<int> &A, int i, int heap_size) {
    int l = (i << 1) + 1;
    int r = (i << 1) + 2;
    int largest = i;
    if(l < heap_size && A[l] > A[largest]) {
        largest = l;
    }
    if(r < heap_size && A[r] > A[largest]) {
        largest = r;
    }
    if(largest != i) {
        swap(A[i], A[largest]);
        max_heapify(A, largest, heap_size);
    }
}
```
> Time complexity : O(log(n)) in the worst case
---

##### Max Heap :
```cpp
// build max heap from given array of integers
void build_max_heap(vector<int> &A) {
    for (int i = A.size() >> 1; i >= 0; i--) {
        max_heapify(A, i, A.size());
    }
}
```
> Time Complexity : O(n) in the worst case
---

##### Heap Sort :
```cpp
// sort a given array of integers in ascending order using max heap
void heap_sort(vector<int> &A) {
    build_max_heap(A);
    int heap_size = A.size();
    for(int i = A.size()-1; i >= 1; i--) {
        swap(A[i], A[0]);
        heap_size -= 1;
        max_heapify(A, 0, heap_size);
    }
}
```
> Time Complexity : O(nlog(n)) in worst case
---
#### Max Priority Queue
---
##### Heap Maximum:
```cpp
// Given a max heap array, return maximum element
int heap_maximum(vector<int> &A) {
    return A[0];
}
```
> Time Complexity : O(1) in the worst case
---

##### Heap Extract Maximum:
```cpp
// Given a maximum heap array, return and pop maximum element
int heap_extract_max(vector<int> &A) {
    if(A.size() < 1) {
        cerr << "ERROR: heap underflow!" << endl;
    } else {
        int max_res = A[0];
        A[0] = A[A.size() - 1];
        max_heapify(A, 0, A.size()-1);
        A.pop_back();
        return max_res;
    }
}
```
> Time Complexity : O(log(n)) in the worst case

##### Heap Increase Key:
```cpp
// Given a maximum heap array, increase value of given index to key
void heap_increase_key(vector<int> &A, int i, int key) {
    if(i >= A.size()) {
        cerr << "ERROR: heap index out of bound!" << endl;
    } else if(key < A[i]) {
        cerr << "ERROR: new key smaller than current key!" << endl;
    } else {
        A[i] = key;
        while(i > 0 && A[(i-1)>>1] < A[i]) {
            swap(A[(i-1)>>1], A[i]);
            i = (i - 1) >> 1;
        }
    }
}
```
> Time Complexity: O(log(n)) in the worst case
---

##### Max Heap Insert:
```cpp
// Given a max heap array, insert a new key into the heap
void max_heap_insert(vector<int> &A, int key) {
    A.push_back(INT_MIN);
    heap_increase_key(A, A.size()-1, key);
}
```
> Time Complexity : O(log(n)) in the worst case
