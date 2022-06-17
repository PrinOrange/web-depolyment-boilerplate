# Next.js 在Linux服务器上的部署样板配置

## 解决什么问题？
这份指南给出了在Linux服务器下部署的样板配置，根据操作指示可以快速上手完成部署工作。
Next.js项目自带服务器，所以要访问Next.js渲染的网站，需要通过代理转发来自客户端的请求到Next.js-Server的端口。所以本指南也准备了针对Apache、Nginx服务器的配置。

## 需要什么准备？
本指南假定你可以在本机上能运行build后的next.js app。

## 创建nextjs-server守护进程

- 编辑本目录下的nextjs.service。对于一些字段，在文件注释中已给出提示。对于`ExecStart`字段，需要填入包管理器或者脚本执行器的程序位置。如果不记得，可以在terminal中执行`which + 名称`来查看位置。例如`which npm`。对于更多字段，可以参考[鸟哥Linux私房菜](https://zq99299.github.io/linux-tutorial/tutorial-basis/17/03.html#unit-%E9%83%A8%E5%88%86)
- 将编辑好的文件移动到/lib/systemd/system位置下。执行命令：
  ```
  sudo cp nextjs.service /lib/systemd/system/nextjs.service
  ```
- 在终端中执行命令
  ```
  service nextjs start run start
  ```
  然后观察服务状态
  ```
  systemctl status nextjs
  ```
  可知是否成功。如果你的Linux带有图形界面，可以在浏览器中打开[http://localhost:3000](http://localhost:3000),即可浏览Next.js项目。

## 设置服务器代理

### Nginx配置：
将本目录下的nginx-boilerplate内容复制到nginx配置文件中指定的Server块中。注意路由块优先级。
如果自有要求，也可以对路由块配置适量更改。