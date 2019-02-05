---
layout: post
title:  "Java Apache Poi"
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


## poi
- Apache Poi 라이브러리 필요
- Excel 파일 입출력 기능 제공
- xlsx -> XSSF[xxx] 제공
- xls  -> HSSF[xxx] 제공
- 대용량 데이터 처리 -> SXSSF[xxx]

## 파일 읽기 예제
{% highlight javascript %}
    List list = new ArrayList();
    
    // 파일 확장자 추출
    File file = new File("excel.xlsx");
    String extension = file.getName().substring(file.getName().indexOf('.')+1);
    FileInputStream fis = new FileInputStream("excel.xlsx");
        
    if("xlsx".equals(extension.toLowerCase())) {
        	
      	// xlsx 파일 읽기
       	XSSFWorkbook xlsx = new XSSFWorkbook(fis);
       	
       	// 첫번째 시트 읽기
       	XSSFSheet xlsxSheet = xlsx.getSheetAt(0);
       	
       	// row를 하나씩 읽어 데이터를 추출한다.
       	for(int rowIndex=1; rowIndex&lt;xlsxSheet.getPhysicalNumberOfRows(); rowIndex++) {
       		XSSFRow xlsxRow = xlsxSheet.getRow(rowIndex);
       		if(null != xlsxRow) {
       			Map map = new HashMap();
                map.put("1", xlsxRow.getCell(0).getStringCellValue());
                map.put("2", xlsxRow.getCell(1).getStringCellValue());
                map.put("3", xlsxRow.getCell(2).getStringCellValue());
                list.add(map);
        		}
        	}
        	
    } else if("xls".equals(extension.toLowerCase())) {
        	
       	// xls 파일 읽기
       	HSSFWorkbook xls = new HSSFWorkbook(fis);
        	
       	// 첫번째 시트 읽기
       	HSSFSheet xlsSheet = xls.getSheetAt(0);
        	
       	// row를 하나씩 읽어 데이터를 추출한다.
       	for(int rowIndex=1; rowIndex&lt;xlsSheet.getPhysicalNumberOfRows(); rowIndex++) {
       		HSSFRow xlsRow = xlsSheet.getRow(rowIndex);
       		if(null != xlsRow) {
                Map map = new HashMap();
                map.put("1", xlsxRow.getCell(0).getStringCellValue());
                map.put("2", xlsxRow.getCell(1).getStringCellValue());
                map.put("3", xlsxRow.getCell(2).getStringCellValue());
                list.add(map);
       		}
       	}
    }
{% endhighlight %}

- 개선 사항
{% highlight javascript %}
    /**
     * 파일의 확장자를 읽어 workbook을 반환하는 메서드
     * 
     * @param filename
     * @throws IOException
     */
	private static Workbook getWorkbook(String filename) throws IOException {
		// FilenameUtils는 common-io 라이브러리에 있는 유틸.
		String extension = FilenameUtils.getExtension(filename);
		Workbook workbook = null;
		try {
			FileInputStream fis = new FileInputStream(filename);
			
			if("xlsx".equalsIgnoreCase(extension)) {
				workbook = new XSSFWorkbook(fis);
			} else if("xls".equalsIgnoreCase(extension)) {
				workbook = new HSSFWorkbook(fis);
			}
			
			fis.close();
		} catch (FileNotFoundException e) {
			if("xlsx".equalsIgnoreCase(extension)) {
				workbook = new XSSFWorkbook();
			} else if("xls".equalsIgnoreCase(extension)) {
				workbook = new HSSFWorkbook();
			}
		}
		
		return workbook;
	}
	
    /**
     * workbook의 데이터를 읽어 리스트로 반환하는 메서드
     * 
     * @param filename
     * @param isHeader 헤더 여부를 입력받아 true면 헤더를 삭제한다.(헤더는 첫번째 행을 기준으로 한다)
     * @throws FileNotFoundException
     * @throws IOException
     */
	public static List&lt;List&lt;String&gt;&gt; getDataListFromExcel(String filename, boolean isHeader) throws FileNotFoundException, IOException {
		List&lt;List&lt;String&gt;&gt; paramList = new ArrayList&lt;&gt;();
		
		// * 파일 읽기 개선사항 예제의 getWorkbook 메서드 사용
		Workbook workbook = getWorkbook(filename);
		
		// 시트 개수만큼 읽기
		for(int i=0; i&lt;workbook.getNumberOfSheets(); i++) {
			Sheet sheet = workbook.getSheetAt(i);
			
			// 헤더 여부 검사
			if(isHeader) {
				sheet.removeRow(sheet.getRow(0));
			}
			
			// row
			for(Row row : sheet) {
				List&lt;String&gt; param = new ArrayList&lt;&gt;();
				if(row == null) {
					continue;
				}
				// cell
				for(Cell cell : row) {
					if (cell == null) {
	                    continue;
	                }

					// cell style type별로 데이터를 받는다.
	                switch(cell.getCellType()) {
	                case Cell.CELL_TYPE_FORMULA:
	                	param.add(cell.getCellFormula());
	                    break;

	                case Cell.CELL_TYPE_NUMERIC:
	                	param.add(String.format("%.0f", cell.getNumericCellValue()));
	                    break;

	                case Cell.CELL_TYPE_STRING:
	                	param.add(cell.getStringCellValue());
	                    break;

	                default:
	                    break;
	                }
				}
				
				paramList.add(param);
			}
		}
		
		return paramList;
	}
{% endhighlight %}

## 데이터를 엑셀 파일로 생성하는 예제
{% highlight javascript %}
    /**
     * 데이터를 excel 파일로 입력하는 메서드
     * 
     * @param wholeList
     * @param sheetName
     * @param savePath
     * @throws IOException 
     * @throws FileNotFoundException 
     */
	public static void createExcelFromData(List&lt;List&lt;Map&lt;String, Object&gt;&gt;&gt; wholeList, String[] sheetName, String savePath, String filename) throws FileNotFoundException, IOException {
		Workbook workbook = getWorkbook(filename);
		Sheet sheet = null;
		
		if(sheetName == null) {
			sheet = workbook.createSheet();
			writeSheetFromData(sheet, wholeList.get(0));
		} else {
			int index = 0;
			for(String name : sheetName) {
				sheet = workbook.createSheet(name);
				writeSheetFromData(sheet, wholeList.get(index++));
			}
		}
		
		createExcelFile(workbook, savePath, filename);
	}
	
    /**
     * sheet에 데이터를 입력하는 메서드
     * 
     * @param sheet
     * @param paramList
     */
	private static void writeSheetFromData(Sheet sheet, List&lt;Map&lt;String, Object&gt;&gt; paramList) {
		Iterator&lt;Map&lt;String, Object&gt;&gt; itr = paramList.iterator();
		Map&lt;String, Object&gt; param = null;
		Row row = null;
		Cell cell = null;
		int rowIdx = 0;
		int cellIdx = 0;
		
		// 헤더 입력
		if(itr.hasNext()) {
			param = itr.next();
			row = sheet.createRow(rowIdx++);
			cellIdx = 0;
			for(String key : param.keySet()) {
				cell = row.createCell(cellIdx++);
				cell.setCellValue(key);
			}
		}
		
		// 데이터 입력
		itr = paramList.iterator();
		while(itr.hasNext()) {
			param = itr.next();
			row = sheet.createRow(rowIdx++);
			cellIdx = 0;
			for(String key : param.keySet()) {
				cell = row.createCell(cellIdx++);
				if(param.get(key) instanceof String) {
					cell.setCellValue(String.valueOf(param.get(key)));
				} else {
					cell.setCellValue(new BigDecimal(String.valueOf(param.get(key))).doubleValue());
				}
			}
		}
	}
	
    /**
     * workbook을 받아 excel 파일을 경로에 생성하는 메서드
     * 
     * @param workbook
     * @param filePath
     * @throws IOException
     * @throws FileNotFoundException
     */
	private static void createExcelFile(Workbook workbook, String savePath, String filename) throws IOException, FileNotFoundException {
		FileUtil.checkPath(savePath);
		FileOutputStream fos = new FileOutputStream(savePath + File.separator + filename);
		workbook.write(fos);
		logger.debug("|=====  " + savePath + File.separator + filename + " 생성!");
	}
{% endhighlight %}