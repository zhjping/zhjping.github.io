- __CURDIR__  
    ```
    make的内嵌变量，自动设置为当前目录
    ```

- __MAKE__  
    ```
    make的内嵌变量，值为：make
    ```

- __\$__  
    ```
    表示变量或者函数的引用，要使用符号"$"的地方，需要书写两个连续的("$$")。
    ```
- __$@__  
    ```
    目标文件
    ```

- __$^__  
    ```
    所有的依赖文件
    ```

- __$?__  
    ```
    表示依赖文件列表中被改变过的所有文件。
    ```

- __$<__  
    ```
    第一个依赖文件。
    ```

- __$(@F)__  
    ```
    表示"$@"的文件部分，如果"$@"值是"dir/foo.o"，那么"$(@F)"就是"foo.o"，
    "$(@F)"相当于函数"$(notdir $@)"。
    ```

- __$*__  
    ```
    在静态模式规则中的使用方法：
    bigoutput littleoutput : %output : text.g
        generate text.g -$* > $@
    自动环变量"$*"被展开为"茎"。在这里就是"big"和"little"。
    ```

- __=__  
    ```
    变量赋值，值可以定义在文件的任何一处，也就是说，右侧中的变量
    不一定非要是已定义好的值，其也可以使用后面定义的值。
    ```

- __:=__  
    ```
    变量赋值，只能使用前面已定义好了的变量，不能使用后面定义的变量。
    ```

- __?=__  
    ```
    变量赋值，如果变量没有定义过，则进行赋值操作，如果已经定义，
    则什么也不做。
    ```
- __+=__  
    ```
    变量追加值，以空格分隔。
    ```

- __$(VAR:A=B)__  
    ```
    替换变量"VAR"中所有"A"字符结尾的字为"B"结尾的字。
    例如：
        foo := a.o b.o c.o
        bar := $(foo:.o=.c)
    在这个定义中，变量"bar"的值就为"a.c b.c c.c"
    ```

- __\\__  
    ```
    反斜线，用于将一个较长的行分解为多行，反斜线之后不能有空格。
    ```

- __函数的调用语法__  
    ```
    $(FUNCTION ARGUMENTS)
    ```

- __subst__  
    ```
    $(subst FROM, TO, TEXT)
        函数名称：  字符串替换函数—subst。
        函数功能：  把字符串"TEXT"中的"FROM"字符替换为"TO"。
        返回值：    替换后的新字符串。

    示例：
        $(subst ee,EE,feet on the street)
        替换"feet on the street"中的"ee"为"EE"，
        结果得到字符串"fEEt on the strEEt"
    ```

- __patsubst__  
    ```
    $(patsubst PATTERN,REPLACEMENT,TEXT)
        函数名称：  模式替换函数—patsubst。
        函数功能：  搜索"TEXT"中以空格分开的单词，将符合模式"PATTERN"替换
                    为"REPLACEMENT"。参数"PATTERN"中可以使用模式通配符"%"
                    来代表一个单词中的若干字符。如果参数"REPLACEMENT"中也
                    包含一个"%"，那么"REPLACEMENT"中的"%"将是"TATTERN"中的
                    那个"%"所代表的字符串。在"TATTERN"和"REPLACEMENT"中，
                    只有第一个"%"被作为模式字符来处理，之后出现的不再作模式
                    字符(作为一个字符)。在参数中如果需要将第一个出现的"%"
                    作为字符本身而不作为模式字符时，可使用反斜杠"\"进行转义
                    处理(转义处理的机制和使用静态模式的转义一致，具体可参考
                    5.12.1 静态模式规则的语法一小节)。
        返回值：    替换后的新字符串。
        函数说明：  参数"TEXT"单词之间的多个空格在处理时被合并为一个空格，
                    并忽略前导和结尾空格。

    示例：
        $(patsubst %.c,%.o,x.c.c bar.c)
        把字符串"x.c.c bar.c"中以.c结尾的单词替换成以.o结尾
        的字符。函数的返回结果是"x.c.o bar.o"
    ```

- __strip__  
    ```
    $(strip STRINT)
        函数名称：  去空格函数—strip。
        函数功能：  去掉字符串(若干单词，使用若干空字符分割)"STRINT"开头和
                    结尾的空字符，并将其中多个连续空字符合并为一个空字符。
        返回值：    无前导和结尾空字符、使用单一空格分割的多单词字符串。
        函数说明：  空字符包括空格、[Tab]等不可显示字符。

    示例：
        STR = "  a  b c "
        LOSTR = $(strip $(STR))
        结果是"a b c"
    ```

- __findstring__  
    ```
    $(findstring FIND,IN)
        函数名称：  查找字符串函数—findstring。
        函数功能：  搜索字符串"IN"，查找"FIND"字符串。
        返回值：    如果在"IN"之中存在"FIND"，则返回"FIND"，否则返回空。
        函数说明：  字符串"IN"之中可以包含空格、[Tab]。搜索需要是严格的文本匹配。

    示例：
        $(findstring a, abc)
        $(findstring a, bc)
        第一个函数结果是字"a"；第二个值为空字符。
    ```

- __filter__  
    ```
    $(filter PATTERN…,TEXT)
        函数名称：  过滤函数—filter。
        函数功能：  过滤掉字符串"TEXT"中所有不符合模式"PATTERN"的单词，保留
                    所有符合此模式的单词。可以使用多个模式。模式中一般需要包
                    含模式字符"%"。存在多个模式时，模式表达式之间使用空格分割。
        返回值：    空格分割的"TEXT"字符串中所有符合模式"PATTERN"的字符串。
        函数说明：  "filter"函数可以用来去除一个变量中的某些字符串，我们下边
                    的例子中就是用到了此函数。

    示例：
        sources := foo.c bar.c baz.s ugh.h
        foo: $(sources)
        cc $(filter %.c %.s,$(sources)) -o foo
        使用"$(filter %.c %.s,$(sources))"的返回值给cc来编译
        生成目标"foo"，函数返回值为"foo.c bar.c baz.s"。
    ```

- __filter-out__  
    ```
    $(filter-out PATTERN...,TEXT)
        函数名称：  反过滤函数—filter-out。
        函数功能：  和"filter"函数实现的功能相反。过滤掉字符串"TEXT"中所有符合
                    模式"PATTERN"的单词，保留所有不符合此模式的单词。可以有多个
                    模式。存在多个模式时，模式表达式之间使用空格分割。
        返回值：    空格分割的"TEXT"字符串中所有不符合模式"PATTERN"的字符串。
        函数说明：  "filter-out"函数也可以用来去除一个变量中的某些字符串，
                    (实现和"filter"函数相反)。

    示例：
        objects=main1.o foo.o main2.o bar.o
        mains=main1.o main2.o
        $(filter-out $(mains),$(objects))
        实现了去除变量"objects"中"mains"定义的字符串(文件名)
        功能。它的返回值为"foo.o bar.o"。
    ```

- __sort__  
    ```
    $(sort LIST)
        函数名称：  排序函数—sort。
        函数功能：  给字符串"LIST"中的单词以首字母为准进行排序(升序)，并去掉
                    重复的单词。
        返回值：    空格分割的没有重复单词的字符串。
        函数说明：  两个功能，排序和去字符串中的重复单词。可以单独使用其中
                    一个功能。

    示例：
        $(sort foo bar lose foo)
        返回值为："bar foo lose"
    ```

- __word__  
    ```
    $(word N,TEXT)
        函数名称：  取单词函数—word。
        函数功能：  取字符串"TEXT"中第"N"个单词("N"的值从1开始)。
        返回值：    返回字符串"TEXT"中第"N"个单词。
        函数说明：  如果"N"值大于字符串"TEXT"中单词的数目，返回空字符串。
                    如果"N"为0，出错！

    示例：
        $(word 2, foo bar baz)
        返回值为"bar"。
    ```

- __wordlist__  
    ```
    $(wordlist S,E,TEXT)
        函数名称：  取字符串函数—wordlist。
        函数功能：  从字符串"TEXT"中取出从"S"开始到"E"的单词串。"S"和"E"表示
                    单词在字符串中位置的数字。
        返回值：    字符串"TEXT"中从第"S"到"E"(包括"E")的单词字符串。
        函数说明：  "S"和"E"都是从1开始的数字。当"S"比"TEXT"中的字数大时，
                    返回空。如果"E"大于"TEXT"字数，返回从"S"开始，到"TEXT"
                    结束的单词串。如果"S"大于"E"，返回空。

    示例：
        $(wordlist 2, 3, foo bar baz)
        返回值为："bar baz"。
    ```

- __words__  
    ```
    $(words TEXT)
        函数名称：  统计单词数目函数—words。
        函数功能：  字算字串"TEXT"中单词的数目。
        返回值：    "TEXT"字串中的单词数。

    示例：
        $(words, foo bar)
        返回值是"2"。所以字串"TEXT"的最后一个单词就是：
        $(word $(words TEXT),TEXT)。
    ```

- __firstword__  
    ```
    $(firstword NAMES…)
        函数名称：  取首单词函数—firstword。
        函数功能：  取字串"NAMES…"中的第一个单词。
        返回值：    字串"NAMES…"的第一个单词。
        函数说明：  "NAMES"被认为是使用空格分割的多个单词(名字)的序列。
                    函数忽略"NAMES…"中除第一个单词以外的所有的单词。

    示例：
        $(firstword foo bar)
        返回值为"foo"。函数"firstword"实现的功能等效于
        "$(word 1, NAMES…)"。
        以上11个函数是make内嵌的的文本处理函数。书写Makefile时
        可搭配使用来实现复杂功能。最后我们使用这些函数来实现一个
        实际应用。例子中我们使用函数"subst"和"patsbust"。Makefile
        中可以使用变量"VPATH"(参考4.5.1一般搜索(变量VPATH)一小节)
        来指定搜索路径。对于源代码所包含的头文件的搜索路径需要使用
        gcc的"-I"参数指定目录来实现，"VPATH"罗列的目录是用冒号":"
        分割的。如下就是Makefile的片段：
            ……
            VPATH = src:../includes
            override CFLAGS += $(patsubst %,-I%,$(subst :, ,$(VPATH)))
            …….
        那么第二条语句所实现的功能就是"CFLAGS += -Isrc –I../includes"。
    ```

- __basename__  
    ```
    $(basename NAMES…)
        函数名称：  取前缀函数—basename。
        函数功能：  从文件名序列"NAMES…"中取出各个文件名的前缀部分(点号之后的
                    部分)。前缀部分指的是文件名中最后一个点号之前的部分。
        返回值：    空格分割的文件名序列"NAMES…"中各个文件的前缀序列。如果文件
                    没有前缀，则返回空字串。
        函数说明：  如果"NAMES…"中包含没有后缀的文件名，此文件名不改变。如果
                    一个文件名中存在多个点号，则返回值为此文件名的最后一个点号
                    之前的文件名部分。

    示例：
        $(basename src/foo.c src-1.0/bar.c /home/jack/.font.cache-1 hacks)
        返回值为："src/foo src-1.0/bar /home/jack/.font hacks"。
    ```

- __dir__  
    ```
    $(dir NAMES…)
        函数名称：  取目录函数—dir。
        函数功能：  从文件名序列"NAMES…"中取出各个文件名的目录部分。文件名的
                    目录部分就是包含在文件名中的最后一个斜线("/")(包括斜线)
                    之前的部分。
        返回值：    空格分割的文件名序列"NAMES…"中每一个文件的目录部分。
        函数说明：  如果文件名中没有斜线，认为此文件为当前目录("./")下的文件。

    示例：
        $(dir src/foo.c hacks)
        返回值为"src/ ./"。
    ```

- __notdir__  
    ```
    $(notdir NAMES…)
        函数名称：  取文件名函数——notdir。
        函数功能：  从文件名序列"NAMES…"中取出非目录部分。目录部分是指最后一个
                    斜线("/")(包括斜线)之前的部分。删除所有文件名中的目录部分，
                    只保留非目录部分。
        返回值：    文件名序列"NAMES…"中每一个文件的非目录部分。
        函数说明：  如果"NAMES…"中存在不包含斜线的文件名，则不改变这个文件名。
                    以反斜线结尾的文件名，是用空串代替，因此当"NAMES…"中存在
                    多个这样的文件名时，返回结果中分割各个文件名的空格数目将
                    不确定！这是此函数的一个缺陷。

    示例：
        $(notdir src/foo.c hacks)
        返回值为："foo.c hacks"。
    ```

- __suffix__  
    ```
    $(suffix NAMES…)
        函数名称：  取后缀函数—suffix。
        函数功能：  从文件名序列"NAMES…"中取出各个文件名的后缀。后缀是文件名中
                    最后一个以点"."开始的(包含点号)部分，如果文件名中不包含一个
                    点号，则为空。
        返回值：    以空格分割的文件名序列"NAMES…"中每一个文件的后缀序列。
        函数说明：  "NAMES…"是多个文件名时，返回值是多个以空格分割的单词序列。
                    如果文件名没有后缀部分，则返回空。

    示例：
        $(suffix src/foo.c src-1.0/bar.c hacks)
        返回值为".c .c"。
    ```

- __addsuffix__  
    ```
    $(addsuffix SUFFIX,NAMES…)
        函数名称：  加后缀函数—addsuffix。
        函数功能：  为"NAMES…"中的每一个文件名添加后缀"SUFFIX"。参数"NAMES…"
                    为空格分割的文件名序列，将"SUFFIX"追加到此序列的每一个文件
                    名的末尾。
        返回值：    以单空格分割的添加了后缀"SUFFIX"的文件名序列。
        函数说明：

    示例：
        $(addsuffix .c,foo bar)
        返回值为"foo.c bar.c"。
    ```

- __addprefix__  
    ```
    $(addprefix PREFIX,NAMES…)
        函数名称：  加前缀函数—addprefix。
        函数功能：  为"NAMES…"中的每一个文件名添加前缀"PREFIX"。参数"NAMES…"
                    是空格分割的文件名序列，将"SUFFIX"添加到此序列的每一个文件
                    名之前。
        返回值：    以单空格分割的添加了前缀"PREFIX"的文件名序列。
        函数说明：

    示例：
        $(addprefix src/,foo bar)
        返回值为"src/foo src/bar"。
    ```

- __join__  
    ```
    $(join LIST1,LIST2)
        函数名称：  单词连接函数——join。
        函数功能：  将字串"LIST1"和字串"LIST2"各单词进行对应连接。就是将"LIST2"
                    中的第一个单词追加"LIST1"第一个单词字后合并为一个单词；将
                    "LIST2"中的第二个单词追加到"LIST1"的第二个单词之后并合并为
                    一个单词，……依次列推。
        返回值：    单空格分割的合并后的字(文件名)序列。
        函数说明：  如果"LIST1"和"LIST2"中的字数目不一致时，两者中多余部分将
                    被作为返回序列的一部分。

    示例1：
        $(join a b , .c .o)
        返回值为："a.c b.o"。

    示例2：
        $(join a b c , .c .o)
        返回值为："a.c b.o c"。
    ```

- __wildcard__  
    ```
    $(wildcard PATTERN)
        函数名称：  获取匹配模式文件名函数—wildcard
        函数功能：  列出当前目录下所有符合模式"PATTERN"格式的文件名。
        返回值：    空格分割的、存在当前目录下的所有符合模式"PATTERN"的文件名。
        函数说明：  "PATTERN"使用shell可识别的通配符，包括"?"(单字符)、
                    "*"(多字符)等。可参考4.4 文件名中使用通配符一节。
                    
    示例：
        $(wildcard *.c)
        返回值为当前目录下所有.c源文件列表。
    ```
