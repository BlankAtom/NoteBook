#net6 #source

# IEnumerable.Contains
```C#
public static bool Contains<TSource>(this IEnumerable<TSource> source, TSource value, IEqualityComparer<TSource>? comparer)  
{  
    if (source == null)  
    {  
        ThrowHelper.ThrowArgumentNullException(ExceptionArgument.source);  
    }  
  
    if (comparer == null)  
    {  
        foreach (TSource element in source)  
        {  
            if (EqualityComparer<TSource>.Default.Equals(element, value)) // benefits from devirtualization and likely inlining  
            {  
                return true;  
            }  
        }  
    }  
    else  
    {  
        foreach (TSource element in source)  
        {  
            if (comparer.Equals(element, value))  
            {  
                return true;  
            }  
        }  
    }  
  
    return false;  
}
```
可以看到，Contains的判断是循环遍历的，我们可以将其简化一下：
```C#
public static bool Contains(IEnumerable<TSource> source, TSource value) {
	foreach (TSource element in source)  
	{  
		if (element == source) 
		{  
			return true;  
		}  
	}  
}
```

# IEnumerable.Find
```C#
public T? Find(Predicate<T> match)  
{  
    if (match == null)  
    {  
        ThrowHelper.ThrowArgumentNullException(ExceptionArgument.match);  
    }  
  
    for (int i = 0; i < _size; i++)  
    {  
        if (match(_items[i]))  
        {  
            return _items[i];  
        }  
    }  
    return default;  
}
```
Find 也是对集合进行遍历然后返回第一个对应的元素的。
FindAll同理。

# IArraySortHelper.BinarySearch
```C#
private static int BinarySearch(T[] array, int index, int length, T value)  
{  
    int lo = index;  
    int hi = index + length - 1;  
    while (lo <= hi)  
    {  
        int i = lo + ((hi - lo) >> 1);  
        int order;  
        if (array[i] == null)  
        {  
            order = (value == null) ? 0 : -1;  
        }  
        else  
        {  
            order = array[i].CompareTo(value);  
        }  
  
        if (order == 0)  
        {  
            return i;  
        }  
  
        if (order < 0)  
        {  
            lo = i + 1;  
        }  
        else  
        {  
            hi = i - 1;  
        }  
    }  
  
    return ~lo;  
}
```
当自定义comparetor之后，判断依据也就会用定义的方式