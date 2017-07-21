# 作业系统API文档v0.1

[TOC]



### 主页

#### 个人画像

**描述：**

- 获取用户各学科能力值
- 获取班级排名
- 获取年纪排名

**请求URL：**

- /api/advanced/ability/own

**请求方式:**

- GET

**参数：**

无

**返回结果：**

```json
{
  "code": 0,
  "data": {
    "course": "chi", "value": 0.6,
    "course": "math", "value": 0.6,
    "course": "eng", "value": 0.6,
    "course": "poli", "value":0.6,
    "course": "his", "value": 0.6,
    "course": "bio", "value": 0.6,
    "course": "geo", "value": 0.6,
    "course": "phys", "value": 0.6,
    "course": "chem", "value":0.6
  }
  "score": 540,
  "totalPoints": 1050,
  "classRank": 30, 
  "classSize": 50,
  "gradeRank": 200,
  "gradeSize": 1000,
  
}
```

| 参数          |        描述        |
| ----------- | :--------------: |
| data        |     学科能力值列表      |
| data.course |       学科名称       |
| data.value  | 学科能力标准值（**百分比**） |
| score       |       总得分        |
| totalPoints |      总分（满分）      |
| classRank   |       班级排名       |
| classSize   |       班级人数       |
| gradeRank   |       年级排名       |
| gredeSize   |       年级人数       |
​	

### 作业考试

#### 获取作业考试列表

**描述：**

- 获取作业考试列表

**请求URL：**

- /api/practice

**请求方式:**

- GET

**参数：**

| 参数     | 是否必须 |                    说明                    |
| ------ | :--: | :--------------------------------------: |
| page   |  否   |              分页页码(**默认为1**)              |
| size   |  否   |             分页条数(**默认为10**)              |
| status |  否   | 作业考试列表类型（tbd: 待完成、completed: 已完成、report: 学情报告，***默认待完成***） |
| course |  否   | ***已完成，学情报告限定***， 默认为***语文*** （详情见表***学科说明***） |
| type   |  否   |  ***已完成限定***，默认为全部（task: 作业, exam: 试卷）   |
| date   |  否   |       **已完成限定**，默认时间为当前月份(YYYY-MM)       |

**返回结果：**

```json
#待完成列表
{
  "code": 0,
  "data":[
    {
      "id": "1",
      "type": "task",
      "course": "his",
      "auth": "王老师",
      "title": "欧洲中世纪史第一课时课后检测",
      "deadline": "2017-07-18",
      "status": "tbd",
    }
  ],
  "page": 1,
  "size": 10,
}
```

```json
#已完成列表
{
  "code": 0
  "data":[
  	{
      "date": "2017-7-18",
      "collection": [
  		{	
    	  "id": "1",
          "type": "task",
          "course": "his",
          "title": "欧洲中世纪史第一课时课后检测",
          "deadline": "2017-07-18",
  		  "submitTime": "2017-07-18",
  		  "correctTime": "2017-07-19",
          "status": "corrected"
		}
      ]
	}
  ]
}
```

```json
#学情报告(只有试卷有学情报告)
{
  "code": 0,
  "data": [
    {
      "id": "1",
      "title": "欧洲中世纪史第一单元练习卷",
      "date": "2017-7-18",
    }
  ],
  "page": 1,
  "size": 10,
}
```

| 参数          |                    说明                    |
| ----------- | :--------------------------------------: |
| id          | 练习纪录／练习标识(status为tbd，为练习标识.status为completed以及report时，为练习纪录标识) |
| type        |          练习类型(task:作业, exam:试卷)          |
| course      |            科目(详情见表***学科说明***)            |
| title       |                   练习题目                   |
| deadline    |             截止时间(YYYY-MM-DD)             |
| submitTime  |       提交时间(YYYY-MM-DD)，***已完成限定***       |
| correctTime |     批改时间(YYYY-MM-DD),   ***已完成限定***      |
| status      | 练习状态(tbd:'待完成',submitted:'已提交', corrected:'已批改') |
| page        |                   当前页码                   |
| size        |                   分页条数                   |



#### 练习详情

**描述：**

- 获取指定练习内容

**请求URL：**

- /api/practice/:id 

**请求方式:**

- GET

 **参数：**

| 参数   | 是否必须 |  说明  |
| ---- | :--: | :--: |
| id   |  是   | 练习id |
**返回结果：**

```json
#task 作业
{
  "code": 0,
  "type": "task",
  "title": "欧洲中世纪史第一课时课后检测",
  "course": "his"
  "data":[
    {
      "id": "1",
  	  "No": "1"	
      "type": "choice",
      "question": "第一次十字军东征的时间是？",
      "selection": [
  		{"key":"A", "value":"1096-1099"},
		{"key":"B": "value":"1147-1148"},
		{"key":"C", "value": "1189-1193"},
        {"key":"D": "value": "1201-1204"}
      ],
    },
    {
      "id": "2",
      "NO": "2",
      "type": "objective"
      "question":"简述宗教改革给欧洲带来了那些影响？",
    }
  ] 
}
```

```json
#exam 试卷
{
  "code": 0,
  "id": "10086",
  "type": "exam",
  "title": "欧洲中世纪史第一单元单元卷",
  "course": "his",
  "totalPoints": 100,
  "duration": 3600
  "data": [
    {
      "type": "choice",
      "collection":[
        {
          "id": "1",
  		  "NO": "1",
          "question": "第一次十字军东征的时间是？",
          "selection": [
            {"key":"A", "value":"1096-1099"},
            {"key":"B": "value":"1147-1148"},
            {"key":"C", "value": "1189-1193"},
            {"key":"D": "value": "1201-1204"}
          ],
        }
      ],
      "points": 2,
      "number": 10
    },
    {
      "type": "blank"
      "collection": [
        {
          "id": "2",
      	  "NO": "2",
      	  "question": "____将亨利四世开除了教籍?"
        }
      ],
	  "points": 1,
	  "number": 20,	
    }
  ]
}
```

| 参数              |              描述               |
| --------------- | :---------------------------: |
| id              |            练习唯一标识             |
| type            |   练习类型(task: 作业, exam: 试卷)    |
| title           |             练习标题              |
| course          |              科目               |
| totalPoints     |        总分（***试卷限定***）         |
| data            |             练习内容              |
| data.type       | 题目类型(choice:单选题,  blank: 填空题) |
| collection      |            题型内容列表             |
| collection.id   |            习题唯一标识             |
| collection.NO   |             习题序号              |
| question        |              题干               |
| selection       |        选项(***单选题限定***)        |
| selection.key   |             选项名称              |
| selection.value |             选项内容              |
| points          |              分值               |
| number          |            题目/空格数量            |
| duration        |      计时，***试卷限定***(单位:秒)      |



#### 提交练习

**描述：**

- 提交练习

**请求URL：**

- /api/practice/:id 

**请求方式:**

- POST

**参数：**

```json
[
  {
    "id": "1",
    "answer": "A"
  },
  {
    "id": "2",
    "answer": [
      "格列高利七世"
    ]
  }
]
```



| 参数              | 是否必须 |           说明           |
| --------------- | :--: | :--------------------: |
| id (path param) |  是   |          练习id          |
| id              |  是   |          题目id          |
| answer          |  是   | 答案（填空拥有多个答案，将答案放在list） |

**返回结果：**

```json
{"code": 0}
```



#### 已批改内容详情

**描述：**

- 获取已批改作业考试内容

**请求URL：**

- /api/practice/log/:id

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |   说明   |
| ---- | :--: | :----: |
| id   |  是   | 练习纪录id |
**返回结果:**

*成功:*

```json
#task 作业
{
  "code": 0,
  "type": "task",
  "title": "欧洲中世纪史第一课时课后检测",
  "course": "his"
  "data": [
    {
      "id": "1",
  	  "NO": "1",
      "type": "choice",
      "question": "第一次十字军东征的时间是？",
  	  "KNId": "1",
  	  "KN": "十字军",
  	  "examine": "分析",
  	  "CR": "0.8",
  	  "CA": "A",
      "answer": "A",
  	  "IC": true,
      "selection": [
  		{"key":"A", "value":"1096-1099"},
		{"key":"B": "value":"1147-1148"},
		{"key":"C", "value": "1189-1193"},
        {"key":"D": "value": "1201-1204"}
      ],
    },
    {
      "id": "2",
      "NO": "2",
      "type": "objective",
      "KNID": "2",
      "KN": "宗教改革",
      "examine": "分析",
  	  "CR": 0.8,
      "CA": "XXX",
      "answer": "XXX",
      "score": "5.5",
      "question": "简述宗教改革给欧洲带来了那些影响?",
    }
  ] 
}
```

```json
#exam 试卷
{
  "code": 0,
  "id": "10086",
  "type": "exam",
  "title": "欧洲中世纪史第一单元单元卷",
  "course": "his",
  "totalPoints": 100,
  "score": 85
  "data": [
    {
      "type": "choice",
      "collection": [
        {
          "id": "1",
          "NO": "1",
          "question": "第一次十字军东征的时间是？",
          "KNId": "1",
          "KN": "十字军",
          "examine": "分析",
          "CR": 0.8,
          "CA": "A",
          "answer": "A",
  		  "IC": true,
          "selection": [
            {"key":"A", "value":"1096-1099"},
            {"key":"B": "value":"1147-1148"},
            {"key":"C", "value": "1189-1193"},
            {"key":"D": "value": "1201-1204"}
          ],
        }
      ],
      "points": 2,
      "number": 10
    },
    {
      "type": "blank"
      "collection": [
        {
          "id": "2",
      	  "NO": "2",
      	  "question": "____将亨利四世开除了教籍?"
      	  "KNId": "2",
          "KN": "宗教改革",
          "examine": "分析",
          "CR": 0.8,
          "CA": "格列高利七世",
          "answer": "格列高利七世",	
      	  "IC": true,
    	}
      ],
	  "points": 1,
	  "number": 20,	
    }
  ]
}
```

失败:

```json
{"code": 40001, "errMessage":"您还没有完成该练习"}
```

| 参数                              |                    描述                    |
| ------------------------------- | :--------------------------------------: |
| id                              |                  练习纪录标识                  |
| type                            |         练习类型(task: 作业, exam: 试卷)         |
| title                           |                   练习标题                   |
| course                          |                    学科                    |
| totalPoints                     |             总分值（***试卷限定***）              |
| score                           |             总得分(***试卷限定***)              |
| data                            |                   习题内容                   |
| data.type                       | 题目类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| collection/data.id              |                  习题唯一标识                  |
| collection/data.NO              |                   习题序号                   |
| collection/data.question        |                    题干                    |
| collection/data.selection       |             选项(***单选题限定***)              |
| collection/data.selection.key   |                   选项名称                   |
| collection/data.selection.value |                   选项内容                   |
| collection/data.KN              |                   知识点                    |
| collection/data.KNId            |                 知识点唯一标识                  |
| collection/data.CR              |                   正确率                    |
| collection/data.examine         |         考查能力(记忆、理解、运用、分析、评价、创新)          |
| collection/data.CA              |                   正确答案                   |
| collection/data.score           |               **主观题习题限定**                |
| collection/data.IC              |   是否正确（true:正确， false）, ***非主观题目限定***    |
| collection/data.answer          |                   我的答案                   |



#### 学情报告详情

**描述：**

- 获取指定试卷学情报告

**请求URL：**

- /api/practice/:id/report

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |    说明     |
| ---- | :--: | :-------: |
| id   |  是   | 已完成试卷纪录id |

**返回结果:**

*成功*：

```json
{
  "code": 0,
  "score": 85,
  "classRank": 12,
  "classSize": 50,
  "gradeRank": 18,
  "gradeSize": 600,
  "SN": 10,
  "STP": 45,
  "STS": 40,
  "ON" : 10,
  "OC": 15,
  "OS": 30,
  "competence": {
      {"name": "理解能力", "points":{"class":10, "own":10}},
  }
  "data": [
   {
     "KNId": "1",
     "KN": "立体几何",
     "process": 0.03
     "map":[
       {"id": "1", "NO": "1", "points": 20, "score": 16, "avg": 14.25}
     ]
   } 
  ]
}
```

*失败:*

```json
{
  "code": 40002,
  "errMessage": "找不到该学情报告"
}
```

| 参数                      |      描述      |
| ----------------------- | :----------: |
| score                   |     总得分      |
| classRank               |     班级排名     |
| gradeRank               |     年纪排名     |
| SN                      |    主观题数量     |
| STP                     |    主观题总分     |
| STS                     |    主观题得分     |
| ON                      |    客观题数量     |
| OC                      |   客观题正确数量    |
| OS                      |    客观题得分     |
| competence              |     素养列表     |
| competence.name         |     素养名称     |
| competence.points       |     素养值      |
| competence.points.class |    班级素养值     |
| competence.points.own   |    个人素养值     |
| KNId                    |    知识点id     |
| KN                      |    知识点名称     |
| process                 |     达成度      |
| map                     | 同一知识点下面试题的集合 |
| map.id                  |     试题id     |
| map.NO                  |  试题在本卷中的题号   |
| map.points              |    试题的分值     |
| map.score               |   该试题我的得分    |
| map.avg                 |   该试题班级得分    |



#### 习题解析

**描述:**

- 获取指定收藏试题信息

**请求URL：**

- /api/practise/log/:id/analysis

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |   说明   |
| ---- | :--: | :----: |
| id   |  是   |  题目id  |
| pid  |  是   | 练习纪录id |

**返回结果:**

*成功*

```json
{
  "code": 0,
  "type": "choice",
  "id": "1",
  "NO": "1",
  "title": "1．下列各组词语中，没有错别字的一组是（）",
  "selection": [
  		{"key":"A", "value":"1096-1099"},
		{"key":"B": "value":"1147-1148"},
		{"key":"C", "value": "1189-1193"},
        {"key":"D": "value": "1201-1204"}
      ],
  "from": {
    "date": "2017-05-30",
    "type": "task",
    "title": "语文作业",
    "id": "1",
  },
  "answer": "A",
  "CA": "A",
  "IC": true,
  "CR": 0.6,
  "analysis": 'XXXXX',
  "KN": [
    "XXXX"
  ],
  "remark": [
    {
      "type": "media",
      "content": "XXX",
  	}
  ]
  "reason": [
    {"type": "offical", "content":['1']},
    {"type": "media", "content": 'XXX'}
  ],
  "officalReason": [
    {
      "id": "1",
      "content": "看错"
    } 
  ]
}
```

```json
#主观题
{
  "code": 0,
  "type": "subjective",
  "id": "1",
  "NO": "5",
  "title": "斯是陋室，唯吾德馨",
  "answer": "XXXX",
  "score": 5,
  "CA": "这是简陋的房子，只是我的品德好",
  "analysis": "XXXXX",
  "IC": true,
  "KN": ["XX"],
  "from": {
    "date": "2017-05-30",
    "type": "task",
    "title": "语文作业",
    "id": "1",
  },
  "remark": [
    {
      "type": "media",
      "content": "XXX",
  	}
  ]
  "reason": [
  	{"type": "offical", "content":['1']}
    {"type": "media", "content": 'XXX'}
  ],
  "officalReason": [
    {
      "id": "1",
      "content": "看错"
    }
  ]
}
```



*失败*

```json
{code: 40010, errMessage: '找不到该试题！'}
```

| 参数                    |                    描述                    |
| --------------------- | :--------------------------------------: |
| type                  |      试题类型(choice:单选题,  blank: 填空题)       |
| id                    |                   习题标识                   |
| NO                    |                   习题序号                   |
| title                 |                   试题名称                   |
| selection             |            选择题选项(***选择题限定***)            |
| from                  |                   试题来源                   |
| from.date             |                   练习时间                   |
| from.type             |      练习类型(task:作业,exam:试卷,raid:闯关)       |
| from.title            |                   练习题目                   |
| from.id               |                  练习纪录id                  |
| answer                |                   我的答案                   |
| CA                    |                   正确答案                   |
| IC                    |          是否正确（true:正确，false:错误）          |
| CR                    |                   正确率                    |
| analysis              |                   答案分析                   |
| KN                    |                知识点名称（集合）                 |
| remark                |                  备注/反思                   |
| remark.type           |                   备注类型                   |
| remark.content        |      备注内容, ***type为media时，内容为url***      |
| reason                |          错误原因(***IC为false限定***)          |
| reason.type           | 输出类型(media:多媒体（语音）, text: 文本,  offical:系统给出的原因 |
| reason.content        |      错误原因, ***type为media时，内容为url***      |
| officalReason         |               系统给出的错误原因集合                |
| officalReason.id      |                 系统错误原因标识                 |
| officalReason.content |                 系统错误原因内容                 |



#### 新增语音备注/反思 

**描述：**

- 给指定习题添加语音备注(正确题目)
- 给指定习题添加语音反思(错误题目)
- 只能给已经批改过的练习中的习题添加
- 更新数据时，将更新后的数据全部上传

**请求URL：**

- /api/practise/id/remark/

**请求方式:**

- POST

**参数：**

| 参数      | 是否必须 |   说明   |
| ------- | :--: | :----: |
| id      |  是   | 习题纪录id |
| mediaId |  是   | 多媒体id  |

**返回结果:**

*成功*

```json
{
  "code":0
}
```

*失败*

```json
{
  "code":4020,
  "errMessage": "语音信息不存在!"
}
```



#### 新增错因分析

**描述：**

- 上传题目错因
- 错因更新，将所有数据重新上传

**请求URL：**

- /api/practise/:id/cause/

**请求方式:**

- POST

**参数：**

```json
{
  cause: [
    {type: "offical", data : ['id','id']},
    {type: "media",  data: ['mediaId','mediaId']}
  ]
}
```

| 参数           | 是否必须 |                说明                 |
| ------------ | :--: | :-------------------------------: |
| id           |  是   |              练习纪录id               |
| type         |  是   | 错因类型(offical:题目自带错因， media:多媒体数据) |
| data         |  是   |               撮影内容                |
| data.id      |  是   |             题目自带错因id              |
| data.mediaId |  是   |              多媒体数据id              |

**返回结果:**

```json
{"code":0}
```



### 目标进阶

#### 学科能力值信息

**描述：**

- 获取指定学科能力南丁格尔玫瑰图数据

**请求URL：**

- /api/advanced/ability

**请求方式:**

- GET

**参数：**

| 参数     | 是否必须 |               说明               |
| ------ | :--: | :----------------------------: |
| course |  是   |         详情见表***学科说明***         |
| term   |  否   | 默认为当前学期(高一上到高三下一次为1、2、3、4、5、6) |
**返回结果:**

*成功*

```json
{
  "code": 0,
  "term": 2,
  "course": "math",
  "ability": 121,
  "classRank": 20,
  "gradeRank": 20,
  "data": [
    {"value": 40, "name": "模块1", "id": "1"}
  ]
}
```

*失败*

```json
{
  "code": 40003,
  "errMessage": "暂无该学期数据！"
}
```

| 参数         |            描述             |
| ---------- | :-----------------------: |
| term       | 学期(高一上到高三下一次为1、2、3、4、5、6) |
| course     |    学科(详情见表***学科说明***)     |
| ability    |            能力值            |
| classRank  |           班级排名            |
| gradeRank  |           学校排名            |
| data       |         南丁格尔玫瑰图数据         |
| data.value |           模块分值            |
| data.name  |           模块名称            |
| data.id    |          模块唯一标识           |



#### 学科能力值知识点构成

**描述：**

- 获取指定模块知识点构成的环形图数据

**请求URL：**

- /api/advanced/module/:id/ability

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |  说明  |
| ---- | :--: | :--: |
| id   |  是   |  模块  |
**返回结果:**

*成功*

```json
{
  "code": 0,
  "id": "1",
  "name": "模块1",
  "ability": 30,
  "courseAbility": 121,
  "data": [
    {"value": 0.12, "name": '空间与图形'}
  ]
}
```

失败

```json
{
  "code": 40004,
  "errMessage": "找不到该模块!"
}
```

| 参数            |   描述   |
| ------------- | :----: |
| id            | 模块唯一标识 |
| name          |  模块名称  |
| ability       | 模块能力值  |
| courseAbility | 学科能力值  |
| data          |  环形图   |
| data.value    | 知识点占比  |
| data.name     | 知识点名称  |



#### 知识达成度

**描述：**

- 获取指定章节知识达成度数据

**请求URL：**

- /api/advanced/chapter/:id/process

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |  说明  |
| ---- | :--: | :--: |
| id   |  是   | 章节id |
**返回结果:**

*成功*

```json
{
  "code": 0
  "data": [
    {"KN": "知识点1", "KNId":"1", "process":0.7}
  ]
}
```

*失败*

```json
{
  "code": 40005,
  "errorMessage": "找不到该章节!"
}
```

| 参数           |    描述    |
| ------------ | :------: |
| data         | 知识点达成度数据 |
| data.KN      |   知识点    |
| data.KNId    | 知识点唯一标识  |
| data.process |  知识点达成度  |



#### 闯关纪录列表

**描述：**

- 获取闯关纪录列表

**请求URL：**

- /api/practise/raid/logs

**请求方式:**

- GET

**参数:**

| 参数   | 是否必须 |     说明      |
| ---- | :--: | :---------: |
| page |  否   | 分页页码(默认为1)  |
| size |  否   | 分页条数(默认为10) |

**返回结果：**

```json
{
  "code": 0,
  "data": [
    {
      "id": "1",
      "date": '2017-06-30 14:02',
      "course": "chi",
      "status": "success",
      "chapter": "第一单元",
      "chapterId":"1"
      "checkpoint": 2
    }
  ],
  "page": 1,
  "size": 10
}
```

| 参数              |            描述             |
| --------------- | :-----------------------: |
| data            |          闯关纪录列表           |
| data.id         |         闯关纪录唯一标识          |
| data.date       |  闯关时间（YYYY-mm-dd hh:mm）   |
| data.course     |     科目（详情见***学科说明***）     |
| data.status     | 闯关状态(success:成功, failed:) |
| data.chapter    |           单元名称            |
| data.checkpoint |           关卡层级            |
| data.chapterId  |           单元标识            |
| page            |           分页页码            |
| size            |           分页条数            |



#### 闯关报告

**描述：**

- 获取指定关卡纪录闯关报告

**请求URL：**

- /api/practise/raid/log/:id/report

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |   说明   |
| ---- | :--: | :----: |
| id   |  是   | 闯关纪录id |
**返回结果：**

*成功*

```json
{
  "code": 0,
  "id": "1",
  "questions": 5,
  "CQ": 4,
  "CR": 0.8,
  "CPR": 0.9,
  "PCR": 0.8,
  "change": 0.03,
  "offer": 0.4
}
```

*失败*

```json
{
  "code": 40006,
  "errMessage": "找不到该闯关纪录!"
}
```

| 参数        |        描述         |
| --------- | :---------------: |
| questions |       题目数量        |
| CQ        |      正确的题目数量      |
| CR        |        正确率        |
| CPR       |       班级通过率       |
| PCR       |       通过正确率       |
| change    | 达成度变化率(正数上升、负数下降) |
| offer     | 能力贡献度（正数上升、负数下降）  |
| id        |     作业（闯关）标识      |



#### 闯关纪录试题

**描述：**

- 获取指定已完成闯关试题数据

**请求URL：**

- /api/practise/raid/log/:id

**请求方式:**

- GET

**参数:**

| 参数   | 是否必须 |    说明    |
| ---- | :--: | :------: |
| id   |  是   | 作业(闯关)id |

**返回结果:**

*成功*

```json
{
  "code":0,
  "course": "chi",
  "chapter": "第一章",
  "checkpoint": 3,
  "status": "failed",
  "data": [
    {
      "type": "choice",
      "id": "1",
      "NO": "1",
      "question": "1．下列各组词语中，没有错别字的一组是()",
      "selection": [
  		{"key":"A", "value":"1096-1099"},
		{"key":"B": "value":"1147-1148"},
		{"key":"C", "value": "1189-1193"},
        {"key":"D": "value": "1201-1204"}
      ],
      "answer": "A",
      "CA": "A",
      "IC": true,
      "analysis": "XXXX",
      "isFavorate": true,
    }
  ]
}
```

*失败*

```json
{
  "error": 40007,
  "errMessage": "找不到该纪录!"
}
```

| 参数              |              描述               |
| --------------- | :---------------------------: |
| type            | 试题类型(choice:单选题,  blank: 填空题) |
| id              |             题目标识              |
| NO              |             题目序号              |
| question        |              题干               |
| selection       |      选择题选项(***选择题限定***)       |
| selection.key   |             选项名称              |
| selection.value |             选项内容              |
| answer          |             我的答案              |
| CA              |             正确答案              |
| IC              |    是否正确（true:正确，false:错误）     |
| analysis        |             答案解析              |
| isFavorate      |   是否收藏(true:已收藏，false:未收藏)    |
| course          |              科目               |
| chapter         |             章节名称              |
| checkpoint      |              关卡               |
| status          |  闯关状态(success:成功, failed:失败)  |



#### 闯关游戏

**描述：**

- 获取指定知识点闯关游戏纪录

**请求URL：**

- /api/practise/raid/KN/:id

**请求方式:**

- GET

**参数:**

| 参数   | 是否必须 |  说明   |
| ---- | :--: | :---: |
| id   |  是   | 知识点id |

**返回结果:**

*成功*

```json
{
  "code": 0,
  "course": 'math',
  "module": '模块1',
  "chapter": '第一章',
  "KN": "知识点1',
  "KNid": "1",
  "checkpoint": 2,
  "ability": 760,
  "pass": 200
}
```

*失败*

```json
{
  "code": 40008,
  "errMessage": "找不到该知识点！"
}
```

| 参数            |   描述   |
| ------------- | :----: |
| course        |   学科   |
| module        |  模块名   |
| chapter       |   章节   |
| KN            |  知识点   |
| KNId          | 知识点标识  |
| checkpoint    | 当前关卡层数 |
| courseAbility | 学科能力值  |
| passCount     |  过关人数  |



#### 闯关练习

**描述：**

- 获取指定知识点指定关卡试题

**请求URL：**

- /api/practise/raid/KN/:id/checkpoint/:checkpointId

**请求方式:**

- GET

**参数:**

| 参数         | 是否必须 |  说明   |
| ---------- | :--: | :---: |
| id         |  是   | 知识点id |
| checkpoint |  是   | 关卡层数  |

**返回结果:**

*成功*

```json
{
  "code": 0,
  "duration": 1800,
  "course": "his",
  "module": "模块1",
  "capter": "第一章",
  "KN": "知识点1",
  "KNId": "1",
  "checkpoint": 2,
  "data": [
    {
      "id": "1",
      "question": '第一次十字军东征的时间是？',
      "selection": [
  		{"key":"A", "value":"1096-1099"},
		{"key":"B": "value":"1147-1148"},
		{"key":"C", "value": "1189-1193"},
        {"key":"D": "value": "1201-1204"}
      ],
  ]
}
```

*失败*

```json
{
  "code": 40009,
  "errMessage": "该关卡不存在!",
}
```

| 参数                   |     描述      |
| -------------------- | :---------: |
| course               |     学科      |
| module               |     模块名     |
| chapter              |     章节      |
| KN                   |     知识点     |
| checkpoint           |    关卡层数     |
| KNId                 |    知识点标识    |
| duration             | 做题时长(单位: 秒) |
| data                 |    题目集合     |
| data.id              |    题目标识     |
| data.question        |     题干      |
| data.selection       |    题目选项     |
| data.selection.key   |    选项名称     |
| data.selection.value |    选项内容     |



#### 提交闯关练习

**描述：**

- 提交指定知识点指定关卡试题

**请求URL：**

- /api/practise/raid/KN/:id/checkpoint/:checkpoint

**请求方式:**

- POST

**参数：**

```json
[
  {
    "id": "1",
    "answer": "A"
  }
]
```

| 参数         | 是否必须 |  说明   |
| ---------- | :--: | :---: |
| KNId       |  是   | 知识点id |
| id         |  是   | 题目id  |
| answer     |  是   | 问题答案  |
| checkpoint |  是   | 关卡层数  |

**返回结果：**

*成功*

```json
{"code":0}
```

*失败*

```json
{
  "code": 40009,
  "errMessage": "该关卡不存在!"
}
```



#### 排名

**描述：**

- 获取排名情况

**请求URL：**

- /api/advanced/rank

**请求方式:**

- GET

**参数：**

| 参数     | 是否必须 |            说明             |
| ------ | :--: | :-----------------------: |
| q      |  否   | class:班级, grade:年级 (默认年级) |
| course |  是   |            科目             |

**返回结果:**

```json
{
  "code": 0,
  "name": '小飞机',
  "classRank": 22,
  "classSize": 55,
  "gradeRank": 121,
  "gradeSize": 200,
  "points": 3600,
  "ranking": [
    {
      "rank":1, 
      "name": "张三", 
      "points": 4700, 
      "isClassmate": true}
  ]
}
```

| 参数                  |              描述              |
| ------------------- | :--------------------------: |
| name                |             我的名字             |
| classRank           |      班级排名(***班级排名限定***)      |
| classSize           |      班级人数(***班级排名限定***)      |
| gradeRank           |      年级排名(***年纪排名限定***)      |
| gradeSize           |      年级人数(***年纪排名限定***)      |
| ranking             |             排行榜              |
| ranking.rank        |              排名              |
| ranking.name        |             学生姓名             |
| ranking.points      |             学生分数             |
| ranking.isClassmate | 是否同班同学(true:是同班,false: 不是同班) |
| points              |             我的分数             |



### 学习管理

#### 思维导图

**描述:**

- 获取选定学科章节的思维导图数据

**请求URL：**

- /api/advanced/mind

**请求方式:**

- GET

**参数：**

| 参数     | 是否必须 |  说明  |
| ------ | :--: | :--: |
| id     |  是   | 章节id |
| course |  是   |  学科  |

**返回结果**

```json
{
  "code": 0,
  "questionCount": 90
  "totalMistakes": 90,
  "data": [
    {
      "KN": '古代诗文阅读',
      "KNId": '1',
      "children": [
        {
          "KN": "文章内容归纳，中心概括",
          "KNId": "2"
          "children": [
            "KN": "常见实词",
            "KNId": "3"
          ]
        },
        {
          "KN": "文章内容的理解"
          "KNId": "4",
          "collection": 9,
          "mistakes": 4,
        }
  	  ]
    },
	{
       "KN": "文章内容的理解1"
       "KNId": "5",
       "collection": 9,
       "mistakes": 4,
	}
  ]
}
```

| 参数                  |        描述        |
| ------------------- | :--------------: |
| data                |      思维导图集合      |
| data.KN             |       知识点        |
| data.KNId           |      知识点标识       |
| data.children       | 子知识点集合(***递归***) |
| children.collection |       好题数量       |
| children.mistakes   |       错题数量       |
| questionCount       |      总的做题数量      |
| totalMistakes       |      总的错题数量      |



#### 已收藏题目列表

**描述:**

- 收藏的好题以及错题列表

**请求URL：**

- /api/practise/favorite/KN/:KNId

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |                 说明                  |
| ---- | :--: | :---------------------------------: |
| q    |  否   |         mistake：错题，无参数时为好题          |
| date |  否   |       时间(YYYY-MM)  ***错题限定***       |
| type |  否   | task:作业，exam:试卷， raid:闯关 ***错题限定*** |
| KNId |  是   |                知识点Id                |
| page |  否   |                分页页码                 |
| size |  否   |                分页条数                 |

**返回结果:**

```json
{
  "code": 0,
  "data": {
    "type": "choice",
    "id": "1",
    "title": '1．下列各组词语中，没有错别字的一组是（）',
    "selection": [
       "key": "A", "value": "涟漪        踌躇      气势磅薄     诎诎",
       "key": "B", "value": "萧瑟        寂寥      诲莫如深     星辉斑澜",
       "key": "C", "value": "尸骸        作揖      直截了当     切齿拊心",
       "key": "D", "value": "夜缒        诽红      陨身不恤     秋毫无犯",
    ],
    "from": {
      "date": "2017-05-30",
      "type": "task",
      "title": "语文作业",
      "id": "1",
    }
  }
}
```

| 参数             |              描述               |
| -------------- | :---------------------------: |
| data           |             题目列表              |
| data.type      | 试题类型(choice:单选题,  blank: 填空题) |
| data.id        |            习题纪录id             |
| data.title     |             试题名称              |
| data.selection |      选择题选项(***选择题限定***)       |
| from           |             试题来源              |
| from.date      |             练习时间              |
| from.type      |     练习类型(task:作业,exam:试卷)     |
| from.title     |             练习名称              |
| from.id        |            练习／闯关id            |



#### 收藏题目

**描述:**

- 在已完成练习中收藏题目

**请求URL：**

- /api/practise/favorite

**请求方式:**

- POST

**参数：**

| 参数   | 是否必须 |  说明  |
| ---- | :--: | :--: |
| id   |  是   | 题目ID |
| pid  |  是   | 练习id |

**返回结果:**

```json
{"code": 0}
```





### 消息

#### 提示信息 

**描述:**

- 获取推送信息 

**请求URL：**

- /api/message/all

**请求方式:**

- GET

**参数:**

| 参数   |       描述        |
| ---- | :-------------: |
| page | 分页页码(**默认为1**)  |
| size | 分页条数(**默认为10**) |

**返回结果:**

```json
{
  "code": 0,
  "data": [
    {
      "type": "practice",
      "title": "第一单元检测",
      "course": "math",
      "deadline": "2017-06-22",
      "auth": "王老师",
      "id": "1",
    }
  ]
}
```

| 参数          |                描述                 |
| ----------- | :-------------------------------: |
| type        | practice: 新布置的作业, correct: 批改的作业, |
| title       |               练习标题                |
| course      |                科目                 |
| deadline    |        截止日期(***新布置作业限定***)        |
| correctTime |        批改时间(***批改作业限定***)         |
| time        |         推送日期(***推荐限定***)          |



### 公共接口

#### 获取学科信息

**描述:**

- 获取用户学科、章节信息

**请求URL：**

- /api/common/course

**请求方式:**

- GET

**参数：**

无

**返回结果:**

```json
{
  "code": 0,
  "data": [
    {
      "course": "chi",
      "name": "语文",
      "children": [
        {
          "id":"1",
          "name": "模块1",
          "children": [
            "id":"2",
            "name": "第一章"
          ]
        }
      ]
    }
  ]
}
```

| 参数                 |   描述    |
| ------------------ | :-----: |
| data               | 学科信息列表  |
| data.course        |   学科    |
| data.name          |  学科名称   |
| data.children      | 学科子节点集合 |
| data.children.id   |  章节标识   |
| data.children.name |  章节名称   |



#### 图片上传

**描述:**

- 获取用户学科、章节信息

**请求URL：**

- /api/common/image/upload

**请求方式:**

- POST

**参数：**

无

**返回结果:**

*成功*

```json
{
  "code":0,
  "mediaId": "1",
  "url": "XXXX"
}
```

*失败*

```json
{
  "code": 50001,
  "url": "图片上传失败！"
}
```

| 参数      |  描述   |
| ------- | :---: |
| mediaId | 多媒体标识 |





#### 语音上传

**描述:**

- 获取用户学科、章节信息

**请求URL：**

- /api/common/voice/upload

**请求方式:**

- POST

**参数：**

无

**返回结果:**

*成功*

```json
{
  "code":0,
  "mediaId": "1",
  "url": "XXXX"
}
```

*失败*

```json
{
  "code": 50002,
  "url": "语音上传失败！"
}
```

| 参数      |  描述   |
| ------- | :---: |
| mediaId | 多媒体标识 |





### 全局返回码说明：

| 返回码  |     说明     |
| ---- | :--------: |
| -1   | 系统繁忙，请稍候再试 |
| 0    |    请求成功    |



### 学科说明:

| 学科   |        参数名         |
| ---- | :----------------: |
| 语文   |    chi（chinese）    |
| 数学   | math (mathematics) |
| 英语   |   eng (english)    |
| 政治   |  poli (politics)   |
| 历史   |   his (history)    |
| 生物   |   bio (biology)    |
| 地理   |  geo (geography)   |
| 物理   |   phys (physics)   |
| 化学   |  chem (chemistry)  |