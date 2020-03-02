js实现操作excel类exceljs(推荐) 表格 其它源码  HTML我帮您  1年前  29640次浏览
exceljs实现java风格用javascript读写操作excel文件，实现了读取sheet工作表，创建列、插入行数据、行格式化输出、行验证表达、设置字体样式、设置单元格边框大小。
类似Excel的jquery/VUE电子表格插件jExcel(推荐)
jquery可视化设计透视表库PivotTable.js网页片在线Excel表格组件x-spreadsheet(推荐)前端操作Excel的js框架sheetjs
==============
https://github.com/exceljs/exceljs
js实现操作excel类exceljs(推荐)
var Excel = require("exceljs");
var workbook = new Excel.Workbook();
workbook.creator = "Me";
workbook.lastModifiedBy = "Her";
workbook.created = new Date(1985, 8, 30);
workbook.modified = new Date();
// Iterate over all sheets
// Note: workbook.worksheets.forEach will still work but this is better
workbook.eachSheet(function(worksheet, sheetId) {
    // ...
});
 
// fetch sheet by name
var worksheet = workbook.getWorksheet("My Sheet");
// fetch sheet by id
var worksheet = workbook.getWorksheet(1);
// Add column headers and define column keys and widths
// Note: these column structures are a workbook-building convenience only,
// apart from the column width, they will not be fully persisted.
worksheet.columns = [
    { header: "Id", key: "id", width: 10 },
    { header: "Name", key: "name", width: 32 },
    { header: "D.O.B.", key: "DOB", width: 10 }
];
// Add a couple of Rows by key-value, after the last current row, using the column keys
worksheet.addRow({id: 1, name: "John Doe", dob: new Date(1970,1,1)});
worksheet.addRow({id: 2, name: "Jane Doe", dob: new Date(1965,1,7)});
// merge a range of cells
worksheet.mergeCells("A4:B5");
// merge by top-left, bottom-right
worksheet.mergeCells("G10", "H11");
worksheet.mergeCells(10,11,12,13); // top,left,bottom,right
// display value as "1 3/5"
ws.getCell("A1").value = 1.6;
ws.getCell("A1").numFmt = "# ?/?";
// for the wannabe graphic designers out there
ws.getCell("A1").font = {
    name: "Comic Sans MS",
    family: 4,
    size: 16,
    underline: true,
    bold: true
};