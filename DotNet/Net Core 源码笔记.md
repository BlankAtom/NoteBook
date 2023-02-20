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
