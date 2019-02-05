---
layout: post
title:  "Java ZIP File"
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


## 파일 압축
- 사용 클래스
    - ZipOutputstream
    - ZipEntry

## 예제
{% highlight javascript %}
    /**
     * 해당 경로를 압축한다.
     * 
     * @param savePath
     * @param filename
     * @throws Exception
     */
    public static void createZipFromFile(String savePath, String filename) throws Exception {
    	// 기존에 같은 이름의 파일이 있으면 삭제
    	File isFile = new File(savePath + sep + filename);
    	if(isFile.exists()) {
    		isFile.delete();
    	}
    	
    	File folder = new File(savePath);
    	String[] listOfFile = folder.list();
    	
    	ZipOutputStream out = new ZipOutputStream(new FileOutputStream(savePath + sep + filename));
    	
    	for(String file : listOfFile) {
    		File fullPathFile = new File(savePath + sep + file);
    		if(fullPathFile.isDirectory()) {
    			fullPathFile = new File(savePath + sep + file + sep);
    		}
    		
    		compressFullPath("", fullPathFile, out);
    	}
    	out.close();
    	
    	logger.debug("|=====  " + savePath + sep + filename + " 생성!");
    }
    
    /**
     * ZipOutputStream를 넘겨 받아서 하나의 압축파일로 만든다.
     * 
     * @param parentPath
     * @param file
     * @param out
     * @throws Exception
     */
    private static void compressFullPath(String parentPath, File file, ZipOutputStream out) throws Exception {
    	byte[] buffer = new byte[1024];
    	
    	if(file.isFile()) {
    		FileInputStream in = new FileInputStream(file);
    		out.putNextEntry(new ZipEntry(parentPath + file.getName()));
    		
    		int len = 0;
    		while((len = in.read(buffer)) &gt; 0) {
                out.write(buffer, 0, len);
            }
    		out.closeEntry();
    		in.close();
    	} else if(file.isDirectory()) {
            ZipEntry entry = new ZipEntry(file.getName() + sep);
            out.putNextEntry(entry);
            
            String[] listOfFile = file.list();
            if(listOfFile != null) {
                for(String filename : listOfFile) {
                    compressFullPath(entry.getName(), new File(file.getPath() + sep + filename), out);
                }
            }
    	}
    	
    }
{% endhighlight %}