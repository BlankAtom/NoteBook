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

