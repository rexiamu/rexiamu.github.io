---
title: html导出为excel
date: 2022-07-04 19:50:31
permalink: /pages/9c61be/
categories:
  - 解决方案
tags:
  - html
  - excel
---

前端根据表格导出Excel功能的简单封装，可设置样式。

## 函数封装

```js
/**
 * 导出table至Excel并下载
 * @param {HTMLElement} table 表格dom
 * @param {String} fileName 导出文件名
 */
export function tableToExcel(table, fileName) {
  var excelContent = table.innerHTML;
  var excelFile = "<html xmlns:o='urn:schemas-microsoft-com:office:office' xmlns:x='urn:schemas-microsoft-com:office:excel' xmlns='http://www.w3.org/TR/REC-html40'>";
  excelFile += "<head><!--[if gte mso 9]><xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet><x:Name>{worksheet}</x:Name><x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions></x:ExcelWorksheet></x:ExcelWorksheets></x:ExcelWorkbook></xml><![endif]--></head>";
  excelFile += excelContent;
  excelFile += "</table>";
  excelFile += "</html>";
  var link = "data:application/vnd.ms-excel;base64," + window.btoa(unescape(encodeURIComponent(excelFile)));
  var a = document.createElement("a");
  a.download = fileName + ".xlsx";
  a.href = link;
  a.click();
}
```

## 函数使用

```js
// 导入 tableToExcel 函数
import { tableToExcel } from "@/utils/index"

...

// 使用
const table = document.querySelector('.export-table')
table.style.background = "#112549" // 修改表格样式，设置行内样式导出时才生效
tableToExcel(table, '设备历史数据')
```
