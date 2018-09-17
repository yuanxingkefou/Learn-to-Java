#Being a servlet

每个请求运行在一个独立的进程中
The Container runs multiple threads to process multiple requests to a single servlet.

servlet的生命周期开始于Container找到servlet类文件，然后加载这个类（发生在Container开启或第一个客户使用）

**幂等**（idempotent）：在编程中，一个幂等操作的特点是其任意多次执行所产生的影响均与一次执行产生的影响相同

get方法是幂等的；post方法是非幂等的

A simple hyperlink always means a GET.

**Review**:servlet lifecycle 

1）The Container initializes a servlet by loading the class, invoking the servlet’s no-arg constructor, and calling the servlet’s init() method.

2）The init() method (which the developer can override) is called only once in a servlet’s life, and always before the servlet can service any client requests.

3）The init() method gives the servlet access to the Serv- letConfig and ServletContext objects, which the servlet needs to get information about the servlet configuration and the web app.

4）The Container ends a servlet’s life by calling its destroy() method.

5）Most of a servlet’s life is spent running a service() method for a client request.

6）Every request to a servlet runs in a separate thread! There is only one instance of any particular servlet class.

7）Your servlet will almost always extend javax.servlet.http. HttpServlet, from which it inherits an implementation of the service() method that takes an HttpServletRequest and an HttpServletResponse.

8）HttpServlet extends javax.servlet.GenericServlet—an abstract class that implements most of the basic servlet methods.

9）GenericServlet implements the Servlet interface.

10）Servlet classes (except those related to JSPs) are in one
of two packages: javax.servlet or javax.servlet.http.

11）You can override the init() method, and you must override at least one service method (doGet(), doPost(),etc.).

![image]()

请求重定向（redirect)发生在客户端；

请求调度（despatch）发生在服务器端
