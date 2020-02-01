#switch 语句

**格式**

```
switch (expression)
{
  case constant-expression:
    statement(s);
    break;
  case constant-expression:
    statement(s);
    break;
  default:
    statement(s);
}
```

注意

- expression必须为常亮表达式，必须为整形或枚举类型
- case语句数量任意
- case中需要`break`语句，否则会继续执行下一个case
- default语句可以选，所有case都不为真时执行
