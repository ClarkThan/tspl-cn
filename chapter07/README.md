# 第七章.输入及输出

所有的输入和输出通过“端口”(port) 进行。端口是指向（可能无限的）数据流（通常是文件）的指针，程序可以通过端口取得字节或字符，或将字节或字符放入流中。端口可以是输入端口，也可以是输出端口，或者同时是输入和输出端口。

和 Scheme 中的其他对象一样，端口是第一级对象。和过程一样，端口没有可打印的字符串和数字表示。有三个初始端口：当前输入端口，当前输出端口以及当前错误端口，分别连接到进程的标准输入，标准输出和标准错误流的文本端口。有几种方式可以打开新的端口。

输入端口通常指向有限的流，例如存储在磁盘上的输入文件。如果使用输入操作（例如 get-u8, get-char 或 get-datum）读取一个已经到达有限流结束的端口，则返回一个特殊的 EOF 对象，谓词`eof-object?`可以用于确定读取操作返回的值是否是 EOF 对象。

端口分为二进制类型和文本类型。二进制端口允许程序从底层的流读取或写入8位无符号字节或多个字节。文本端口允许程序读取或写入字符。

在许多情况下，底层的数据流被组织为一个字节序列，但是这些字节序列需要被视为字符编码对待。在这种情况下，可以使用 `transcoder`(代码转换器) 来创建文本端口，以将字节解码为字符（用于输入），或将字符编码为字节（用于输出）。transcoder封装了用于确定如何将字符表示为字节的编解码器(codec)。系统提供了三种标准的codec：latin-1, Unicode utf-8, 以及 Unicode utf-16。latin-1 使用1个字节编码一个字符，UTF-8使用1-4个字节编码一个字符，UTF-16使用2-4个字节编码一个字符。

transcoder同时封装了行结束符样式，用于确定如何识别行结束符。如果 eol-style 为 none，则不识别行尾。6 种其它标准的 eol-style 如下：

> lf: 	line-feed character  
cr: 	carriage-return character  
nel: 	Unicode next-line character  
ls: 	Unicode line-separator character  
crlf: 	carriage return followed by line feed, and  
crnel: 	carriage return followed by next line


eol-style 以不同的方式对输入和输出操作产生影响。对于输入，所有的 eol-style（none 除外）使得每个行结束符或两个字符序列被转换为单个的行结束符。对于输出，所有的 eol-style（none 除外）导致单个的行结束符转换成与 eol-style 相关的一个或两个字符序列。在输入方向，所有的 eol-style 是等价的（none 除外），在输出方向，`none` 和 `lf` 是等价的。

除了 codec 和 eol-style 外，transcoder还封装了一个其他信息：`error-handling-mode`(错误处理模式)。它确定如果发生解码或编码错误进怎么办。即，在输入方向上，一个字节序列无法被所封装的codec转换为一个字符，或者在输出方向上，一个字符无法被codec转换为一个字节序列。错误处理模式有：忽略，抛出，以及替换。在忽略模式下，不合法的字符或字节序列会被忽略；在抛出模式下，会抛出一个 i/o-decoding 或者 i/o-encoding 异常；在替换模式下，出错的字符或字符编码会被预设的字符替换掉：在输入方向上，替换字符是 U+FFFD，而在输出方向上，在 utf-8 或 utf-16 编码下会使用 U+FFFD 替换，在latin-1 编码下会用问号(?) 替换。

端口可以被缓冲以提高效率，消除对于每个字节或字符都对操作系统进行系统调用的开销。系统支持3种标准的缓冲模式：block, line 以及 none。在块缓冲模式下，将使用特定大小的数据块（取决于所使用的 Scheme 实现）从流中读取输入，或者将数据块发送到输出流。在行缓冲模式下，以行为单位进行缓冲（或者其他与实现相关的方式）。行缓冲与块缓冲有显著的区别，行缓冲通常仅用于文本输出端口，因为二进制端口中没有行的划分，而对于输入端口，有可能在输入流变得可用时立即读取。如果缓冲模式设为 none 则不执行缓冲，所有的输入立即发送到流，而输入只根据具体的需要进行读取。

本章剩下的部分包括了transcoder操作，文件端口，标准端口，字符串端口， bytevector 端口，自定义端口，通用端口操作，输入操作，输出操作，简便I/O，文件系统操作，以及 bytevector 与字符串之间的转换。
