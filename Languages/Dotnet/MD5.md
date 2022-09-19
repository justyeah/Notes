# Dotnet core MD5加密
```c#
using System.Security.Cryptography;
using (var md5 = MD5.Create())
{
    var result = md5.ComputeHash(Encoding.UTF8.GetBytes(inputValue));
    var strResult = BitConverter.ToString(result);
    string result3 = strResult.Replace("-", "");
}
```