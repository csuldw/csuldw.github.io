


//非递归

```
int BinSearch(int Array[],int SizeOfArray,int key/*要找的值*/)  
{  
    int low=0,high=SizeOfArray-1;  
    int mid;  
    while (low<=high)  
    {  
        mid = (low+high)/2;  
        if(key==Array[mid])  
            return mid;  
        if(key<Array[mid])  
            high=mid-1;  
        if(key>Array[mid])  
            low=mid+1;  
    }  
    return -1;  
}  
```

//递归

```
int BinSearch(int Array[],int low,int high,int key/*要找的值*/)  
{  
    if (low<=high)  
    {  
        int mid = (low+high)/2;  
        if(key == Array[mid])  
            return mid;  
        else if(key<Array[mid])  
            return BinSearch(Array,low,mid-1,key);  
        else if(key>Array[mid])  
            return BinSearch(Array,mid+1,high,key);  
    }  
    else  
        return -1;  
}  
```



```
//二分查找的递归版本
int binary_search_recursion(const int array[], int low, int high, int key)  
{  
    int mid = low + (high - low)/2;  
    if(low > high)  
        return -1;  
    else{  
        if(array[mid] == key)  
            return mid;  
        else if(array[mid] > key)  
            return binary_search_recursion(array, low, mid-1, key);  
        else  
            return binary_search_recursion(array, mid+1, high, key);  
    }  
}  

//二分查找的循环版本
int binary_search_loop(const int array[], int len, int key)  
{  
    int low = 0;  
    int high = len - 1;  
    int mid;  
    while(low <= high){  
        mid = (low+high) / 2;  
        if(array[mid] == key)  
            return mid;  
        else if(array[mid] > key)  
            high = mid - 1;  
        else  
            low = mid + 1;  
    }  
    return -1;  
}
```