#### Linux三剑客

#### 1.sed

#### 1.1 简介

```
sed流编辑器，配合正则表达式，进行文本处理。处理时，把当前处理的行存储在临时缓冲区中（模式空间），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往【屏幕】。接着处理下一行，这样不断重复，直到文件末尾。【文件内容并没有改变】，除非你使用重定向存储输出。
```

#### 1.2 命令、选项、替换标记

```python
sed [options] 'command' file(s)
sed [options] -f scriptfile file(s)
```

##### 1.2.1 选项

```
选项：
-e<script> / --expression=<script>:以选项中指定的script来处理输入的文本文件
-f<script> / --file=<script文件>：以选项中指定的script来处理输入的文本文件
-n:仅显示script处理后的结果
```

##### 1.2.2 参数

```
文件：待处理文件列表
```

##### 1.2.3 命令

```
a\:在当前行下面插入文本
i\:匹配之后直接进行修改（比较常用）
c\:把选定的行改为新的文本
d:删除选择的行
s:替换指定字符
```

##### 1.2.4 替换标记

```
g:表示行内全面替换
p:表示打印行
w:表示把行写入一个文件
```

##### 1.2.5 元字符集

```java
^: 匹配行开始； /^sed/ 匹配所有以sed开头的行
$: 匹配行结束；/sed$/ 匹配所有以sed结尾的行
.:匹配一个非【换行符】的任意字符； /s.d/ 匹配s后接一个任意字符，最后接d
* 匹配0个或多个字符，如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行。
\(..\) 匹配子串，保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers。
[] 匹配一个指定范围内的字符，如/[sS]ed/匹配sed和Sed。
[^] 匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行。
& 保存搜索字符用来替换其他字符，如s/love/**&**/，love这成**love**。
\< 匹配单词的开始，如:/\<love/匹配包含以love开头的单词的行。
\> 匹配单词的结束，如/love\>/匹配包含以love结尾的单词的行。
x\{m\} 重复字符x，m次，如：/0\{5\}/匹配包含5个0的行。
x\{m,\}重复字符x,至少m次。如：/0\{5,\}/匹配至少有5个0的行。
x\{m,n\} 重复字符x，至少m次，不多于n次，如：/0\{5,10\}/匹配5~10个0的行。
```

##### 1.2.6 常用操作

替换操作（s）

```java
	sed 's/book/books/' books.txt    #将处理的所有行进行输出
---只打印发生替换的行---
	sed -n 's/book/books/p' books.txt    #常用组合（-n:静默模式 		p:打印模板块的行）
---全局替换（g）---
	sed 's/book/books/g' book.txt  #global(會全行匹配搜索到的内容)
---指定從第N処匹配開始替換---
	sed 's/book/books/3g' book.txt
---删除空格开头或结尾的行--- 
	sed 's/^[ \t]*//g' book.txt	
```

刪除操作（d）

```python
---删除空白行---
	sed '/^$/d' test.txt
---删除文件第n行---
	sed 'nd'test.txt
---删除文件第n到m行---
	sed 'n,md' test.txt
---删除文件第n行到末尾所有行---
	sed '2,$d' test.txt
---删除文件最后一行---
	sed '$d' test.txt
---输出文件中所有以“test”开头的行
	sed '/^test/d' test.txt
```

子串匹配标识\1( 匹配给定样式中的一部分)

```java
1.echo this is digit 7 in a number | sed 's/digit \([0-9])\/\1/'
# this is 7 in a number
解释：命令行中将"digit 7"作为一个整体，并匹配子串进行替换
2.echo aaa BBB | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
# BBB aaa
解释：将"aaa BBB"作为一个整体，分为两个子串，并进行替换
3.echo "mysql-redis-server" | sed 's/\(mysql\)/\1SQL/'
```

组合多表达式

```python
（1）sed '表达式' | sed '表达式'  <==> （2）sed '表达式；表达式' <==> sed -e '表达式' -e '表达式'
（2）示例：去除空白行并删除行开头的空格
方式1 # sed '/^$/d' test.txt | sed 's/^[ ]*//'
方式2 # sed '/^$/d;s/^[ ]*//' test.txt
方式3(-e:多点编辑) # sed -e '/^$/d' -e 's/^[ ]*//' test.txt
```

引用

```python
sed表达式可以使用单引号进行引用，但表示式内部包含【变量字符串】，就需要使用双引号；
test=hello
echo "hello world" | sed 's/$test/Hello/'
示例： 修改/test目录下所有以.txt结尾文件中的'dangyang'为'xiaoding'
# name="dangynag"
# find /test -type f -name "*.txt" | xargs sed -i "s/$name/xiaoding/g"
```

选定行的范围（,）

```python
---所有在test和check内的行内容將被打印出来---
	# sed '/test/,/eheck/p' test.txt
---打印从第n行开始到第一个包含以test开头的行---
	#sed -n 'n,/^test/p' test.txt
---对于test和west之间的行，每行的末尾用字符串aaa bbb替换---
	#sed '/test/,/west/s/$/aaa bbb/' test.txt
```

从文件读入(r)

```python
file里的内容被读进来，显示在与test匹配的行后面，【如果显示多行，则file的内容将显示在所有匹配的行后面】。
# sed '/test/r file' filename
```

从文件写(w)

```python
在[filename]文件中所有包含test的行都被写入到[file]里
# sed '/test/w [file]' [filename]
```

追加(行下)：a\

```python
1. 将'this is the first name'追加到以'test'开头的行后面。
# sed  -i '/^test/a\this is the first name' hello.txt
2.在hello.txt的第2行下加入"this is a test"
# sed  -i '2a\this is a test line' hello.txt
```

插入(行上)： i\

```python
1.将'this is a test line'追加到以test开头的行前面
# sed -i '/^test/i\this is a test line' hello.txt
2.在hello.txt第6行前插入 'this is a test line'
# sed -i '6i\this is a test line' hello.txt
```

下一个： n命令

```python
如果'test'被匹配，则移动到匹配行的下一行，进行替换
# sed '/test/{n; /aaa/BBB;}' hello.txt
```

#### 1.3 实例

（1）给某行添加注释

```
sed -i '/^%dnagyang/ s/^\(.*\)$/#\1/' /etc/sudoers
```

（2）查看奇、偶行

```
sed 'n;d' staff.csv     #奇数行
sed '1d;n;d' staff.csv	#偶数行
第一条命令中的【n;】表示输出当前行并立即读取下一行。第二条命令先把第一行记录删除，于是再输出的奇数行就自然变成原来的偶数行了。
```

