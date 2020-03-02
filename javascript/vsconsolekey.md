console快捷键
==============
对于前端开发者来说，在开发过程中需要监控某些表达式或变量的值的时候，用 debugger 会显得过于笨重，取而代之则是会将值输出到控制台上方便调试。最常用的莫过于console，但是反复的 console.** 输入起来太过繁琐。vscode里面的console快捷键。

cas --> console.assert() (如果断言为 false，则在信息到控制台输出错误信息)
ccl --> console.clear(object); (清除控制台console输出)
cco --> console.count(label); (记录 count() 调用次数，并输出到控制台)
cdi --> console.dir(object); (查看对象的信息)
cer --> console.error(object); (输出错误到控制台)
clg --> console.log(object); (向控制台输出一条信息)
----------------------------
cti --> console.time(object); (计时器，开始计时间，与 timeEnd() 联合使用，用于算出一个操作所花费的准确时间)
cte --> console.timeEnd(object); (计时结束)
ctr --> console.trace(object); (显示当前执行的代码在堆栈中的调用路径)
cwa --> console.warn(object); (输出警告信息，信息最前面加一个黄色三角，表示警告)
cin --> console.info(object); (控制台输出一条信息 与console.log()类似)
cgr --> console.group(object); (在控制台创建一个信息分组。 一个完整的信息分组以 console.group() 开始，console.groupEnd() 结束)
cge --> console.groupEnd(object); (设置当前信息分组结束)