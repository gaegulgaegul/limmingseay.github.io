---
layout: post
title:  "Java Jsoup"
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

## Jsoup
- Jsoup lib 필요
- html/xml 데이터 크롤링 기능 제공

## Document
- html/xml 파일을 관리하는 클래스

## Elements
- html/xml 데이터를 관리하는 클래스
- method
    - select([tagName]) : id, class, tag 등 해당 태그의 속성을 읽는다.
    - attr([attributeName]) : 해당 속성에 대한 값을 반환한다.
    - text() : 태그의 값을 반환한다.
    - html() : html 태그를 반환한다.
    - empty() : 해당 태그를 삭제한다.

## 파일을 읽어 데이터를 가져오는 예제
{% highlight javascript %}
    List list = new ArrayList();

    File input = new File("parseFile.xml");
    
    // xml parsing.
    Document reviseDoc = Jsoup.parse(input, "", Parser.xmlParser());

    // 상위 Element 선언
    Elements sanctionEntity = reviseDoc.select("sanctionEntity");
        
    // sanctionEntity tag
    for(int i=0; i&lt;sanctionEntity.size(); i++) {
            
        // regulation tag
        Elements regulation = sanctionEntity.get(i).select("regulation");
        for(int j=0; j&lt;regulation.size(); j++) {
            Map map = new hashMap();
            map.put("select", regulation.get(j).select("publicationUrl").text());
            map.put("attr", sanctionEntity.get(i).attr("logicalId"))
            list.add(map);
        }
          
    }
{% endhighlight %}

## 페이지를 읽어 데이터를 가져오는 예제
{% highlight javascript %}
    List list = new ArrayList();

    Document doc = Jsoup.connect("http://url.com").get();
        
    // A~Z까지 html파일명을 가져온다.
    Elements htmlFile = doc.select("#cosCountryList li a");
    String[] fileName = new String[htmlFile.size()];
    for(int i=0; i&lt;htmlFile.size(); i++) {
      	fileName[i] = htmlFile.get(i).attr("href");
    }
        
    // 각 html파일의 태그를 읽어온다. 
    for(int j=0; j&lt;fileName.length; j++) {
      	String html = "http://urlPath/  +  fileName[j];
       	Document htmlTag = Jsoup.connect(html).get();
        	
       	String country = htmlTag.select(".countryName span").text();
        	
       	// 데이터 저장
       	Elements chiefsOutput = htmlTag.select("#chiefsOutput");
       	for(int k=0; k&lt;chiefsOutput.size(); k++) {
       		Map map = new HashMap();
            map.put("1", country)
            map.put("2", chiefsOutput.get(k).select(".title span").text());
            map.put("3", chiefsOutput.get(k).select(".cos_name span").text());
            list.add(map);
       	}
    }
{% endhighlight %}
