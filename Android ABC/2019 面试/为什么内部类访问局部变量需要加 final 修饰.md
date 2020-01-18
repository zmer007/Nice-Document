提示：Variable 'v' is accessed from within inner class, needs to be declared final

比如下面的 var 为什么要使用 final 修饰
```java
private static void fun() {
    String var = "";
    tv.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            System.out.println(var);
        }
    });
}
```

局部变量随着方法的执行完毕而终结，此时 OnClickListener 还没有执行到 onClick 回调，而 var 已经不存在了，所以出现问题。添加 final 修饰后，var 为被当作成员字段处理，避免了提前释放问题。