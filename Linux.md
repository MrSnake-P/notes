[TOC]

## 常用指令

重定向

> `>`		覆盖文件，没有文件新建文件
>
> `>>`	   可以追加信息，在原有的基础上添加信息

## csvtk

[文档](https://bioinf.shenwei.me/csvtk/usage/)

​	csv现在是程序员存储数据非常广泛和常用的格式。csvtk 由编程语言go开发，下载即用，非常方便。

### pretty

​	#格式化显示

```shell
csvtk pretty company.csv -s "|"
companyName                       |contactName
Alfreds Futterkiste               |Maria Anders
Ana Trujillo Emparedados y helados|Ana Trujillo
Antonio Moreno Taquería           |Antonio Moreno
```



### headers

 	#显示文件中第一行

```shell
csvtk headers company.csv	# 以竖列的形式
customerID
companyName
contactName
contactTitle
address
-------------------------
cat company.csv | csvtk headers	# 与上面等价，用cat输入到管道符的形式
```

### head

​	#显示文件前n行

```shell
cat company.csv | csvtk head -n 5	# 不指定-n，默认为10行
```

### cut

​	#裁剪文件中的列，支持精确定位，和模糊查询

```shell
cat company.csv | csvtk cut -f 5,6 	# 裁剪第五和第六列
# cat company.csv | csvtk cut -f companyName,contactName 等价
companyName,contactName
Alfreds Futterkiste,Maria Anders
Ana Trujillo Emparedados y helados,Ana Trujillo
Antonio Moreno Taquería,Antonio Moreno
--------------------------------------------
cat company.csv | csvtk cut -F -f *Name	# 模糊查询，等价于上面
companyName,contactName
Alfreds Futterkiste,Maria Anders
Ana Trujillo Emparedados y helados,Ana Trujillo
Antonio Moreno Taquería,Antonio Moreno
```

