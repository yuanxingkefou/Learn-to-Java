#http报文

http使用术语流入和流出来描述事务处理的方向，报文流入源端服务器，工作完成后，会流回用户的Agent代理中

所有报文都会向下游流动

**组成**：起始行，首部，主体（可选）

一组首部总是应该一一个空行（CRLF，回车符加换行符）结束

**请求报文的格式：**
```
<method><request-URL><version>
<headers>
<entity-body>
```
例：
`GET /specials/saw-blade.gif HTTP/1.0
       Host:www.joes-hardware.com`
       
**响应报文的格式：**
```
<version><status-code><reason-phrase>
<headers>
<entity-body>
```
例：
`HTTP/1.0  200 OK
        Content-Type: image/gif
        Content-Length:8572`
