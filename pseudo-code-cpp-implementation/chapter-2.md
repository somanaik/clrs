##### Insertion Sort :
```cpp
// sort the given array of n integers in ascending order
void insertion_sort(vector<int> &A) {
    int n = A.size();
    for(int j = 1; i < n; j++) {
        int key = A[j];
        // Insert A[j] into the sorted sequence A[1...j-1].
        int i = j - 1;
        while (i > -1 and A[i] > key) {
            A[i + 1] = A[i];
            i = i - 1;
        }
        A[i + 1] = key;
    }
}
```
> Time complexity : O(n^2) in worst case
---

##### Merge :
```cpp
// merge two sorted subarrays A[p...q] and A[q+1...r]
void merge(vector<int> &A, int p, int q, int r) {
    int n1=q-p+1;
    int n2=r-q;
    vector<int> L(n1+1), R(n2+1);
    for(int i=0; i<n1; i++) {
        L[i]=A[p+i];
    }
    for(int i=0; i<n2; i++) {
        R[i]=A[q+i+1];
    }
    
    L[n1]=INT_MAX;  // init to infinity
    R[n2]=INT_MAX;  // init to infinity

    int i=0, j=0;
    for(int k=p; k<=r; k++) {
        if(L[i]<=R[j]) {
            A[k]=L[i];
            i++;
        } else {
            A[k]=R[j];
            j++;
        }
    }
}
```
> Time Complexity : O(n) in worst case
---

##### Merge sort :
```cpp
// sort subarray A[p...r] in ascending order
void merge_sort(int A[], int p, int r){
    if(p<r){
        int q=(p+r)/2;
        merge_sort(A,p,q);
        merge_sort(A,q+1,r);
        merge(A,p,q,r);             // check merge function code
    }
}
```
> Time Complexity : O(nlog(n)) in worst case
---

##### Bubble Sort :
```cpp
// sort given array of n integers in ascending order
void bubble_sort(vector<int> &A) {
    int n=A.size();
    for(int i=0; i<n; i++) {
        for(int j=n-1; j>i; j--) {
            if(A[j]<A[j-1]) {
                swap(A[j],A[j-1]);
            }
        }
    }
}
```
> Time Complexity  : O(n^2) in worst case