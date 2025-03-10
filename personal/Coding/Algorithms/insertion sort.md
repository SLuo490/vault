## Definition
Insertion sort is an efficient algorithm for sorting a small number of elements. 

Insertion sort can be viewed as sorting a hand of playing cards. 
- Start with an empty left hand and the cards in a pile on the table
- Pick up the first card in the pile and hold it with your left hand
- Then remove one card at the time from the pile, and insert it into the correct position in your left hand. 

You find the correct position for a card by comparing it with each of the cards in your left hand. Starting at the right and moving left. As soon as you find a value that is less than or equal to the card you are holding, insert the card to the right of the card you are comparing. 

If all the cards in your left hand have values greater than the card you are holding, place the card as the leftmost card in your hand. **At all times, the cards in your left hand are sorted**. 
## Pseudocode

Insertion sort takes two parameters:
- `A` containing the values to be sorted
- `n` number of values to sort

The values occupying position `A[1]` through `A[n]` of the array, which denote by `A[1:n]`. When the `Insertion-Sort` is finished, array `A[1:n]` contains the original values, but in sorted order. 

```
Insertion-Sort(A, n)
	for i = 2 to n
		key = A[i]
		// Insert A[i] into the sorted subarray A[1:i - 1]
		j = i - 1
		while j > 0 && A[j] > key
			A[j + 1] = A[j]
			j = j - 1
		A[j + 1] = key
```

Given the array `A` that starts out with the sequence `{5, 2, 4, 6, 1, 3}`, the insertion sort procedure is shown below:

![[Pasted image 20250310154935.png]]

At the start of each iteration of the **for** loop, the subarray `A[1:i - 1]` consists of the elements originally in `A[1:i - 1]`, but in sorted order

## Implementation

### C++
```cpp
vector<int> insertionSort(vector<int>& nums, int n) {
    for (int i = 1; i < n; i++) {
        int key = nums[i]; 
        int j = i - 1; 
        
        while (j >= 0 && nums[j] > key) {
            nums[j + 1] = nums[j]; 
            j -= 1; 
        }
        nums[j + 1] = key; 
    }
    return nums; 
}
```

### Python
```python
def insertionSort(nums: List[int]) -> List[int]:
    for i in range(1, len(nums)):
        key = nums[i]
        j = i - 1
        
        while j >= 0 and nums[j] > key:
            nums[j + 1] = nums[j]
            j -= 1
        nums[j + 1] = key    
    return nums
```

### Time and Space Complexity

For the time complexity, the best case would be `O(n)` if the `nums` is sorted. But the average/worst case, the time complexity would be `O(n^2)`.  

The space complexity would be `O(1)` as we are sorting in place without using any new space. 

## Exercises

### 2.1-1
Illustrate the operation of INSERTION-SORT on an array initially containing the sequence `{31, 41, 59, 26, 41, 59}`

![[2.1-1 Insertion Sort]]