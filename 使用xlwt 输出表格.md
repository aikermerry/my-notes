##  使用xlwt 输出表格

xlrd只支持对Excel文件格式为xls文件的读取  

xlwt只支持对Excel文件格式为xls文件的写入 

```
file = xlwt.Workbook(encoding='utf-8')
```

```
sheet1 = wb.add_sheet(u'sheet1')
```

```
row0 = [u'IDTime',u'Authe',u'Title',u'myContent',u'timeDate',u'zhuanfa',u'huifu',u'dianzan',u'KeyClass','Url','imgName','Addr']
```

```
row = cursor.fetchall()
for i in range(0, len(row0)):
  sheet1.write(0,i,row0[i])
  for j in range(0,len(row[i])):
     sheet1.write(i+1,j,row[i][j])
```

```
file.save('result.xls')
```