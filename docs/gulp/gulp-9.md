## 如何安装插件

您必须在每个机器上执行前一段中必须执行的步骤。但是，您需要为正在处理的每个项目安装插件。

根据您使用的工具，可能会为您自动化。在这种情况下，请检查您正在使用的工具的文档。

如果您只是使用文本编辑器和终端窗口，就像本书的第一章一样，您可以按照以下步骤操作：

1.  创建要在其中创建应用程序的文件夹
2.  打开该文件夹并输入 npm init 。这将在您完成所有设置步骤后创建 **package.json** 文件。在设置过程中，您可以填写每一行，或者您可以通过按**输入**键快速逐步完成每个问题，并在最后回答**是**。
3.  You can now install Gulp and Gulp plugins via the command:

    一个。 npm install gulp --save-dev

    湾 npm install gulp-less --save-dev （例如安装 Gulp 插件 gulp-less ）

| ![](img/00005.jpeg) | 提示：当您预先了解插件时，您也可以进行这样的声明，例如 npm install gulp gulp-less gulp-coffeescript --save-dev 一次安装所有三个模块。 |

| ![](img/00005.jpeg) | 提示：如果您找到示例代码并注意到 package.json 文件中的大量条目，您可以通过键入 npm install --save-dev 来查找所有条目，然后再查找软件包并在本地恢复它们。 |