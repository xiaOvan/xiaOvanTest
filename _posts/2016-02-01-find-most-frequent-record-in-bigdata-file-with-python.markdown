---
layout: post
title: 利用Python脚本从上亿条网站日志中查找系统访问前10的IP地址
date: 2016-02-01 15:32:24.000000000 +09:00
categories: algorithm
tags: bigdata algorithm python
author: xiaOvan
---

* content
{:toc}

## 问题
	处理问题：从网站后台日志从提取访问系统的IP记录，并且找出访问系统排名前10的IP,其中IP记录上亿条


  分析:面对的文本文件的IP地址记录上亿，占用的存储空间为 numOfip * 15b/1024*1024 > 1GB,所以一次性将文件记录读入内存是会造成out of memory错误.

## 解决思路:

1.随机生成待处理文件

2.按照IP地址4位中的最后1位对255区模来对文件进行hash，把大文件分成256个小文件,这样同样一个IP就只可
能出现在一个文件之中，这样最后拆分后文件中出现数量最大的IP即为我们所求的最终结果

3.对所有拆分后的文件IP出现的频率进行统计

实现代码分别如下:

### I.生成文件

```python
import random
import datetime
def generateRandom(rangeFrom,rangeTo):
        return random.randint(rangeFrom,rangeTo)
def generateIp(fileLoc,numberofLines):
        IP=[]
        file_hand = open(fileLoc,'a+')
        for i in range(numberofLines):
                IP.append(str(generateRandom(0,255))+'.'+str(generateRandom(0,255)) +'.'+ str(generateRandom(0,255))+'.'+ str(generateRandom(0,255))+'\n')
        file_hand.writelines(IP)
        file_hand.close()
if __name__=='__main__':
        starttime = datetime.datetime.now()
        for i in range(10):
                generateIp('massiveIP.txt',10000000)
        endtime = datetime.datetime.now()
        print('generated in' + (endtime-starttime).seconds + 's')
```

### II.拆分文件

```python
import datetime
def  writeFile(targetFolder,dictContent):
        for key,value in dictContent.items():
                with open(targetFolder + "/file_" + str(key) + ".txt",'a+') as out:
                        out.writelines(dictContent[key])

def  splitFile(SourceLoc,DestFolder):
        file_handler = open(SourceLoc,'r')
        split_num=999999
        tempDict = dict()
        lineCounter = 0
        Counter = 1
        with open(SourceLoc) as fp:
                for line in fp:
                        lastNum = int(line[line.rfind('.')+1:])
                        index = lastNum%256
                        if(tempDict.get(index)):
                                tempDict[index].append(line)
                                lineCounter = lineCounter + 1
                                if( lineCounter == split_num ):
                                        print('write ' + str(Counter) + ' times')
                                        lineCounter = 0
                                        Counter = Counter +1
                                        writeFile(DestFolder,tempDict)
                                        tempDict = dict()
                        else:
                                tmp = list()
                                tmp.append(line)
                                tempDict[index]=tmp
                                lineCounter = lineCounter + 1
                                if( lineCounter == split_num ):
                                        print('write ' + str(Counter) + ' times')
                                        lineCounter = 0
                                        Counter = Counter +1
                                        writeFile(DestFolder,tempDict)
                                        tempDict = dict()

if __name__=='__main__':
        time1 = datetime.datetime.now()
        splitFile('massiveIP.txt','test')
        time2 = datetime.datetime.now()
        print('finished in ' + (time2-time1).seconds + 's')
```

### III.排序

```python
import os
import datetime

def findIP(targetFolder):
        Result=dict()
        IP=dict()
        for root,dirs,files in os.walk(targetFolder):
                for f in files:
                        with open(os.path.join(targetFolder,f),'r') as fp:
                                for line in fp:
                                        if line in IP:
                                                IP[line]=IP[line]+1
                                        else:
                                                IP[line]=1
                        IP=sorted(IP.items(),key=lambda d:d[1])
                        Result[IP[-1][0]] = IP[-1][1]
                        IP={}
        print(sorted(Result.items(),key=lambda d:d[1],reverse=True)[0:10])

if __name__=='__main__':
        time1 = datetime.datetime.now()
        findIP('test')
        time2 = datetime.datetime.now()
        print('finished in ' + (time2-time1).seconds + 's')
```


### IV.得到结果

	[('168.43.69.168\n', 1684), ('168.43.219.91\n', 1682), ('168.43.42.174\n', 1679), ('168.43.221.137\n', 1678), ('168.43.235.71\n', 1674), ('168.43.140.227\n', 1670), ('168.43.53.105\n', 1669), ('168.43.210.99\n', 1669), ('168.43.206.215\n', 1667), ('168.43.111.255\n', 1667)]

### V.中间出现的错误：
	在拆分文件的时候，曾经出现python进程被系统强制killed的情况，是因为之前没有加上pivot计数对文件进行拆分，把所有数据读入内存后进行拆分
