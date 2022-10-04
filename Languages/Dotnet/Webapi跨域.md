# Web Api 跨域配置

> ## 在services上添加
```csharp {.line-numbers}
// 跨域设置
class AppSettings{
    public readonly static CorPolicyName = "PolicyName";
    //...
}
builder.Services.AddCors(options =>
{
    options.AddPolicy(name: AppSettings.CorPolicyName,
    policy =>
    {
        //policy.WithOrigins("http://localhost", "http://10.8.8.8", "http://10.8.8.6");//.AllowAnyOrigin().AllowAnyHeader().AllowAnyMethod();
        policy.AllowAnyOrigin();
        policy.AllowAnyHeader();
        policy.AllowAnyMethod();
    });
});
```
* <font color=red>不同同时使用AllowAnyOrigin和AllowCredentials</font>
> ## 在 app 上添加
``` csharp
app.UseAuthorization();
// 跨域设置
app.UseCors(AppSettings.CorPolicyName);
app.MapControllers();

app.Run();
```
* <font color=red>注意： app.UseCors(“cors”); 要写在app.UseAuthorization(); 后面，否则会报错。</font>

> ## 在 Controller 或 Method 上添加属性
```csharp
[EnableCors(AppSettings.CorPolicyName)]
public IActionResult Get()
{
    return Ok(new {Code = 200});
}
```