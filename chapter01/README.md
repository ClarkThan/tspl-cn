# 第一章 介绍

Scheme 是一门通用编程语言。它是一门高级语言，它支持对结构化数据进行操作，比如字符串，列表，向量，以及支持操作更传统的数据，比如数字和字符。Scheme 通常被认为适合于符号计算，丰富的数据类型及灵活的控制结构使它成为一种真正的通用编程语言。Scheme 已经被用于编写文本编辑器，优化编译器，操作系统，图形应用，专家系统，数值计算，金融分析，虚拟现实系统以及几乎所有可能的应用类型。Scheme 是一种相当简单易学的语言，因为它基于很少的语法形式和语意概念，并且大多数实现具备的交互性质，鼓励试验性编程。但是，要充分理解 Scheme 是有挑战性的。要在开发中发挥它的潜力需要认真的学习及实践。

在不同机器上的相同版本的 Scheme 实现上，Scheme 程序具有高度的可移植性。因为机器的底层细节几乎完全对程序员隐藏。在不同的 Scheme 仍然具备可移植性，因为 Scheme 的实现者们一直在努力完善语言标准(Revised Reports), 最新的语言标准是"Revise<sup>6</sup>Report", 通过一组标准库和一个标准机制来定义新的可移植库和顶级程序来强调可移植性。

「注：现在最新的是R7RS了，Scheme 被一分为二，分别的 large 和 small. R7RS small已经发布，large 还在制定当中」

早期的一些 Scheme 实现比较低效，然而，现在很多新的基于编译器的实现运行速度和用低级语言（C，汇编）写的程序一样快。 运行时检查可以帮助程序员发现各种错误，虽然有时候会拖慢速度，但是在大多数实现里，这些检查可以被禁用.

Scheme 支持很多数据类型（或者对象），包括字符，字符串，符号，列表及向量对象，以及一套完整的数值类型，包括复数，实数还有任意精度的有理数。

存储对象所需要的内存空间是动态分配和保持的，直到对象不再需要，然后自动释放，通常由垃圾收集器定期地回收不再需要的对象所占据的内存空间。简单的原子值，例如小整数，字符，布尔值以及空列表等，通常处理成立即数，因而不会产生分配和回收的开销。

Regardless of representation，所有对象都是“first-class”对象；因为它们是无限期保留的，它们可以作为参数传递给一个过程，也可以作为过程的返回值 return 回来，或者通过组合形成新的对象。这是和别的语言最大的差别，其它语言的复合数据，比如数组是静态分配的，而且从来不释放，进入一个代码块分配的空间在退出块时无条件地释放，或者由程序员来自己管理释放。

Scheme 是一种 "call-by-value" 语言，即所谓的传值调用。但是，至少可变对象（可以被修改的对象）的值其实是指针，指向真正的存储地址。这些指针隐藏在幕后，程序员不需要关注它们。需要理解的是，当一个对象传递给一个过程或者从过程中返回的时候，并不是对象的拷贝。

Scheme 的核心语法很小，扩展语法是在核心语法的基础上派生而来，核心语法加上扩展语法再加上一组元过程（比如`+ - * /`)构成了完整的 Scheme 语言。一个 Scheme 解释器或编译器可以相当小巧，而且潜在的快速及高度可靠。很多扩展语法及元过程可以由 Scheme 自身定义出来。简化了实现的难度，而且增加了可靠性。

Scheme 代码和数据共享一个相同的打印表示法，这样做的结果是，任何 Scheme 代码都可以自然而然地表示为一个 Scheme 对象。例如，变量及语法关键字可以表示为符号，结构化的语法形式可以表示为列表。这种表示法是 Scheme 提供的语法扩展工具（宏）的基础，通过宏，可以在现有的语法形式及过程的基础上定义出新的语法。它还促进了直接用 Scheme 来实现 Scheme 的解释器，编译器及其它代码转换工具，以及用 Scheme 来实现其它语言转换工具。

Scheme 的变量及关键字是词法作用域的，Scheme 程序使用块结构。可以将标识符导入到一个程序或者库中，或者在一个给定的代码块内部（比如一个库，程序或者函数体）进行局部绑定。局部绑定仅仅是词法可见的，即，程序文本中特定的代码块。块外部出现的相同名字的标识符指向不同的绑定，如果存在于块外部的标识符没有绑定则引用无效。代码块可以嵌套，并且一个块里的绑定可以遮蔽外层的同名绑定。The scope of a binding is the block in which the bound identifier is visible minus any portions of the block in which the identifier is shadowed. 块结构及词法作用域促进了模块化编程，使得程序易读，易维护以及提高可靠性。Efficient code for lexical scoping is possible because a compiler can determine before program evaluation the scope of all bindings and the binding to which each identifier reference resolves. This does not mean, of course, that a compiler can determine the values of all variables, since the actual values are not computed in most cases until the program executes.

在大多数语言里，定义一个过程仅仅是简单地将一块代码与一个名字关联起来，块内部的变量无疑就是过程的参数。

In some languages, a procedure definition may appear within another block or procedure so long as the procedure is invoked only during execution of the enclosing block. In others, procedures can be defined only at top level. In Scheme, a procedure definition may appear within another block or procedure, and the procedure may be invoked at any time thereafter, even if the enclosing block has completed its execution. To support lexical scoping, a procedure carries the lexical context (environment) along with its code.

Furthermore, Scheme procedures are not always named. Instead, procedures are first-class data objects like strings or numbers, and variables are bound to procedures in the same way they are bound to other objects.

As with procedures in most other languages, Scheme procedures may be recursive. That is, any procedure may invoke itself directly or indirectly. Many algorithms are most elegantly or efficiently specified recursively. A special case of recursion, called tail recursion, is used to express iteration, or looping. A tail call occurs when one procedure directly returns the result of invoking another procedure; tail recursion occurs when a procedure recursively tail-calls itself, directly or indirectly. Scheme implementations are required to implement tail calls as jumps (gotos), so the storage overhead normally associated with recursion is avoided. As a result, Scheme programmers need master only simple procedure calls and recursion and need not be burdened with the usual assortment of looping constructs.

Scheme supports the definition of arbitrary control structures with continuations. A continuation is a procedure that embodies the remainder of a program at a given point in the program. A continuation may be obtained at any time during the execution of a program. As with other procedures, a continuation is a first-class object and may be invoked at any time after its creation. Whenever it is invoked, the program immediately continues from the point where the continuation was obtained. Continuations allow the implementation of complex control mechanisms including explicit backtracking, multithreading, and coroutines.

Scheme also allows programmers to define new syntactic forms, or syntactic extensions, by writing transformation procedures that determine how each new syntactic form maps to existing syntactic forms. These transformation procedures are themselves expressed in Scheme with the help of a convenient high-level pattern language that automates syntax checking, input deconstruction, and output reconstruction. By default, lexical scoping is maintained through the transformation process, but the programmer can exercise control over the scope of all identifiers appearing in the output of a transformer. Syntactic extensions are useful for defining new language constructs, for emulating language constructs found in other languages, for achieving the effects of in-line code expansion, and even for emulating entire languages in Scheme. Most large Scheme programs are built from a mix of syntactic extensions and procedure definitions.

Scheme evolved from the Lisp language and is considered to be a dialect of Lisp. Scheme inherited from Lisp the treatment of values as first-class objects, several important data types, including symbols and lists, and the representation of programs as objects, among other things. Lexical scoping and block structure are features taken from Algol 60 [21]. Scheme was the first Lisp dialect to adopt lexical scoping and block structure, first-class procedures, the treatment of tail calls as jumps, continuations, and lexically scoped syntactic extensions.

Common Lisp [27] and Scheme are both contemporary Lisp languages, and the development of each has been influenced by the other. Like Scheme but unlike earlier Lisp languages, Common Lisp adopted lexical scoping and first-class procedures, although Common Lisp's syntactic extension facility does not respect lexical scoping. Common Lisp's evaluation rules for procedures are different from the evaluation rules for other objects, however, and it maintains a separate namespace for procedure variables, thereby inhibiting the use of procedures as first-class objects. Also, Common Lisp does not support continuations or require proper treatment of tail calls, but it does support several less general control structures not found in Scheme. While the two languages are similar, Common Lisp includes more specialized constructs, while Scheme includes more general-purpose building blocks out of which such constructs (and others) may be built.

The remainder of this chapter describes Scheme's syntax and naming conventions and the typographical conventions used throughout this book.