---
layout: post
title:  "Java Apache HttpComponents"
image: ''
date:   2019-02-05 15:00:00
tags:
- Java
- TIL
description: ''
categories:
- Java
---

<br/>
<br/>

## http components
- HTTP 프로토콜을 사용할 수 있도록 도와주는 lib.
- 기존의 간단한 HttpClient와는 다르게 여러 더 세부적인 옵션들을 Builder로 설정할 수 있습니다.
- 쿠키, 권한, dns, socket, redirect 전략 등 여러 세부 설정을 할 수 있게 변경되었습니다.

- HttpCore 모듈 : HTTP프로토콜을 이용해서 데이터를 전송할 수 있는 로우 레벨의 기능을 제공
- HttpClient 와 HttpAsyncClient 모듈 : HttpCore 컴포넌트와 결합해서 클라이언트 역할을 할 수 있는 API 제공,
  HttpClient와 HttpAsyncClient 모듈의 차이는 전통적인 blocking IO를 사용하느냐 아니면 NIO기능을 사용하느냐이다. 

## HttpClient의 특징
1. Client side 동작을 구현한 라이브러리
  (서버에게 HTTP request를 던지고 HTTP response를 받을 수 있다. 즉 서버로부터 데이터를 받아 활용할수 있다.)
2. HTTP의 명령어에 따른 클래스를 제공
  (HttpGet, HttpHead, HttpPost, HttpPut, HttpDelete, HttpTrace, HttpOption)
3. 편리하고 쉬운 HTTP 개발을 할 수 있다.

## 필수 라이브러리
- commons-logging-x.x.x.jar
- commons-codec-x.x.x.jar
- httpcore-x.x.x.jar
- httpclient-x.x.x.jar

## 로그인 후 파일 다운로드 하는 예제
{% highlight javascript %}
    public static void loginFileDownload(String loginPath, String downloadPath, String username, String password, String fileName) throws IOException {
        
        int byteRead = 0;
        int byteWritten = 0;

        CloseableHttpResponse response;
        
        // 로그인을 위한 CloseableHttpClient 선언
        CloseableHttpClient client = HttpClients.createDefault();
        
        // 로그인 경로 지정
        HttpPost post = new HttpPost(loginPath);
        
        // 파일 다운도르 경로 지정
        HttpGet get = new HttpGet(downloadPath);
        
        post.setHeader("User-Agent", USER_AGENT);
        
       // ID, PW 설정
        List&lt;NameValuePair&gt; nameValuePair = new ArrayList&lt;NameValuePair&gt;();
        nameValuePair.add(new BasicNameValuePair("username", username));
        nameValuePair.add(new BasicNameValuePair("password", password));
        
        // post 통신으로 EURO 페이지 로그인.
        post.setEntity(new UrlEncodedFormEntity(nameValuePair));
        
        // get 통신으로 파일 다운로드
        response = client.execute(get);
        
        InputStream is = (InputStream) response.getEntity().getContent();
        
        // 다운로드 되는 경로 설정.
        File file = new File(getFilePath("transBefore") + sep + fileName);
        OutputStream os = new FileOutputStream(file);
        
        byte[] buf = new byte[size];
        while ((byteRead = is.read(buf)) != -1) {
        	os.write(buf, 0, byteRead);
        	byteWritten += byteRead;
        }
        
        logger.debug("|===== File name : " + fileName);
        logger.debug("|===== of bytes  : " + byteWritten);
        
        os.close();
        response.close();
        client.close();
        
    }
{% endhighlight %}

## 참고
<a href="https://m.blog.naver.com/PostView.nhn?blogId=javaking75&logNo=220341248215&categoryNo=7&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F">Apache HTTP 컴포넌트</a>