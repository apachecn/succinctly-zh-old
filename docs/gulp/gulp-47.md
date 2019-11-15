## 捆绑和缩小

在以前的 ASP.NET 版本中，我们引入了 System.Web.Performance 命名空间，这有助于缩小和捆绑 CSS 和 JavaScript 文件。这样做的好处是通过缩小使文件的大小更小。通过将文件捆绑在一起成为一个文件，还可以减少要下载的并发文件数。这是个好消息，因为浏览器只允许少量这些同时使用。我总是发现[这篇文章](http://www.asp.net/mvc/overview/performance/bundling-and-minification)是对这个特定主题的一个很好的介绍。完成本书后，请花些时间阅读。