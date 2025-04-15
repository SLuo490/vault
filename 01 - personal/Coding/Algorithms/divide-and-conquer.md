## Definition
Divide-and-conquer is a method in breaking down problems into several subproblems that are similar to the original problem but smaller in size. It solves the subproblem recursively, then combine these solutions to create a solution to the original problem. 

### Divide-and-Conquer Method
* **Base Case**: When problem is small enough, solve directly
* **Recursive Case**: Three characteristic steps:
	1. **Divide**: Break the problem into smaller instances of same problem
	2. **Conquer**: Solve subproblems recursively
	3. **Combine**: Form solution to original problem from subproblem solutions

## Example: [[merge sort]] algorithm

Merge sort algorithm follows the divide-and-conquer method. It sorts subarray `A[p...r]`, starting with the entire array `A[1...n]`. It then recursively sorts smaller and smaller subarrays. 

### Steps:
1. **Divide**:
	- Compute midpoint `q` of `A[p...r]` (average of p and r)
	- Divide `A[p...r]` into `A[p...q]` and `A[q + 1...r]`
2. **Conquer**:
	- Sort `A[p...q]` recursively using merge sort
	- Sort `A[q + 1...r]` recursively using merge sort
3. **Combine**:
	- Merge sorted subarrays `A[p...q]` and `A[q + 1...r]` back into `A[p...r]`

### Merge Procedure
```
MERGE(A, p, q, r)
	nL = q - p + 1  // length of A[p:q]
	nR = r - q      // length of A[q + 1:r]

	let L[0:nL - 1] and R[0:nR - 1] be new arrays
	for i = 0 to nL - 1 // copy A[p:q] into L[0:nL - 1]
		L[i] = A[p + i]
	for j = 0 to nR - 1 // copy A[q + 1:r] into R[0:nR - 1]
		R[j] = A[q + j + 1]

	i = 0  // i indexes the smallest remaining element in L
	j = 0  // j indexes the smallest remaining element in R
	k = p  // k indexes the location in A to fill

	// As long as each of the arrays L and R contains an unmerged element, 
	// copy the smallest unmerged element back into A[p:r]

	while i < nL and j < nR
		if L[i] <= R[j]
			A[k] = L[i]
			i = i + 1
		else 
			A[k] = R[j]
			j = j + 1
		k = k + 1

	//  copy the remainder of the end of L and R array into A[p:r]
	while i < nL
		A[k] = L[i]
		i = i + 1
		k = k + 1

	while j < nR
		A[k] = R[j]
		j = j + 1
		k = k + 1
```

![[Pasted image 20250313142111.png]]

### Merge Sort Procedure
```
MERGE-SORT(A, p, r)
	if p >= r               // zero or one element?
		return
	q = [(p + r) / 2]       // midpoint of A[p:r]
	
	MERGE-SORT(A, p, q)     // recursively sort A[p:q]
	MERGE-SORT(A, q + 1, r) // recursively sort A[q + 1:r]
	
	// Merge A[p:q] and A[q + r:r] into A[p:r]
	MERGE(A, p, q, r)
```

![[Pasted image 20250313143004.png]]