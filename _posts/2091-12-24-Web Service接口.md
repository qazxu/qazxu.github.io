---
layout: mypost
title: Web Service接口
categories: [java]
---

### web Service概念

Web service使用与平台和编程语言无关的方式进行通讯的一项技术, web service 是一个接口, 他描述了一组可以在网络上通过标准的XML消息传递访问的操作, 它基于xml语言协议来描述要执行的操作或者要与另外一个web 服务交换数据, 一组以web服务在面向服务体系结构中定义的web应用程序。

可以简单的理解为web service是一个SOA(面向服务的编程)架构, 它不依赖于语言, 也不依赖于平台, 可以实现不同语言之间的通讯和相互调用.SOAP(简单对象访问协议) 是xml web service的通讯协议。当用户通过UDDI找到WSDL(Web Service Description Language)文档后,通过SOAP调用建立的web service的一个或者多个操作.SOAP是xml文档形式的调用方法规范, 可以支持不同的底层接口。

**总结就是soap请求是HTTP POST的一个专用版本，遵循一种特殊的xml消息格式Content-type设置为: text/xml任何数据都可以xml化。**

### 建立请求

首先需要建立一个http连接，在请求时可以设置权限信息（如果有的话）、请求方式、并且设置对应的请求头信息

```java
	import java.io.BufferedReader;
	import java.io.BufferedWriter;
	import java.io.IOException;
	import java.io.InputStream;
	import java.io.InputStreamReader;
	import java.io.OutputStreamWriter;
	import java.net.HttpURLConnection;
	import java.net.URL;
	import org.dom4j.DocumentException;

	public class ErpHttpRequest {
		//请求地址
		private static final String REQUEST_URL = "xxxx;
		//用户名
		private static final String USERNAME = "XXX";
		//密码
		private static final String PASSWORD = "XXX";
		
		//post请求
		public static String httpPost(String requestXml) throws IOException, DocumentException{
			URL url = new URL(REQUEST_URL);
			HttpURLConnection connection = (HttpURLConnection)url.openConnection();
			//打开输入输出的开关
			connection.setDoInput(true);
			connection.setDoOutput(true);
			//设置请求方式
			connection.setRequestMethod("POST");
			//设置请求权限验证
			String input = USERNAME + ":" + PASSWORD;
			String encoding = new sun.misc.BASE64Encoder().encode(input.getBytes());
			connection.setRequestProperty("Authorization", "Basic " + encoding);
			//设置请求的头信息
			connection.setRequestProperty("Content-type", "application/xml; charset=utf-8");
			//获得输出流
			BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(connection.getOutputStream(), "UTF-8"));
			//发送数据
			writer.write(requestXml);
			writer.close();
			System.out.println(requestXml);
			System.out.println(connection.getResponseCode());
			StringBuffer sb = new StringBuffer("");
			//判断请求成功
			if(connection.getResponseCode() == 200){
				InputStream in = connection.getInputStream();
				BufferedReader reader = new BufferedReader(new InputStreamReader(in));
				String line = null;
				while((line = reader.readLine()) != null){
					sb.append(line);
				}
				System.out.println(sb.toString());
			}
			return sb.toString();
		}
	}
```

### 报文格式

下面是部分报文
```xml
 <InputParameters>
  <ns:P_IFACE_CODE></ns:P_IFACE_CODE>
  <ns:P_BATCH_NUMBER></ns:P_BATCH_NUMBER>
  <ns:P_REQUEST_DATA>
   <PARAMETER>
    <G_RCV_HEADERS>
     <RCV_HEADER>
      <BATCH_NUMBER></BATCH_NUMBER>
      <MES_SHIPMENT_NUMBER></MES_SHIPMENT_NUMBER>
```

### 解析报文
解析报文使用的是dom4j，这步看官方文档好理解，关键在于解析过程需要对节点进行逐层遍历读取和赋值。麻烦-_-"

```java
SAXReader reader = new SAXReader();
Document   document = reader.read(PropertiesUtils.class.getClassLoader().getResourceAsStream("requestXML/MES-RCV-004.xml"));
Element root=document.getRootElement();  
Element memberElm = root.element("InputParameters");
			
// 读取更改批次号
Element numberElm = memberElm.element("P_BATCH_NUMBER");
numberElm.setText("批次号赋值");

String docXmlText = document.asXML();
ErpHttpRequest.httpPost(docXmlText);
```

另外，也可以采用反向生成的方式，但为此会多出很多代码，项目会显得很臃肿，不推荐。(具体可以看下方的Webxml)


### 参考
- [Dom4j完整文档](https://blog.csdn.net/qq_38584967/article/details/90040429)
- [我的第一次Webservice接口开发](https://blog.csdn.net/qq_38584967/article/details/90040429)
- [Webxml](http://www.webxml.com.cn/zh_cn/web_services_item.aspx?id=776756327947797A706B413D)