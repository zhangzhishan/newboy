---
title: C++ 0x0a problem
tags: cpp
date: 16 Jan 2017 6:20 PM
---

在`C++`中`fopen`在非二进制工作模式下会自动的把`0x0a`（也即`\n`）转成`0x0d 0x0a`（也即`\r\n`），只有在`fopen`的模式中选择`wb`，其可以避免translate。

但是，即使是不translate在windows平台里面的编辑器等中也会显示为换行。也即，单凭肉眼是分辨不出正常的情况。

另：在高版本的VS中需要使用，`fopen_s(&fp, "file.txt", "wb");`。
## t mode
也即text(translated) mode.

Open in text (translated) mode. In this mode, CTRL+Z is interpreted as an end-of-file character on input. In files opened for reading/writing with `"a+"`, `fopen_s` checks for a CTRL+Z at the end of the file and removes it, if possible. This is done because using `fseek` and `ftell` to move within a file that ends with a CTRL+Z, may cause `fseek` to behave improperly near the end of the file.

Also, in text mode, carriage return–linefeed combinations are translated into single linefeeds on input, and linefeed characters are translated to carriage return–linefeed combinations on output. When a Unicode stream-I/O function operates in text mode (the default), the source or destination stream is assumed to be a sequence of multibyte characters. Therefore, the Unicode stream-input functions convert multibyte characters to wide characters (as if by a call to the `mbtowc` function). For the same reason, the Unicode stream-output functions convert wide characters to multibyte characters (as if by a call to the `wctomb` function).
## b mode
Open in binary (untranslated) mode; translations involving carriage-return and linefeed characters are suppressed.

