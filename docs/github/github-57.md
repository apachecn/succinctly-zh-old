## GitHub Markdown

除了标准的 Markdown 构造之外，GitHub 还为 GitHub 环境提供了额外的增强功能。

### 下划线

GitHub 中的下划线字符不用于组合**粗体**和 _ 斜体 _ 文本，因为下划线通常用于代码中。 GitHub 将忽略单词之间的下划线字符，并将它们视为文本的一部分。

### 网址

如果 GitHub 检测到 URL 模式，例如 http://github.com ，它会自动将其视为超链接。您仍然可以使用 Markdown 语法命名超链接，但是如果要显示 URL，则直接键入 URL 并让 GitHub 显示它会容易得多。

### 删除线文字

由于添加和删除是 GitHub 代码显示的重要组成部分，因此您可以使用波浪号字符（ ~~ ）来表示删除线文本。例如：

~~删除货币符号~~

此语法在 GitHub 中显示如下：

**删除了货币符号**

### 语法高亮

您可以使用三重反引号语法来显示代码部分，但 GitHub 会增加额外的扭曲。通过在第一个反引号头之后指定语言名称，GitHub 将对关键字进行颜色编码并提供其他语法突出显示功能。通过添加 JavaScript 作为语言，GitHub 将此代码呈现为：

```JavaScript

if（x == 1）{

Console.write（“EXPECTED MULTIPLE ROWS”）;

}

在 GitHub 中：

```
if (x==1) {
         Console.write("EXPECTED MULTIPLE ROWS")
}

```

这有助于开发人员，因为代码看起来可能与他们的开发工具类似。

### 用户和问题

GitHub 还允许＃用于链接到现有的发行号，而 @ 用于指定用户名。