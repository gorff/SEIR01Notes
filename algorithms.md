# Algoithms from my.genearlassemb.ly site

## Binary search 

```function binarySearch(arr, item, first = 0, last = null) {
    if (!last) last = arr.length
    let midpoint = Math.floor((last - first) / 2) + first
    if (arr[midpoint] === item) return midpoint
    if (arr[midpoint] > item) return binarySearch(arr, item, first, midpoint)
    if (arr[midpoint] < item) return binarySearch(arr, item, midpoint, last)
}
```
## Recursive function
```
function sumArrayOfNums(arr, index = 0, sum = 0){
  if(index === arr.length){
      return sum;
  }
  sum += arr[index];
  return sumArrayOfNums(arr, index + 1, sum);
} 
```

## Recursive function with helper function
```function sumArrayOfNums(arr){
    let index = 0;
    let sum = 0;
    // Notice the lack of parameters in rSum!
    function rSum(){
      if(index === arr.length){
        return sum;
      }
      sum += arr[index];
      index++;
      return rSum();
    }
    // Once you’ve defined the helper function, make sure you call it!
    return rSum();
  }
  ```

  # sorting

| Sort           | Best Case | Average Case | Worst Case | Space     | Stability | Sorting Method |
|----------------|-----------|--------------|------------|-----------|-----------|----------------|
| Bubble sort    | Ω(N)      | Θ(N^2)       | O(N^2)     | O(1)      | Stable    | Comparison     |
| Insertion sort | Ω(N)      | Θ(N^2)       | O(N^2)     | O(1)      | Stable    | Comparison     |
| Bucket sort    | Ω(N+K)    | Θ(N+K)       | O(N^2)     | O(N+K)    | Stable    | Distribution   |
| Radix sort     | Ω(NK)     | Θ(NK)        | O(NK)      | O(N+K)    | Stable    | Distribution   |
| Merge sort     | Ω(log(N)) | Θ(log(N))    | O(log(N))  | O(N)      | Stable    | Comparison     |
| Quick sort     | Ω(log(N)) | Θ(log(N))    | O(N^2)     | O(log(N)) | Unstable  | Comparison     |
