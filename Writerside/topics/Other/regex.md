# 正则

## 正则表达式匹配多个空格
```java
String string="a   b  a  a ";
for(String a:string.split("\\s+")){
    System.out.println(a);
}

```
