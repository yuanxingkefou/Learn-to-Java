#XML(Extensible Markup Language可扩展标记语言）

**结构**

必须以一个文档头开始,如：
`<?xml version="1.0" encoding="UTF-8"?>`

通常然后是文档类型定义（DTD），如：
```
<!DOCTYPE web-app PUBLIC "-//Sun Microsystesms, Inc.//DTD web Application 2.2//EN"
              "http://java.sum.com/j2ee/dtds/web-app_2_2.dtd">
```

然后是正文，正文包含根元素，根元素包含其他元素，元素可以有子元素，文本或两者皆有，元素可以有属性

**解析XML文档**

* 树型解析器，如DOM解析器，将读入的XML解析成树结构


* 流机制解析器，在读入XML时，生成相应的事件

**验证XML文档**

`<!ELEMENT configuration ...>`用来指定某个元素可以拥有什么样的子元素

`<!ATTLIST element attribute type default>`可以用来指定合法的元素属性的规则

**使用XPath来定位信息**

```
XPathFactory xpfactory=XPathFactpry.newInstance();
path=xpfactory.newXPath();
//调用evaluate方法来计算XPath表达式：
String username=path.evaluate("/configuration/database/username",doc);
```

**使用命名空间**

**流机制解析器**

当文档很大，并且处理算法很简单，可以在运行时解析节点，而不必看到完整的树形结构，应该使用流机制解析器

* SAX解析器

* StAX解析器

**生成XML文档**

XSL转换



