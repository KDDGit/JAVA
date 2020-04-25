# Web 交互方式

      《web异步与实时交互iframe、AJAX、WebSocket开发实践》
      
使用iframe、AJAX、WebSocket通过**轮询**、**长轮询**、**长连接**和**推送**方式，实现Web交互

### 1、[介绍异步实时Web交互技术](#介绍异步实时Web交互技术)

### 2、[介绍 frame](#介绍frame)

### 3、[讲解 JAX](#讲解JAX)

### 4、[讲解 WebSocket](#讲解WebSocket)

### 5、[对比分析](#对比分析)

## 介绍异步实时Web交互技术

基于HTTP协议的Web交互（缺点：1、需要整页刷新、2、速度慢、3、有延迟）

Web异步交互技术 iframe、AJAX、WebSocket           
iframe 模拟异步传输，能解决缺点1、2(使用的是HTTP协议)        
AJAX 通过异步通信和响应，基本上能解决缺点1、2、3（使用的是HTTP协议）  
WebSocket 新的WebSocket协议，全双工通信连接，能解决缺点1、2、3，又可实现，服务端向客户端推送消息


HTTP通信步骤：         
1、HTTP基于TCP协议，HTTP开始前，TCP层会通过“三次握手”建立TCP连接，建立后HTTP就可传输了           
2、客户端向服务器发送请求           
3、服务器收到请求，处理后，返回响应信息          
4、客户端收到返回信息，通过浏览器显示，然后断开连接          


Web实时交互方式：轮询、长轮询、长连接、推送

轮询：客户端定时发送请求，服务端收到请求后，马上响应，并关闭连接          
长轮询：客户端发送请求，服务端收到请求后，阻塞，并保持连接，当服务端有数据后，响应并关闭连接        
长连接：客户端发送请求，服务端收到请求后，阻塞，并保持连接，当服务端有数据后，响应并保持连接        
推送：客户端和服务端建立连接后，服务端和客户端相互推送数据       


## 介绍frame

暂略

## 讲解JAX

暂略

## 讲解WebSocket

WebSocket协议通信机制：WebSocket协议是独立的基于TCP的协议

通信模式： 
浏览器先向服务器发起一个HTTP请求。请求包含附加头信息，是一个升级后的HTTP请求，服务器解析附加头信息后，产生应答信息返回给浏览器，这样WebSocket连接建立完成。双发可通过这个连接自由传输数据，直到一方关闭连接

WebSocket的握手细节（详见书）

数据帧传输格式细节（详见书）

WebSocket协议通信实现相关技术           
构造函数：var ws = new WebSocket("ws//127.0.0.1:8080/WebSocket/WebSocket")         
WebSocket事件：open、message、error、close            
WebSocket方法：send()方法、close()方法            

简单案例        
客户端
```html
<!DOCTYPE HTML>
<html>
<head>
</style>
<script type="text/javascript">
      var ws = null;
      function setConnected(connected){
            document.getElementById('connect').disabled=connected;
            document.getElementById('disconnect').disabled=!connected;
            document.getElementById('echo').disabled=!connected;
      }
      function connect(){
            if("websocket" in window){
                  ws = new WebSocket("ws//127.0.0.1:8080/WebSocket/WebSocket");
            }else{
                  alert('浏览器不支持WebSocket');
                  return ;
            }
            ws.open = function(){setConnected(true);};
            ws.onmessage = function(e){log('接收：'+e.data);};
            ws.onclose = function(){setConnected(false);};
      }
      function disConnect(){
            if(ws != null){
                  ws.close();
                  ws=null;
            }
            setConnected(false);
      }
      function echo(){
            if(ws != null){
                  var message = document.getElementById('message').value;
                  log('发送:'+message);
                  ws.send(message);
            }else{
                  alert('WebSocket未连接');
            }
      }
      function log(message) { 　
            var console = document.getElementById('console'); 　
            var p = document.createElement('p'); 　
            p.style.wordWrap = 'break-word';　  
            p.appendChild(document.createTextNode(message));　 　
            console.appendChild(p);   　
            while (console.childNodes.length > 25) { 　 
                  console.removeChild(console.firstChild); 
            }   　
            console.scrollTop = console.scrollHeight; 
      } 
      </script> 
      </head> 
<body>  
      <p>WebSocket Demo</p >    
      <input id="connect" type="button"　value="连接" onclick="connect()" />  
      <input id="disconnect"　type="button"　disabled="disabled" value="断开连接" 　onclick="disconnect();" />  
      <br>  
      <textarea id="message" style="width: 350px"></textarea>  
      <br>  
      <input id="echo" type="button" onclick="echo( );" value="发送" 　disabled="disabled">  
      <div id="console"></div> 
</body> 
</html>

```

服务端
```java
package com.geek1994.Websocket; 
import java.io.IOException; 
import javax.Websocket.OnClose; 
import javax.Websocket.OnMessage; 
import javax.Websocket.OnOpen; 
import javax.Websocket.Session; 
import javax.Websocket.server.ServerEndpoint;  
// @ServerEndpoint注解是一个类层次的注解，将当前的类定义成一个WebSocket服务器端，其中的值将被用于监听用户连接的终端。 @ServerEndpoint("/Websocket")　  
public class WebSocketDemo {  
      @OnMessage　  
      public void onMessage(String message, Session session) { 　
            try { 　　　　　　
                  if (session.isOpen()) { 　　　　　　 
                        System.out.println("发送消息"+message); 　　　　　　　　
                        session.getBasicRemote().sendText(message); 　　　　　　
                   } 　　　　
             } catch (IOException e) { 　　　　　　
                  try { 　　　　　　　　
                        session.close(); 　　　　　　
                   } catch (IOException e1) { 　　　　　　　　 // 将异常打印在控制台上　　　　　 
                        e.printStackTrace();  　　　　　　
                   } 　　　　
             }  
      } 　
      
      @OnOpen  
      public void onOpen() { 　
            System.out.println("open");  
      }  
      @OnClose  
      public void onClose() { 　
            System.out.println("close");  
       } 
 }

```



## 对比分析



