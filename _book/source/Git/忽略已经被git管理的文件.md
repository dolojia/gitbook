# .gitignore规则和忽略已经被git管理的文件

文件 .gitignore 的格式规范如下：

- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 惊叹号（!）取反。表示不忽略

```
所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 
匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 
c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9]这里写代码片 
表示匹配所有 0 到 9 的数字）。 使用两个星号（*) 表示匹配任意中间目录，比如a/**/z 可以匹配 a/z, a/b/z 或 
a/b/c/z等。
```

我们再看一个 .gitignore 文件的例子：

```properties
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

## 如何忽略已经被git管理的文件

.gitignore 文件 一般再项目建立的时候就应该写好，这样才能有效，但如果是已经被git管理的文件，后边发现不想再跟踪了，直接再.gitignore添加此文件 是不会生效的 。有效的操作如下；

```shell
#1.git rm -r --cached <fileA> 
   此命令的含义是untrack 指定的fileA，或者使用 --cached . 表示将untrack 工作目录下的所有文件。
#2.修改.gitignore 文件 
   将fileA 添加到忽略文件中
#3.提交
   最后一步就是将修改提交。
   git add . 
   git commit -m "ignore fileA"
   git push
```

