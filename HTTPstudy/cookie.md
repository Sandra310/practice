# cookie机制
### cookie的类型
* 会话cookie： 临时记录用户访问站点时的设置和偏好，用户退出浏览器则被删除
* 持久cookie： 持久维护用户会周期性访问的站点的配置文件或登录名。存储在硬盘上，重启电脑仍存在
### cookie是如何工作的
用户首次访问时，Web服务器对用户一无所知，所以想给这个用户一个独有的cookie（Set-cookie:id="34294";domain="xxx"），下次就能识别这个用户了。下次用户访问时，就能用id="34294"来查找服务器为其积累的数据，如购物历史、地址信息等。
### cookie的存储
不同浏览器存储方式不同。
* 网景的Navigator的cookie<br>
存储在cookie.txt文件中，每一行代表一个cookie，有7个用tab键分割的字段
domain（cookie的域）、allh（域中所有主机都能获取还是指定了名字的）、path（域中与cookie相关的路径前缀）、secure（是否只有在SLL连接时才发送cookie）、expiration（过期时间）、name、value
* IE
### 不同站点使用不同cookie
浏览器只向服务器发送该服务器产生的cookie。<br>
* cookie的域属性
产生cookie的服务器可以向Set-Cookie响应首部t添加Domain属性控制哪些站点可以看到cookie
* cookie的路径属性
path属性可以设置cookie只能用于部分Web站点  path= /autos/



