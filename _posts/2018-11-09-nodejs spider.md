---
title: 'node.js简单小爬虫实践'
layout: post
tags:
    - js
category: Javascript
---

<!--more-->

最近花了点时间了解了一下node.js的爬虫程序，发现还是挺简单的，遂以此记录我的学习历程。  

首先是node环境搭建和项目的初始化，这部分网上有很多教程，就不赘述了。本文选择抓取慕课网node教程的章节信息，话不多说，直接贴代码：  
```
const http = require('http');
const fs = require('fs');
const cheerio = require('cheerio');
const url = "http://www.imooc.com/learn/348";

http.get(url, res => {
    let html = "";
    /* 获取页面数据 */
    res.on('data', data => {
        html += data
    })
    /* 数据获取结束 */
    res.on('end', () => {
        let courseData = filterChapters(html);
        printData(courseData);
        // fs.writeFile('./data/immoc.txt', printData(courseData), err => {
        //     if (err) throw new Error('写文件失败！' + err);
        //     console.log("写入文件成功！");
        // })
    })
}).on('error', () => {
    console.log("数据获取失败！");
})

/* 过滤章节信息来获取需要的课程信息 */
function filterChapters(html) {
    let $ = cheerio.load(html);    // cheerio -> 相当于nodejs版的jQuery,可用其获取要获取的页面的dom节点信息
    let chapters = $(".chapter");
    let courseData = [];
    chapters.each(function (item) {
        let chapter = $(this);
        let chapterTitle = chapter.find('h3').text().trim().split('\n')[0];
        let videos = chapter.find('.video').children('li');
        let chapterData = {
            chapterTitle: chapterTitle,
            videos: []
        };
        videos.each(function (item) {
            let video = $(this).find('.J-media-item');
            let videoTitle = video.text().trim().split('\n')[0].trim();
            let videoTime = video.text().trim().split('\n')[1].trim();
            let id = String(video.attr('href')).split('/video/')[1];
            let address = "https://www.imooc.com" + String(video.attr('href'));
            chapterData.videos.push({
                title: videoTitle,
                time: videoTime,
                id: id,
                address: address
            })
        });

        courseData.push(chapterData)
    });
    return courseData;
}

/* 打印抓取的信息 */
function printData(courseData) {
    courseData.forEach(item => {
        let chapterTitle = item.chapterTitle;
        console.log(chapterTitle + '\n');
        item.videos.forEach(video => {
            console.log(' [' + video.id + '] ' + video.title + ' ' + video.time + '\t' + video.address + '\n');
        }
        )
    })
}

/* 将抓取到的信息作为字符串保存在文件中 */
// function printData(courseData) {
//     let fileStr = "";
//     courseData.forEach(item => {
//         let chapterTitle = item.chapterTitle;
//         fileStr += chapterTitle + '\n';
//         item.videos.forEach(video => {
//             fileStr += ' [' + video.id + '] ' + video.title + ' ' + video.time + '\t' + video.address + '\n';
//         }
//         )
//     })
//     return fileStr;
// }
```  
<br><br>
运行后结果如下：  
```
第1章 前言

 [6687] 1-1 Node.js基础-前言 (01:20)    https://www.imooc.com/video/6687

 [6688] 1-2 为什么学习Nodejs (05:43)    https://www.imooc.com/video/6688

第2章 安装 Nodejs

 [6689] 2-1 Node.js基础-课程简介 (01:19)        https://www.imooc.com/video/6689

 [6690] 2-2 Nodejs版本常识 (01:02)      https://www.imooc.com/video/6690

 [6691] 2-3 Windows下安装Nodejs (04:43) https://www.imooc.com/video/6691

 [6692] 2-4 Linux下安装Nodejs (06:24)   https://www.imooc.com/video/6692

 [6693] 2-5 Mac下安装Nodejs (03:55)     https://www.imooc.com/video/6693

第3章 等不及了来尝鲜

 [6694] 3-1 Node.js基础-起一个web服务器 (05:14) https://www.imooc.com/video/6694

 [6695] 3-2 Node.js基础-命令行中体验 (02:47)    https://www.imooc.com/video/6695

第4章 模块与包管理工具

 [6697] 4-1 Node.js 的模块 与 Commonjs 规范 (03:44)     https://www.imooc.com/vi
deo/6697

 [6700] 4-2 模块的分类 (00:45)  https://www.imooc.com/video/6700

 [6701] 4-3 简单的Nodejs模块 (09:23)    https://www.imooc.com/video/6701

第5章 横扫 Nodejs API

 [6705] 5-1 Node.js-不要陷入版本选择的深渊 (02:32)      https://www.imooc.com/vi
deo/6705

 [6710] 5-2 URL网址解析的好帮手 (10:30) https://www.imooc.com/video/6710

 [6711] 5-3 QueryString参数处理小利器 (06:40)   https://www.imooc.com/video/6711

 [6712] 5-4 HTTP知识先填坑 (09:43)      https://www.imooc.com/video/6712

 [6713] 5-5 HTTP知识填坑之“以慕课网为例分析” (10:13)  https://www.imooc.com/vi
deo/6713

 [7557] 5-6 HTTP 事件回调进阶 (17:51)   https://www.imooc.com/video/7557

 [7558] 5-7 HTTP 源码解读之先了解作用域、上下文 (20:50) https://www.imooc.com/vi
deo/7558

 [7963] 5-8 HTTP 源码解读 (22:08)       https://www.imooc.com/video/7963

 [7964] 5-9 HTTP 性能测试 (09:15)       https://www.imooc.com/video/7964

 [7965] 5-10 HTTP 小爬虫 (17:33)        https://www.imooc.com/video/7965

 [8525] 5-11 事件模块小插曲 (15:15)     https://www.imooc.com/video/8525

 [8837] 5-12 Node.js：request方法 (17:56)       https://www.imooc.com/video/8837
```
若想写入文件中，可看上述代码的注释部分。  

至此，一个简单的爬虫就完成了。当然，node的学习还是路漫漫兮修远兮~  

PS：双11要来了，最近又温度骤降，别人都在剁手，我只能在办公室里跺跺脚了。。XD