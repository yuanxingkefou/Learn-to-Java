#使用Spring Web Flow

适用于元素按规定流程运行的程序，如会话式的web应用程序

**##在Spring中配置web flow**

目前只能在XML中进行配置，需要在上下文定义XML文件中添加spring配置命名空间

* 装配流程执行器（当用户进入一个流程时，会为用户创建并启动一个流程实例）
  
  `<flow:flow-executor id="flowExecutor" />`
  
* 配置流程注册表（加载流程定义，并让流程执行器可以使用它们）

```
//在base-path目录下查找以"flow.xml"结尾的文件
<flow:flow-registry id="flowRegistry" base-path="/WEB-INF/flows">
    <flow:flow-location-pattern value="*-flow.xml" />
</flow:flow-registry>

//显式声明
<flow:flow-registry id="flowExecutor">
    <flow:flow-location id="flowId" 
          path="/WEB-INF/flows/springflow.xml" />
</flow:flow-registry>
```

* 处理流程请求（需要一个FlowHandlerMapping来帮助DispatcherServlet将流程请求发送给SpringWebFlow,响应请求的是FlowHandlerAdapter)

```
<bean class="org.springframework.webflow.mvc.servlet.FlowHandlerMapping">
    <property name="flowRegistry" ref="flowRegistry" />
</bean>

<bean class="org.springframework.webflow.mvc.servlet.FlowHandlerAdapter">
    <property name="flowExecutor" ref="flowExceutor" />
</bean>
```

**##流程的组件**

**状态**：流程中事件发送的地点

状态类型            |作用                                             |  表现方式
-------------------|-------------------------------------------------|----------------------------
行为（Action)       |流程逻辑发生的地方|<action-state id="">    <evaluate expression="行为状态要做的事" />  </action-state>
决策（Decision)     |将流程分成两个方向，基于流程数据的评估结果确定流程方向|<decision-state id="">  <if test="" then="" else="" /> </decision-state>
结束（End）         |流程的最后一站，一旦进入End状态，流程就会终止|<end-state id="" />
子流程（Subflow)    |会在当前正在运行的流程上下文中启动一个新的流程|<subflow-state id="" subflow=""> <input name="" value="" /> </subflow-state>
视图（View）        |会暂停流程，并邀请用户参与流程|<view-state id="流程内标识名" view="逻辑视图名" model="表单所绑定的对象" />

**转移**：通过转移的方式从一个状态到另一个状态

使用transition元素进行定义，会作为各种状态元素（行为，视图，子进程）的子元素
```
<transition to="nextStatus" />
//基于事件触发：
<transition on="事件" to="nextStatus" />
//抛出异常触发：
<transition on-exception="Exception" to="nextStatus" />

//全局转移（流程中的所有状态默认拥有
<global-transitions>
    <transition on="" to="" />
</global-transition>
```

**流程数据**：

* 声明变量：流程数据保存在变量中，能够以多种方式创建
```
//任意状态进行访问
<var name="" class="" />
//行为状态的一部分或者视图状态的入口
<evaluate result="" expression="" />
//作用域有name的前缀指定
<set name="" value="" />
```

* 定义流程数据的作用域

范围    |生命作用域和可见性
--------|----------------
Conversation|最高层级的流程开始时创建，在最高层级流程结束时销毁，被最高层级的流程和其所有的子流程所共享
Flow        |当流程开始时创建，流程结束时销毁，只有在创建它的流程是可见的
Request     |当一个请求进入流程时创建，在流程返回时销毁
Flash       |当流程开始时创建，流程结束时销毁，在视图状态渲染后，它也会被清除
View        |当进入视图状态时创建，状态退出时销毁，只有在视图状态可见




