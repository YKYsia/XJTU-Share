#######################################
#   将这里的CLASSDIR改成你机子上cool所在的目录
#   以便Makefile可以找到cool/bin所在的位置
CLASSDIR= ~/cool
#######################################
#作业号，用来判断需要链接的头文件和源文件的目录
ASSN = 2
#没有用,表示这个作业是用来干嘛的
CLASS= xjtucomplier
#表示lib的位置，这里不用lib，暂时没有用
LIB= -L/usr/sww/lib

#make依赖的文件列表
SRC= cool.flex test.cl README
#make需要的源文件列表
CSRC= lextest.cc utilities.cc stringtab.cc handle_flags.cc
#一个shell脚本程序
TSRC= mycoolc
HSRC=
#需要生成的文件
CGEN= cool-lex.cc
HGEN=
#一个完整的编译过程
LIBS= parser semant cgen
CFIL= ${CSRC} ${CGEN}          
LSRC= Makefile
#OBJS表示所有的.o目标
OBJS= ${CFIL:.cc=.o}
#执行make test.output命令的时候，会将记号输出到test.out文件中
OUTPUT= test.output

#指定头文件和源文件的目录
CPPINCLUDE= -I. -I ${CLASSDIR}/include/PA${ASSN} -I ${CLASSDIR}/src/PA${ASSN}

#设置flex的编译选项,-d表示允许debug，-o表示允许output
FFLAGS= -d -o cool-lex.cc
#使用flex
FLEX=flex ${FFLAGS}

#设置编译器g++
CC=g++
#设置g++的编译选项,-g表示添加调试信息，-Wall表示输出警告
CFLAGS= -g -Wall -Wno-unused ${CPPINCLUDE}
#-MM表示自动生成.o文件和.h文件的关系，且不输出这个关系
DEPEND = ${CC} -MM ${CPPINCLUDE}

#缺省目标
default: source lexer dotest

#调用shell脚本检查文件是否齐全
source : ${SRC} ${TSRC} ${LSRC} ${LIBS} lsource 

lsource: ${LSRC}

#将记号输出到output.test文件中去
${OUTPUT}:	lexer test.cl
	@rm -f test.output
	-./lexer test.cl >test.output 2>&1

#生成lexer，这里的OBJS都是.o，makefile会自动根据依赖项生成.o文件
lexer: ${OBJS}
	${CC} ${CFLAGS} ${OBJS} ${LIB} -o lexer

#双缀规则，表示所有的.cc文件都必须编译成.o文件
.cc.o:
	${CC} ${CFLAGS} -c $<

#处理cool-flex，生成词法分析程序的.cc文件
cool-lex.cc: cool.flex
	${FLEX} cool.flex

#测试lexer
dotest:	lexer test.cl
	./lexer test.cl

${LIBS}:
	${CLASSDIR}/etc/link-object ${ASSN} $@

# 以下的部分用来检查和copy本次实验需要的文件
# 如果目录下有这些文件的话，不会被覆盖

${SRC} :								
	${CLASSDIR}/etc/copy-skel ${ASSN} ${SRC}

${LSRC} :
	${CLASSDIR}/etc/link-shared ${ASSN} ${LSRC}

${TSRC} ${CSRC}:
	-ln -s ${CLASSDIR}/src/PA${ASSN}/$@ $@

${HSRC}:
	-ln -s ${CLASSDIR}/include/PA${ASSN}/$@ $@

#以下的部分执行清理,使用：例如，make clean

submit-clean: ${OUTPUT}
	-rm -f *.s core ${OBJS} lexer cool-lex.cc *~ parser cgen semant

clean :
	-rm -f ${OUTPUT} *.s core ${OBJS} lexer cool-lex.cc *~ parser cgen semant *.d *.cc

clean-compile:
	@-rm -f core ${OBJS} cool-lex.cc ${LSRC}

%.d: %.cc ${SRC} ${LSRC}
	${SHELL} -ec '${DEPEND} $< | sed '\''s/\($*\.o\)[ :]*/\1 $@ : /g'\'' > $@'

-include ${CFIL:.cc=.d}


