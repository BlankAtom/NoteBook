#webapi #net6 #csharp 

## Json解析器
### 修改默认json解析器为`Newtonsoft.Json`
```c#
public void ConfigureServices(IServiceCollection services) {
	services.AddControllers().AddNewtonsoftJson();
}
```

## 设置全局`JsonOptions`
```Cs
services.AddControllers().AddJsonOptions(options => {
	options.JsonSerializerOptions.IncludeFields = true;
	options.JsonSerializerOptions.PropertyNameCaseInsensitive = true;
})
```

## 添加全局过滤器
```C#
services.AddControllers(options => {
	options.Filters.Add<LogFilter>();
})
```

## 添加跨域
In Service configure
```C#
service.AddCors(options => {
	options.AddPolicy("CorName", builder => {
		builder.AllowAnyOrigin()
				.AllowAnyHeader()
				.AllowAnyMethod();
	})
})
```

In application app built.
```C#
app.UseCors("CorName");
```
> Tips: 跨域应用的位置在`UseRouting`之后


## 动态加载程序集并注册`Controller`
```cs
foreach(Assembly a in assemblies)
	services.AddControllers().AddApplicationPart(a);
```

> note: `services.AddControllers()`会返回`IMvcBuilder`实例，如果有多次凋用该方法的情况，可以先获取实例，再调用。

