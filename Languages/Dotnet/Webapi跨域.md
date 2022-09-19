# Web Api 跨域配置

> ## 在services上添加
``` C#
services.AddCors(options => 
    options.AddPolicy("cors",
    p => p.AllowAnyOrigin().AllowAnyHeaher().AllowAnyMethod().AllowCredentials())
);
```
* <font color=red>不同同时使用AllowAnyOrigin和AllowCredentials</font>
> ## 在 app 上添加
``` c#
app.UseCors("any");
```
* <font color=red>注意： app.UseCors(“cors”); 要写在app.UseAuthorization(); 后面，否则会报错。</font>

> ## 在 Controller 或 Method 上添加属性
```c#
[EnableCors("any")]
public IActionResult Get()
{
    return Ok(new {Code = 200});
}
```