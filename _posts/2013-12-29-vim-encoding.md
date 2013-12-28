---
layout: post
title: vim encoding
---
起因是因为写了个脚本用来监控我的游戏人物角色金币的：
	#! /bin/bash
	
	curl -i --cookie ck_${1}.cookie http://123.196.114.171/feb/equip.php 2>/dev/null | grep --color 麻花
	
然而，却一直报`grep: illegal byte sequence`的错误.

但是在shell上直接敲上面命令却是可以通过的，于是怀疑是引入了`麻花`这两个中文的缘故，果然换成英文就可以了。

检查了下terminal的字符集是设置成utf-8的，于是得出结论：	脚本的编码不是utf-8.

仔细看了下vim的配置：
	set encoding=utf-8
	set fileencoding=prc " same as euc-cn
	set fileencodings=utf-8,gb2312,gbk,gb18030

显然，罪魁祸首是`set fileencoding=prc`，因为这导致了存储保存编码为prc而不是utf-8

### 看完帮助文档的总结:
- **encoding**
  enc，表示vim处理时使用的编码格式，加入文件存储在磁盘的格式与encoding值不同，那么就会转换成这个格式，官方建议值utf-8
- **fileencoding**
  fenc，表示vim认为的文件格式，当打开一个已有的文件时，这个变量值取决于fileencodings，如果fileencodings没有认出来，fenc就取手工设置的值，如果没有赋予初值，那fenc就取enc的值，fenc的值很重要是因为他将决定文件保存到磁盘里的文件编码。如果时新建的文件，那么fenc值就取vimrc里设置的初值，这就是为什么我的脚本文件保存成prc格式。
- **fileencodings**
  fencs，其值为list，代表打开已有文件时，vim猜测文件编码的顺序。当vim认为属于某个编码时，就会将fenc也设置为猜测的编码值，这样存储时也会保证和原编码一致。

### tips:
	可以在打开文件后，手动运行:set fenc=XXXX以改变保存时希望使用的文件编码。
	当然也可以使用iconv等工具改变编码：）

### TODO：
	文件编码好多，看到vim帮助文档直接列举了如下这些点，有空要了解一下:)
	1   latin1	8-bit characters (ISO 8859-1, also used for cp1252)
	1   iso-8859-n	ISO_8859 variant (n = 2 to 15)
	1   koi8-r	Russian
	1   koi8-u	Ukrainian
	1   macroman    MacRoman (Macintosh encoding)
	1   8bit-{name} any 8-bit encoding (Vim specific name)
	1   cp437	similar to iso-8859-1
	1   cp737	similar to iso-8859-7
	1   cp775	Baltic
	1   cp850	similar to iso-8859-4
	1   cp852	similar to iso-8859-1
	1   cp855	similar to iso-8859-2
	1   cp857	similar to iso-8859-5
	1   cp860	similar to iso-8859-9
	1   cp861	similar to iso-8859-1
	1   cp862	similar to iso-8859-1
	1   cp863	similar to iso-8859-8
	1   cp865	similar to iso-8859-1
	1   cp866	similar to iso-8859-5
	1   cp869	similar to iso-8859-7
	1   cp874	Thai
	1   cp1250	Czech, Polish, etc.
	1   cp1251	Cyrillic
	1   cp1253	Greek
	1   cp1254	Turkish
	1   cp1255	Hebrew
	1   cp1256	Arabic
	1   cp1257	Baltic
	1   cp1258	Vietnamese
	1   cp{number}	MS-Windows: any installed single-byte codepage
	2   cp932	Japanese (Windows only)
	2   euc-jp	Japanese (Unix only)
	2   sjis	Japanese (Unix only)
	2   cp949	Korean (Unix and Windows)
	2   euc-kr	Korean (Unix only)
	2   cp936	simplified Chinese (Windows only)
	2   euc-cn	simplified Chinese (Unix only)
	2   cp950	traditional Chinese (on Unix alias for big5)
	2   big5	traditional Chinese (on Windows alias for cp950)
	2   euc-tw	traditional Chinese (Unix only)
	2   2byte-{name} Unix: any double-byte encoding (Vim specific name)
	2   cp{number}	MS-Windows: any installed double-byte codepage
	u   utf-8	32 bit UTF-8 encoded Unicode (ISO/IEC 10646-1)
	u   ucs-2	16 bit UCS-2 encoded Unicode (ISO/IEC 10646-1)
	u   ucs-2le	like ucs-2, little endian
	u   utf-16	ucs-2 extended with double-words for more characters
	u   utf-16le	like utf-16, little endian
	u   ucs-4	32 bit UCS-4 encoded Unicode (ISO/IEC 10646-1)
	u   ucs-4le	like ucs-4, little endian

滚去碎觉了
