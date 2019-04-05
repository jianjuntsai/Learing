#python

## String
### startswith()方法
描述
startswith() 方法用于检查字符串是否是以指定子字符串开头，如果是则返回 True，否则返回 False。如果参数 beg 和 end 指定值，则在指定范围内检查。

语法
startswith()方法语法：
```
str.startswith(str, beg=0,end=len(string));
```

## List
### count()方法
描述
count() 方法用于统计某个元素在列表中出现的次数。

语法

```
list.count(obj)
```
参数
obj -- 列表中统计的对象。
返回值
返回元素在列表中出现的次数。

##自带函数
###  filter() 函数
描述
filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。

以下是 filter() 方法的语法:

```
filter(function, iterable)
```