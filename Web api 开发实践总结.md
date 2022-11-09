#webapi #net6 #csharp 

## Json解析器
### 修改默认json解析器为`Newtonsoft.Json`
```
public void ConfigureServices(IServiceCollection services) {
	services.AddControllers().AddNewtonsoftJson();
}
```

## 设置全局`JsonOptions`
```
services.AddControllers().AddJsonOptions(options => {
	options.JsonSerializerOptions.IncludeFields = true;
	options.JsonSerializerOptions.PropertyNameCaseInsensitive = true;
})
```

## 添加全局过 