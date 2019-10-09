### 1. 压缩
#### 文件压缩
    import zipfile
    file = '/home/jacky/tmp/zip_test/test.apk'
    f = zipfile.ZipFile('test.zip', 'w', zipfile.ZIP_DEFLATED)
    f.write(file)
    f.close()

#### 重写压缩包里的目录结构   
    import zipfile
    file = '/home/jacky/tmp/zip_test/test.apk'
    f = zipfile.ZipFile('test.zip', 'w', zipfile.ZIP_DEFLATED)
    f.write(file, '/res/new_file_name.apk')
    f.close()

#### 压缩目录
    import zipfile
    import glob
    files = glob.glob('/home/jacky/tmp/zip_test/zip_dir_test/*')
    f = zipfile.ZipFile('test.zip', 'w', zipfile.ZIP_DEFLATED)
    for file in files:
        f.write(file)
    f.close()

### 2. 获取文件大小、创建时间、访问时间
```
    # -*- coding: UTF8 -*-
    import time
    import datetime
    import os

    1、　　'''把时间戳转化为时间: 1479264792 to 2016-11-16 10:53:12'''
    　　　　def TimeStampToTime(timestamp):
    　　　　　　timeStruct = time.localtime(timestamp)
    　　　　　　return time.strftime('%Y-%m-%d %H:%M:%S',timeStruct)


    2、　　'''获取文件的大小,结果保留两位小数，单位为MB'''
    　　　　def get_FileSize(filePath):
    　　　　　　filePath = unicode(filePath,'utf8')
    　　　　　　fsize = os.path.getsize(filePath)
    　　　　　　fsize = fsize/float(1024*1024)
    　　　　　　return round(fsize,2)

    3、　　'''获取文件的访问时间'''
    　　　　def get_FileAccessTime(filePath):
    　　　　　　filePath = unicode(filePath,'utf8')
    　　　　　　t = os.path.getatime(filePath)
    　　　　　　return TimeStampToTime(t)

    4、　　'''获取文件的创建时间'''
    　　　　def get_FileCreateTime(filePath):
    　　　　　　filePath = unicode(filePath,'utf8')
    　　　　　　t = os.path.getctime(filePath)
    　　　　　　return TimeStampToTime(t)

    5、　　'''获取文件的修改时间'''
    　　　　def get_FileModifyTime(filePath):
    　　　　　　filePath = unicode(filePath,'utf8')
    　　　　　　t = os.path.getmtime(filePath)
    　　　　　　return TimeStampToTime(t)
```


### 3. 逐行读取
    file=open('',''r)
    for line in file.readlines():
        print line

### 4. 编码问题
    UnicodeEncodeError: 'ascii' codec can't encode character u'\\xa0' in position 20: ordinal not in range(128)

    添加encode logFile.write(r.text.encode('utf-8'))

###  5. Python 包管理工具
    sudo apt-get install python-pip

### 6. 字典中的key或value排序
    sorted函数对字典按key排序和按value排序
#### 按key值对字典排序
    sorted(dict.keys())
#### 按value值对字典排序
    dic_words=sorted(dic_words.items(),key=lambda item:item[1])
    这里key参数对应的lambda表达式的意思则是选取元组中的第二个元素作为比较参数
    （如果写作key=lambda item:item[0]的话则是选取第一个元素作为比较对象，也就是key值作为比较对象。lambda x:y中x表示输出参数，y表示lambda函数的返回值），
    所以采用这种方法可以对字典的value进行排序。注意排序后的返回值是一个list，而原字典中的名值对被转换为了list中的元组。

### 7. 字符串判断否为字母或者数字(浮点数)
      str.isalnum() 所有字符都是数字或者字母
      str.isalpha() 所有字符都是字母
      str.isdigit() 所有字符都是数字
      str.isspace() 所有字符都是空白字符、t、n、r

### 8. 元组
    Python的元组与列表类似，不同之处在于元组的元素不能修改。
    元组使用小括号，列表使用方括号。
    元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可。

### 9. 字典更新元素
    book_dict["owner"] = "tyson" #第一种方式，指定key，并且为其赋值一个value
    book_dict.update({"country": "china"}) #第二种方式，使用update方法,传入一个字典进去
    book_dict.update(temp = "无语中", help = "帮助") #第三种方式，直接传一个以key为变量进去


### 10. 中文文本分析
    库jieba

### 11. html分析
    库xml
    xpath
    from lxml import etree
