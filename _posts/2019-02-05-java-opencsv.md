---
layout: post
title:  "Java Open CSV"
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

## opencsv
- csv 파일을 읽고 쓸 수 있는 기능을 제공한다.
- writeAll()로 한번에 데이터를 파일에 쓸 수 있다.

## csv 파일을 생성하는 예제
{% highlight javascript %}
    /**
     * csv 파일에 데이터를 입력하여 파일을 생성하는 메서드
     * 
     * @param paramList
     * @param saveFile
     * @throws IOException 
     */
	public static void createCsvFormData(List&lt;Map&lt;String, Object&gt;&gt; paramList, String saveFile) throws IOException {
		List&lt;String[]&gt; dataList = transformMapToArray(paramList);
		CSVWriter cw = new CSVWriter(new FileWriter(saveFile));
		cw.writeAll(dataList);
		cw.close(); // 반드시 close()를 해줘야 파일에 데이터가 입력된다.
		logger.debug("|=====  " + saveFile + " 생성!!");
	}
	
    /**
     * map으로 입력받은 데이터를 String[]으로 변환하는 메서드
     * 
     * @param paramList
     */
	private static List&lt;String[]&gt; transformMapToArray(List&lt;Map&lt;String, Object&gt;&gt; paramList) {
		Iterator&lt;Map&lt;String, Object&gt;&gt; itr = paramList.iterator();
		Map&lt;String, Object&gt; param = null;
		List&lt;String[]&gt; dataList = new ArrayList<>();
		List&lt;String&gt; data = null;
		
		// 헤더 입력
		if(itr.hasNext()) {
			param = itr.next();
			data = new ArrayList<>();
			for(String key : param.keySet()) {
				data.add(key);
			}
			dataList.add(Arrays.copyOf(data.toArray(), data.toArray().length, String[].class));
		}
		
		// 데이터 입력
		itr = paramList.iterator();
		while(itr.hasNext()) {
			param = itr.next();
			data = new ArrayList<>();
			for(String key : param.keySet()) {
				data.add(String.valueOf(param.get(key)));
			}
			dataList.add(Arrays.copyOf(data.toArray(), data.toArray().length, String[].class));
		}
		
		return dataList;
	}
{% endhighlight %}