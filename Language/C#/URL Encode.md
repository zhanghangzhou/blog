C# 与 JS 之间编解码处理
---
> escape不编码字符有69个：*，+，-，.，/，@，_，0-9，a-z，A-Z  
encodeURI不编码字符有82个：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z  
encodeURIComponent不编码字符有71个：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z

# Type 1
## JS: escape :

js使用数据时可以使用escape
例如：搜藏中history纪录。
0-255以外的unicode值进行编码时输出%u****格式，其它情况下escape，encodeURI，encodeURIComponent编码结果相同。
解码使用：unescape

## C#:

HttpUtility.UrlEncode   
HttpUtility.UrlDecode

# Type 2
## JS: encodeURI ：

进行url跳转时可以整体使用encodeURI
例如：Location.href=encodeURI("http://cang.baidu.com/do/s?word=百度&ct=21");
解码使用decodeURI();

## C#:  
decodeURIComponent

# Type 3
## JS: encodeURIComponent ：
传递参数时需要使用encodeURIComponent，这样组合的url才不会被#等特殊字符截断。                      
例如：
```
<script language="javascript">
document.write('<a href="http://passport.baidu.com/?logout&aid=7& u='+encodeURIComponent("http://cang.baidu.com/bruce42")+'">退出</a& gt;');
</script>
```
解码使用decodeURIComponent()

## C#:
[HttpContext.Current.]Server.UrlDecode
[HttpContext.Current.]Server.UrlEncode
