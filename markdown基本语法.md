# 1.标题的用法
添加（#）号
# 2.段落语法
用空白行将文本隔离（不可用空格或tap进行缩进）

第一段

第二段
# 3.换行语法
在一行的末尾添加两个或多个空格，然后按回车键,即可创建一个换行 
# 4.强调语法
要加粗文本，请在单词或短语的前后各添加两个星号  
例：**sb**  
要用斜体显示文本，请在单词或短语前后添加一个星号  
例：*sb*  
要用同时用粗体和斜体突出显示文本，请在单词或短语的前后各添加三个星号  
例：***sb***
# 6.引用语法
要创建块引用，请在段落前添加一个 > 符号。  
>一句话

块引用可以包含多个段落。为段落之间的空白行添加一个 > 符号。  

>第一段
>
>第二段
# 7.列表语法
要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。  
1.第一  
2.第二  
3.第三
# 8.代码语法
要将单词或短语表示为代码，请将其包裹在反引号中  
例：at the command prompt,tupe`nano`  
如果你要表示为代码的单词或短语中包含一个或多个反引号，则可以通过将单词或短语包裹在双反引号中。  
``at the command prompt,tupe`nano`。``
# 9.分割线语法  
要创建分隔线，请在单独一行上使用三个或多个星号 (***)、破折号 (---) 或下划线 (___) ，并且不能包含其他内容。  

**为了兼容性，请在分隔线的前后均添加空白行。**  
  --  
分割线
# 10.链接语法
链接文本放在中括号内，链接地址放在后面的括号中，链接title可选。  
这是一个链接[力扣刷题](https://leetcode.cn/studyplan/)

*给链接增加TITLE*   
链接title是当鼠标悬停在链接上时会出现的文字，这个title是可选的，它放在圆括号中链接地址后面，跟链接地址之间以空格分隔。  
这是一个链接[力扣刷题](https://leetcode.cn/studyplan/ "很好的编程题库")  

*网站和Email地址*  
使用尖括号可以很方便地把URL或者email地址变成可点击的链接。   
<https://leetcode.cn/studyplan/> 

*带格式化的链接*  
强调 链接, 在链接语法前后增加星号。 要将链接表示为代码，请在方括号中添加反引号  
这是一个链接 **[力扣](https://leetcode.cn/studyplan/)**  
# 11.图片语法  
要添加图像，请使用感叹号 (!), 然后在方括号增加替代文本，图片链接放在圆括号里，括号里的链接后可以增加一个可选的图片标题文本。  
![这是一个图片](力扣.jpg "力扣")  
**链接图片**  
给图片增加链接，请将图像的Markdown 括在方括号中，然后将链接添加在圆括号中。  
[![力扣刷题库](力扣.jpg)](https://leetcode.cn/studyplan/)
# 12.转义字符语法
要显示原本用于格式化 Markdown 文档的字符，请在字符前面添加反斜杠字符 \ 。  
\*Without the backslash, this would be a bullet in an unordered list.  
# 13.内嵌HTML标签  
HTML 的行级內联标签如<span><cite>不受限制，可以在 Markdown 的段落、列表或是标题里任意使用。依照个人习惯，甚至可以不用 Markdown 格式，而采用 HTML 标签来格式化。 

word  
<em>word</em>  