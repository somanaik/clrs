<details>
    <summary><strong>Maximum Subarray (Divide and Conquer)</strong></summary>

```cpp
// The procedure find_max_crossing_subarray takes as input the array A and the
// indices low, mid, and high, and it returns a tuple containing the indices demarcating
// a maximum subarray that crosses the midpoint, along with the sum of the values in
// a maximum subarray
tuple<int,int,int> find_max_crossing_subarray(vector<int> &A, int low, int mid, int high) {
    int left_sum=INT_MIN;
    int sum=0;
    int max_left = -1;
    for (int i=mid; i>=low; i--){
        sum = sum + A[i];
        if (sum > left_sum) {
            left_sum = sum;
            max_left = i;
        }
    }

    int right_sum = INT_MIN;
    int max_right = -1;
    sum=0;

    for (int i=mid+1; i<=high; i++){
        sum = sum + A[i];
        if(sum > right_sum) {
            right_sum = sum;
            max_right = i;
        }
    }

    return make_tuple(max_left, max_right, right_sum+left_sum);
}

// it returns a tuple containing the indices of, starting and ending in order, subarray with maximum sum in A[low,...,high] 
tuple<int,int,int> find_maximum_subarray(vector<int> &A, int low, int high) {
    if (low == high) return make_tuple(low,high,A[low]);

    int mid = (low + high) / 2;
    int left_low, left_high, left_sum;
    int right_low, right_high, right_sum;
    int cross_low, cross_high, cross_sum;
    
    tie(left_low, left_high, left_sum) = find_maximum_subarray(A,low,mid);
    tie(right_low, right_high, right_sum) = find_maximum_subarray(A,mid+1,high);
    tie(cross_low, cross_high, cross_sum) = find_max_crossing_subarray(A,low,mid,high);

    if (left_sum >= right_sum && left_sum >= cross_sum) {
        return make_tuple(left_low,left_high,left_sum);
    } else if (right_sum >= left_sum && right_sum >= cross_sum) {
        return make_tuple(right_low, right_high, right_sum);
    }
    return make_tuple(cross_low,cross_high,cross_sum);
}
```
> Time complexity : O(nlog(n)) in worst case

</details>
<details>
    <summary><strong>Matrix Multiplication</strong></summary>

```cpp
// standard matrix multiplication
vector<vector<int>> matrix_multiplication(vector<vector<int>> &A, vector<vector<int>> &B) {
    int n = A.size(), m=A[0].size();
    int p = B[0].size();
    vector<vector<int>> C(n,vector<int>(p,0));
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < p; j++) {
            for(int k = 0; k < m; k++) {
                C[i][j]+=A[i][k]*B[k][j];
            }
        }
    }
    return C;
}
```
> Time Complexity : O(n^3) in worst case

</details>

<details>
    <summary><strong>Strassen’s Matrix Multiplication(matrix size is power of 2)</strong></summary>

```cpp
// matrix addition
vector<vector<int>> matrix_addition(vector<vector<int>> &A, vector<vector<int>> &B) {
    int n = A.size(), m=A[0].size();
    vector<vector<int>> C(n,vector<int>(m));
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            C[i][j]=A[i][j]+B[i][j];
        }
    }
    return C;
}

// matrix subtraction
vector<vector<int>> matrix_subtraction(vector<vector<int>> &A, vector<vector<int>> &B) {
    int n = A.size(), m=A[0].size();
    vector<vector<int>> C(n,vector<int>(m));
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            C[i][j]=A[i][j]-B[i][j];
        }
    }
    return C;
}

// copy submatrix
vector<vector<int>> copy_sub_matrix(vector<vector<int>> &A, int srow, int erow, int scol, int ecol) {
    vector<vector<int>> C(erow-srow,vector<int>(ecol-scol));
    for(int i = srow; i < erow; i++) {
        for(int j = scol; j < ecol; j++) {
            C[i-srow][j-scol]=A[i][j];
        }
    }
    return C;
}

// multiply two square matrix A and B of size 2^n
vector<vector<int>> strassen_matrix_multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
    int rows = A.size();

    // base condition
    if(rows==1) {
        return {{A[0][0]*B[0][0]}};
    }

    // divide the matrices
    vector<vector<int>> A11 = copy_sub_matrix(A,0,rows/2,0,rows/2);
    vector<vector<int>> A12 = copy_sub_matrix(A,0,rows/2,rows/2,rows);
    vector<vector<int>> A21 = copy_sub_matrix(A,rows/2,rows,0,rows/2);
    vector<vector<int>> A22 = copy_sub_matrix(A,rows/2,rows,rows/2,rows);
    
    vector<vector<int>> B11 = copy_sub_matrix(B,0,rows/2,0,rows/2);
    vector<vector<int>> B12 = copy_sub_matrix(B,0,rows/2,rows/2,rows);
    vector<vector<int>> B21 = copy_sub_matrix(B,rows/2,rows,0,rows/2);
    vector<vector<int>> B22 = copy_sub_matrix(B,rows/2,rows,rows/2,rows);

    // compute S1 to S10 matrices
    vector<vector<int>> S1, S2, S3, S4, S5, S6, S7, S8, S9, S10;
    S1 = matrix_subtraction(B12,B22);       // B12 - B22
    S2 = matrix_addition(A11,A12);          // A11 + A12
    S3 = matrix_addition(A21,A22);          // A21 + A22
    S4 = matrix_subtraction(B21,B11);       // B21 - B11
    S5 = matrix_addition(A11,A22);          // A11 + A22
    S6 = matrix_addition(B11,B22);          // B11 + B22
    S7 = matrix_subtraction(A12,A22);       // A12 - A22
    S8 = matrix_addition(B21,B22);          // B21 + B22
    S9 = matrix_subtraction(A11,A21);       // A11 - A21
    S10 = matrix_addition(B11,B12);         // B11 + B12

    // compute P1 to P7 recursively
    vector<vector<int>> P1, P2, P3, P4, P5, P6, P7;
    P1 = strassen_matrix_multiply(A11,S1);      // A11*S1
    P2 = strassen_matrix_multiply(S2,B22);      // S2*B22
    P3 = strassen_matrix_multiply(S3,B11);      // S3*B11
    P4 = strassen_matrix_multiply(A22,S4);      // A22*S4
    P5 = strassen_matrix_multiply(S5,S6);       // S5*S6
    P6 = strassen_matrix_multiply(S7,S8);       // S7*S8
    P7 = strassen_matrix_multiply(S9,S10);      // S9*S10

    // compute C11, C12, C21 and C22
    vector<vector<int>> C11, C12, C21, C22, P54, P62, P51, P37;
    P54 = matrix_addition(P5,P4);
    P62 = matrix_subtraction(P6,P2);
    P51 = matrix_addition(P5,P1);
    P37 = matrix_addition(P3,P7);
    
    C11 = matrix_addition(P54,P62);         // P5 + P4 + P6 - P2
    C12 = matrix_addition(P1,P2);           // P1 + P2      
    C21 = matrix_addition(P3,P4);           // P3 + P4
    C22 = matrix_subtraction(P51,P37);      // P5 + P1 - P3 - P7

    // combine C11, C12, C21 and C22 into single matrix
    vector<vector<int>> C(rows);
    for(int i=0; i<rows/2; i++) {
        C[i].insert(C[i].begin(),C11[i].begin(), C11[i].end());
        C[i].insert(C[i].end(),C12[i].begin(), C12[i].end());
    }
    for(int i=rows/2, j=0; i<rows; i++,j++) {
        C[i].insert(C[i].begin(),C21[j].begin(),C21[j].end());
        C[i].insert(C[i].end(),C22[j].begin(),C22[j].end());
    }

    return C;
}
```

> Time Complexity : O(n^2.8) in worst case

</details>