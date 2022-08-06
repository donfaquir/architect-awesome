作者：晓得博客
链接：https://www.zhihu.com/question/22916306/answer/1661734440
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


什么是CNAME记录？CNAME记录如何使用
CName记录是Canonical Name的简称，通常称别名指向，CNAME记录可用于将一个域名别名为另一个规范名称的域名系统（DNS）资源记录。
网站是由一组由一组唯一标识的位置（称为IP地址）提供服务的；但是要访问这些站点（例如：晓得博客），我们通常会键入它们对应的域名，这些域名更容易记住。要找到正确的IP地址，您的浏览器将联系域名服务器（DNS），并在其数据库中查询IP地址。
CNAME记录如何使用
例如，假设您有几个子域，例如http://www.mydomain.com，http://ftp.mydomain.com，http://mail.mydomain.com等，并且您希望这些子域指向您的主域名http://mydomain.com。您可以创建CNAME记录，而不是为每个子域创建A记录并将其绑定到您域的IP地址。
如下表所示，如果服务器的IP地址发生更改，则只需更新一个A记录，并且所有子域都会自动更新，因为所有CNAMES都指向带有A记录的主域：

http://mydomain.com指向服务器IP地址，并通过http://www.mydomain.com指向相同的地址http://mydomain.com。如果IP地址发生更改，则只需要在一个地方进行更新即可：只需为修改A记录http://mydomain.com，那么http://www.mydomain.com自动继承更改。
CNAME记录必须始终指向另一个域名，永远不要直接指向IP地址。如果您尝试将CNAME记录指向IP地址，DNSimple的记录编辑器会警告您。CNAME对其他记录必须是唯一的。
CNAME记录局限性
CNAME记录必须始终指向另一个域名，并且永远不要直接指向IP地址。
您不能为主域名（http://mydomain.com）本身创建CNAME记录，该记录必须是A记录。
例如，您不能将http://mydomain.com映射到http://google.com，但是可以将http://google.mydomain.com映射到http://google.com。
使用CNAME记录意味着有一个额外的请求发送到DNS服务器，这可能会导致几毫秒的延迟
一个CNAME记录不能与另一个具有相同名称的记录共存。不能同时有CNAME和TXT记录http://www.example.com。
一个CNAME可以指向另一个CNAME，尽管出于性能原因通常不建议使用此配置。如果适用，CNAME应该尽可能地指向目标名称，以避免不必要的性能开销。