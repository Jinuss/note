---
categories:
- C#
tags:
- C#
---
目前我只接触过两种模板导出的方式：

-  C#利用NPOI接口制作Excel模板，在服务端用数据渲染模板

-  在前端利用前人搭建好的框架，利用office编写xml制作模板，在客户端进行数据的渲染，导出的格式是word。

在制作报表时两种方式都可以满足的基本需求，但excel模板更加强大，因为xml模板的布局大体在事先就要确定好，扩展性不高，而excel模板就可以根据数据的特点进行自定义布局，拓展性更强。下面介绍如何在服务端利用NPOI构建excel模板。

简单来讲，NPOI是一个库，它可以处理多种文件格式如xls、xlsx、doc、ppt、vsd等。除了制作excel模板，也可以读取excel中的数据。本文具体介绍制作excel模板。

添加引用NPOI后，在文件的头部引入如下几个接口
>using NPOI.HSSF.UserModel;

>using NPOI.HSSF;

>using NPOI.SS.UserModel

>using NPOI.SS.Util

HSSF使用于2007之前的xls版本，XSSF适用于2007及其之后的xlsx版本，它们是excel/doc格式读写库。NPOI.SS是Excel公用接口及Excel公式计算引擎。更多具体的功能以及接口可以自行百度。

具体实现(code)如下：
       
         public List<string> GetExcel(string year, string month, string type, out string statusCode, out string errMsg){
            statusCode = "0000";
            errMsg = "";
            List<string> response = new List<string>();
            string strFilePath = "";
            string strGuid = Guid.NewGuid().ToString();
            string FilePath = "\\BufFile\\OutFiles\\DownLoadFiles\\ExportExcel\\" + strGuid;
                //构建文件缓存目录
            strFilePath = System.IO.Directory.GetParent(System.IO.Directory.GetParent(System.AppDomain.CurrentDomain.BaseDirectory).FullName).FullName + FilePath;
            if (!System.IO.Directory.Exists(strFilePath))
            {
                System.IO.Directory.CreateDirectory(strFilePath);
            }
            //文件命名
            string strFileName = strFilePath + "\\" + "XXXXXX.xls";
            string ret = FilePath + "\\" + "XXXXXX.xls";
            string uploadPath = System.IO.Directory.GetParent(System.IO.Directory.GetParent(System.AppDomain.CurrentDomain.BaseDirectory).FullName).FullName + "\\BufFile\\OutFiles\\UploadFiles\\";
            List<CompletionRateModel> data = GetCompletionRate(year, month, out statusCode, out errMsg);
            try
            {   //创建工作薄
                HSSFWorkbook hssfWorkBook = new HSSFWorkbook();
                    //编辑文件信息，如文件所属公司、作者、创建日期等
                DocumentSummaryInformation dsi = PropertySetFactory.CreateDocumentSummaryInformation();
                dsi.Company = "ZondyCyber";
                SummaryInformation si = PropertySetFactory.CreateSummaryInformation();
                si.Author = "ZondyCyber";
                si.LastAuthor = "ZondyCyber";
                si.CreateDateTime = DateTime.Now;
                hssfWorkBook.DocumentSummaryInformation = dsi;
                hssfWorkBook.SummaryInformation = si;
                    //创建名为YYYYYY的表sheet
                HSSFSheet hssfSheet = hssfWorkBook.CreateSheet("YYYYYY") as HSSFSheet;

                //设置列宽
                for (int c = 0; c < 7; c++)
                {
                    hssfSheet.SetColumnWidth(c, 12 * 266);
                }

                //设置列头的单元格样式
                HSSFCellStyle cellStyle = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                HSSFFont cellFont = hssfWorkBook.CreateFont() as HSSFFont;
                cellFont.Boldweight = Convert.ToInt16(FontBoldWeight.Bold);
                cellFont.FontName = "宋体";
                cellFont.FontHeightInPoints = 12;
                cellStyle.Alignment = HorizontalAlignment.Center;
                cellStyle.VerticalAlignment = VerticalAlignment.Center;
                cellStyle.WrapText = true;
                cellStyle.SetFont(cellFont);
                cellStyle.BorderTop = cellStyle.BorderRight = cellStyle.BorderBottom = cellStyle.BorderLeft = BorderStyle.Thin;//BorderStyle.None
                //左对齐样式
                HSSFCellStyle leftCellStyle = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                leftCellStyle.CloneStyleFrom(cellStyle);
                leftCellStyle.Alignment = HorizontalAlignment.Left;
                //居中填充样式
                HSSFCellStyle fillCellStyle = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                fillCellStyle.CloneStyleFrom(cellStyle);
                fillCellStyle.FillForegroundColor = NPOI.HSSF.Util.HSSFColor.Grey25Percent.Index;
                //fillCellStyle.FillPattern = FillPattern.Diamonds;
                //fillCellStyle.FillPattern = FillPattern.FineDots;
                fillCellStyle.FillPattern = FillPattern.LeastDots;
                fillCellStyle.FillBackgroundColor = NPOI.HSSF.Util.HSSFColor.Grey25Percent.Index;

                //居中填充样式(无边框)
                HSSFCellStyle fillCellStyle2 = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                fillCellStyle2.CloneStyleFrom(fillCellStyle);
                //fillCellStyle2.FillPattern = FillPattern.NoFill;
                fillCellStyle2.BorderTop = fillCellStyle2.BorderRight = fillCellStyle2.BorderBottom = fillCellStyle2.BorderLeft = BorderStyle.None;//BorderStyle.None
                //值的样式
                HSSFCellStyle valueStyle = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                HSSFFont valueFont = hssfWorkBook.CreateFont() as HSSFFont;
                valueFont.FontName = "宋体";
                valueFont.FontHeightInPoints = 10;
                valueStyle.BorderTop = valueStyle.BorderRight = valueStyle.BorderBottom = valueStyle.BorderLeft = BorderStyle.Thin;//BorderStyle.None
                valueStyle.WrapText = true;
                valueStyle.Alignment = HorizontalAlignment.Center;
                valueStyle.VerticalAlignment = VerticalAlignment.Center;
                valueStyle.SetFont(valueFont);
                //值的样式（无边框）
                HSSFCellStyle valueStyle2 = hssfWorkBook.CreateCellStyle() as HSSFCellStyle;
                valueStyle2.CloneStyleFrom(valueStyle);
                valueStyle2.BorderTop = valueStyle2.BorderRight = valueStyle2.BorderBottom = valueStyle2.BorderLeft = BorderStyle.None;//BorderStyle.None

                //开始构建表格
                int rowIndex = 0;//记录用到第几行
                //构建表题
                String Title = "XXXXXX(" + type + ")统计     " + year + "年" + month + "月";
                //创建表格的第一行的第一个单元格
                    hssfSheet.CreateRow(0).CreateCell(0).CellStyle = fillCellStyle;
                    //获取表格的第一行的第一个单元格，并为其赋值
                hssfSheet.GetRow(0).GetCell(0).SetCellValue(Title);
                //合并单元格
                    /*
                    * cellRangeAddress可以合并行或列，第一个参数是起始行号，第二个参数是终止行号，第三个参数是起始列号，第三个参数是终止列号
                    */
                CellRangeAddress region = new CellRangeAddress(rowIndex, rowIndex + 2, 0, 6);
                hssfSheet.AddMergedRegion(region);
                hssfSheet.SetEnclosedBorderOfRegion(region, BorderStyle.Thin, NPOI.HSSF.Util.HSSFColor.Black.Index);
                rowIndex = rowIndex + 3; //3
                String Type = "部门";
                //构建表头
                hssfSheet.CreateRow(rowIndex).CreateCell(0).CellStyle = valueStyle;
                hssfSheet.GetRow(rowIndex).GetCell(0).SetCellValue("序号");
                /**
                    ** 构建表格的具体细节省略，无非是合并单元格，填充数据
                    **/
                //表格构建完毕
                FileStream file = new FileStream(strFileName, FileMode.Create);
                hssfWorkBook.Write(file);
                file.Close();
                response.Add(ret);
            }
