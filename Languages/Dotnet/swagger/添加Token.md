
```csharp
var securityScheme = new OpenApiSecurityScheme()
{
    Description = "JWT Authorization header using the Bearer scheme. Example: \"Authorization: Bearer {token}\"",
    Name = "Authorization",
    //参数添加在头部
    In = ParameterLocation.Header,
    //使用Authorize头部
    Type = SecuritySchemeType.Http,
    //内容为以 bearer开头
    Scheme = "bearer",
    BearerFormat = "JWT"
};
//把所有方法配置为增加bearer头部信息
var securityRequirement = new OpenApiSecurityRequirement
{
    {
        new OpenApiSecurityScheme
        {
            Reference = new OpenApiReference
            {
                Type = ReferenceType.SecurityScheme,
                Id = "bearerAuth"
            }
        },
        new string[] {}
    }
};

//注册到swagger中
builder.Services.AddSwaggerGen(c=> { 
    c.AddSecurityDefinition("bearerAuth", securityScheme);
    c.AddSecurityRequirement(securityRequirement);
});
```