#查找含有文字的文件
[grep详解](http://billie66.github.io/TLCL/book/zh/chap20.html)
You can user the -n option of xargs to limit the number of arguments
`find . -name "*" | xargs -n 20 grep 'state-icons'`

<table class="multi">
<caption class="cap">grep 选项</caption>
<tbody><tr>
<th class="title">选项</th>
<th class="title">描述</th>
</tr>
<tr>
<td valign="top" width="20%">-i</td>
<td valign="top">忽略大小写。不会区分大小写字符。也可用--ignore-case 来指定。 </td>
</tr>
<tr>
<td valign="top">-v</td>
<td valign="top">不匹配。通常，grep 程序会打印包含匹配项的文本行。这个选项导致 grep 程序
只会不包含匹配项的文本行。也可用--invert-match 来指定。 </td>
</tr>
<tr>
<td valign="top">-c</td>
<td valign="top">打印匹配的数量（或者是不匹配的数目，若指定了-v 选项），而不是文本行本身。
也可用--count 选项来指定。 </td></tr>
<tr>
<td valign="top">-l</td>
<td valign="top">打印包含匹配项的文件名，而不是文本行本身，也可用--files-with-matches 选项来指定。</td>
</tr>
<tr>
<td valign="top">-L</td>
<td valign="top">相似于-l 选项，但是只是打印不包含匹配项的文件名。也可用--files-without-match 来指定。</td>
</tr>
<tr>
<td valign="top">-n</td>
<td valign="top">在每个匹配行之前打印出其位于文件中的相应行号。也可用--line-number 选项来指定。</td>
</tr>
<tr>
<td valign="top">-h</td>
<td valign="top">应用于多文件搜索，不输出文件名。也可用--no-filename 选项来指定。 </td>
</tr>
<tr>
<td>-s, --no-messages</td>
<td>Silent mode.  Nonexistent and unreadable files are ignored (i.e.
their error messages are suppressed).</td>
</tr>
</tbody></table>
