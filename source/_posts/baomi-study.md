---
title: 保密观保密学习
date: 2022-05-11 12:21:15
tags:
---


{% note danger modern %}
本文仅供学习使用,看本文前默认有抓取请求的能力,若无请直接关闭
{% endnote %}

## 获取学时
上传时长的接口调用2次:
    1.视频/音频开始播放的时候
    2.视频/音频(页面)关闭的时候
都会调用接口`/portal/api/v2/studyTime/saveCoursePackage.do`,上传播放数据:
```json
    {
        ......
        "resourceLength":802  // 播放文件的原始长度
        "studyLength":0  // 播放进度?
        "studyTime":0    // 播放进度?
        ......
    }
```
{% note success modern%}
解决方案:
最简单粗暴的方式就是直接拦截这个请求,然后把`studyLength`和`studyTime`直接改成`resourceLength`的值
{% endnote %}

## 考试

{% note warning modern %}
目前电脑端考试提交好像有问题,考试了两次都返回不了成绩,所以不建议在电脑上进行考试
{% endnote %}

手机端考试调用`/api/exam/application/getExamContentData.do`接口
响应结果:
```json
{
    "data":{
        ......
        "typeList":[] //这个数组里面就是两组题目的答案(选择/判断)
        ......
    }
}
```
{% note success modern%}
解决方案:
自己把`typeList`里面的数据复制出来查找考试答案即可
{% endnote %}


附:
```js
//快捷代码
data.data.typeList[0].questionList.map(item=>(item.answer))
data.data.typeList[1].questionList.map(item=>(item.answer))
```
![测试图片](https://raw.githubusercontent.com/konglinghu777/images/main/20220511124900.png)

![考试结果](https://raw.githubusercontent.com/konglinghu777/images/main/IMG_2800.PNG)


