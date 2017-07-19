## 作业系统API文档v0.1

##### 全局返回码说明：

| 返回码  |     说明     |
| ---- | :--------: |
| -1   | 系统繁忙，请稍候再试 |
| 0    |    请求成功    |
##### 学科说明:

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
## 接口说明：

##### 1.个人画像

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
  code: 0,
  chi: 60,
  math: 60,
  eng: 60,
  poli: 60,
  his: 60,
  bio: 60,
  geo: 60,
  phys: 60,
  chem: 60,
  total: 540,
  classRank: 30, 
  classSize: 50,
  gradeRank: 200,
  gradeSize: 1000,
  
}
```

| 参数        |  描述   |
| --------- | :---: |
| chi       | 语文能力值 |
| math      | 数学能力值 |
| eng       | 英语能力值 |
| poli      | 政治能力值 |
| his       | 历史能力值 |
| bio       | 生物能力值 |
| geo       | 地理能力值 |
| phys      | 物理能力值 |
| chem      | 化学能力值 |
| total     | 总能力值  |
| classRank | 班级排名  |
| classSize | 班级人数  |
| gradeRank | 年纪排名  |
| gradeSize | 年纪人数  |
​	

##### 2. 作业考试

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
  code: 0,
  data:[
    {
      id: '1',
      type: 'task',
      course: 'his',
      auth: '王老师',
      title:'欧洲中世纪史第一课时课后检测',
      deadline: '2017-07-18',
      status: 'tbd',
    }
  ],
  page: 1,
  size: 10,
}
```

```json
#已完成列表
{
  code: 0
  data: [
  	{
      date: '2017-7-18',
      collection: [
  		{	
    	  id: '1',
          type: 'task',
          course: 'his',
          title:'欧洲中世纪史第一课时课后检测',
          deadline: '2017-07-18',
  		  submitTime: '2017-07-18',
  		  correctTime: '2017-07-19',
          status: 'corrected'
		}
      ]
	}
  ]
}
```

```json
#学情报告(只有试卷有学情报告)
{
  code: 0,
  data: [
    {
      id: '1',
      title: '欧洲中世纪史第一单元练习卷',
      date: '2017-7-18',
    }
  ],
  page: 1,
  size: 10,
}
```

| 参数          |                    说明                    |
| ----------- | :--------------------------------------: |
| id          |                   唯一标识                   |
| type        |          练习类型(task:作业, exam:试卷)          |
| course      |            科目(详情见表***学科说明***)            |
| title       |                   练习题目                   |
| deadline    |             截止时间(YYYY-MM-DD)             |
| submitTime  |       提交时间(YYYY-MM-DD)，***已完成限定***       |
| correctTime |     批改时间(YYYY-MM-DD),   ***已完成限定***      |
| status      | 练习状态(tbd:'待完成',submitted:'已提交', corrected:'已批改') |
| page        |                   当前页码                   |
| size        |                   分页条数                   |


##### 3. 练习详情

**描述：**

- 获取指定练习详情

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
  code: 0,
  type: 'task',
  title: '欧洲中世纪史第一课时课后检测',
  course: 'his'
  data: [
    {
      id: '1',
      type: 'choice',
      question: '第一次十字军东征的时间是？',
      selection: {
        A: '1096-1099',
        B: '1147-1148',
        C: '1189-1193',
        D: '1201-1204'
      },
    },
    {
      id: '2',
      type: 'objective'
      question: '简述宗教改革给欧洲带来了那些影响？',
    }
  ] 
}

```

```json
#exam 试卷
{
  code: 0,
  id: '10086',
  type: 'exam',
  title: '欧洲中世纪史第一单元单元卷' ,
  course: 'his',
  totalPoints: 100,
  data: [
    {
      type: 'choice',
      collection: [
        {
          id: '1',
          question: '第一次十字军东征的时间是？',
          selection: {
            A: '1096-1099',
            B: '1147-1148',
            C: '1189-1193',
            D: '1201-1204'
          },
        }
      ],
      points: 2,
      number: 10
    },
    {
      type: 'blank'
      collection: [
        {
          id: '2',
      	  question: '____将亨利四世开除了教籍?'
        }
      ],
	  points: 1,
	  number: 20,	
    }
  ]
}
```



| 参数             |                    描述                    |
| -------------- | :--------------------------------------: |
| id             |                  练习唯一标识                  |
| type           |         练习类型(task: 作业, exam: 试卷)         |
| title          |                   练习标题                   |
| course         |                    学科                    |
| totalPoints    |              总分（***试卷限定***）              |
| type(list)     | 题目类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| id(collection) |                  题目唯一标识                  |
| question       |                    题干                    |
| selection      |             选项(***单选题限定***)              |
| points         |                    分值                    |
| number         |                 题目/空格数量                  |

##### 

##### 4. 提交练习

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
    id: '1',
    answer: 'A'
  },
  {
    id: '2',
    answer:[
      '格列高利七世'
    ]
  }
]
```



| 参数       | 是否必须 |           说明           |
| -------- | :--: | :--------------------: |
| id       |  是   |          练习id          |
| id(list) |  是   |          题目id          |
| answer   |  是   | 答案（填空拥有多个答案，将答案放在list） |

**返回结果：**

```json
{code: 0}
```



##### 5. 已完成内容详情

**描述：**

- 获取已完成作业考试内容

**请求URL：**

- /api/practice/:id/done 

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |  说明   |
| ---- | :--: | :---: |
| id   |  是   | 已完成练习 |
**返回结果:**

*成功:*

```json
#task 作业
{
  code: 0,
  type: 'task',
  title: '欧洲中世纪史第一课时课后检测',
  course: 'his'
  data: [
    {
      id: '1',
      type: 'choice',
      question: '第一次十字军东征的时间是？',
  	  KNId: '1',
  	  KN: '十字军',
  	  examine: '分析',
  	  CR: 0.8,
  	  CA: 'A',
      answer: 'A',
  	  IC: true,
      selection: {
        A: '1096-1099',
        B: '1147-1148',
        C: '1189-1193',
        D: '1201-1204'
      },
    },
    {
      id: '2',
      type: 'objective',
      KNID: '2',
      KN: '宗教改革',
      examine: '分析',
  	  CR: 0.8,
      CA: ' ',
      answer: ' ',
      score: '5.5',
      question: '简述宗教改革给欧洲带来了那些影响？',
    }
  ] 
}
```

```json
#exam 试卷
{
  code: 0,
  id: '10086',
  type: 'exam',
  title: '欧洲中世纪史第一单元单元卷' ,
  course: 'his',
  totalPoints: 100,
  score: 85
  data: [
    {
      type: 'choice',
      collection: [
        {
          id: '1',
          question: '第一次十字军东征的时间是？',
          KNId: '1',
          KN: '十字军',
          examine: '分析',
          CR: 0.8,
          CA: 'A',
          answer: 'A',
  		  IC: true,
          selection: {
            A: '1096-1099',
            B: '1147-1148',
            C: '1189-1193',
            D: '1201-1204'
          },
        }
      ],
      points: 2,
      number: 10
    },
    {
      type: 'blank'
      collection: [
        {
          id: '2',
      	  question: '____将亨利四世开除了教籍?'
      	  KNId: '2',
          KN: '宗教改革',
          examine: '分析',
          CR: 0.8,
          CA: '格列高利七世',
          answer: '格列高利七世',	
      	  IC: true,
    	}
      ],
	  points: 1,
	  number: 20,	
    }
  ]
}
```

失败:

```json
{code: 40001, errMessage:'您还没有完成该练习'}
```

| 参数            |                    描述                    |
| ------------- | :--------------------------------------: |
| id            |                  练习唯一标识                  |
| type          |         练习类型(task: 作业, exam: 试卷)         |
| title         |                   练习标题                   |
| course        |                    学科                    |
| totalPoints   |              总分（***试卷限定***）              |
| type[list]    | 题目类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| collection-id |                  题目唯一标识                  |
| question      |                    题干                    |
| selection     |             选项(***单选题限定***)              |
| KN            |                   知识点                    |
| KNId          |                 知识点唯一标识                  |
| CR            |                   正确率                    |
| examine       |         考查能力(记忆、理解、运用、分析、评价、创新)          |
| totalScore    |             总得分(***试卷限定***)              |
| score         |                   题目得分                   |
| CA            |                   正确答案                   |
| answer        |                   我的答案                   |
| IC            |   是否正确（true:正确， false）, ***非主观题目限定***    |

##### 

##### 6. 学情报告详情

**描述：**

- 获取指定试卷学情报告

**请求URL：**

- /api/practice/:id/report

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |  说明   |
| ---- | :--: | :---: |
| id   |  是   | 已完成试卷 |

**返回结果:**

*成功*：

```json
{
  code: 0,
  totalScore: 85,
  classRank: 12,
  classSize: 50,
  gradeRank: 18,
  gradeSize: 600,
  SN: 10,
  STP: 45,
  STS: 40,
  ON : 10,
  OC: 15,
  OS: 30,
  competence: [
   	{name: '理解能力', points: 10},
  ]
  data: [
   {
     KNId:'1'
     KN: '立体几何',
     process: 0.03
     map:[
       {id: '1', no: '1', points: 20, score: 16, avg: 14.25}
     ]
   } 
  ]
}
```

*失败:*

```json
{
  code: 40002,
  errMessage: '找不到该学情报告'
}
```

| 参数                |      描述      |
| ----------------- | :----------: |
| totalScore        |     总得分      |
| classRank         |     班级排名     |
| gradeRank         |     年纪排名     |
| SN                |    主观题数量     |
| STP               |    主观题总分     |
| STS               |    主观题得分     |
| ON                |    客观题数量     |
| OC                |   客观题正确数量    |
| OS                |    客观题得分     |
| competence [list] |      素养      |
| competence-name   |     素养名称     |
| competence-points |     素养得分     |
| KNId              |    知识点id     |
| KN                |    知识点名称     |
| process           |     达成度      |
| map[list]         | 同一知识点下面试题的集合 |
| map-id            |     试题id     |
| map-no            |  试题在本卷中的题号   |
| points            |    试题的分值     |
| score             |   该试题我的得分    |
| avg               |   该试题班级得分    |


##### 7.学科能力值信息

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
  code: 0,
  term: 2,
  course: 'math',
  ability: 121,
  classRank: 20,
  gradeRank: 20,
  data: [
    {value: '40', name: '模块1',id: 1}
  ]
}
```

*失败*

```json
{
  code: 40003,
  errMessage: '暂无该学期数据！'
}
```

| 参数         |            描述             |
| ---------- | :-----------------------: |
| term       | 学期(高一上到高三下一次为1、2、3、4、5、6) |
| course     |    学科(详情见表***学科说明***)     |
| ability    |            能力值            |
| classRank  |           班级排名            |
| gradeRank  |            年纪             |
| data[list] |         南丁格尔玫瑰图数据         |
| data-value |           模块分值            |
| data-name  |           模块名称            |
| data-id    |          模块唯一标识           |


##### 8. 学科能力值细化

**描述：**

- 获取指定模块的环形图数据

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
  code: 0,
  id: 1,
  name: '模块1'，
  ability: 30,
  courseAbility: 121,
  data: [
    {value: 0.12, name: '空间与图形'}
  ]
}
```

失败

```json
{
  code: 40004,
  errMessage: '找不到该模块!'
}
```

| 参数            |   描述   |
| ------------- | :----: |
| id            | 模块唯一标识 |
| name          |  模块名称  |
| data[list]    |  环形图   |
| data-value    | 知识点占比  |
| data-name     | 知识点名称  |
| ability       | 模块能力值  |
| courseAbility | 学科能力值  |


##### 9. 知识达成度

**描述：**

- 获取指定章节知识达成度数据

**请求URL：**

- /api/advanced/chapter/:id/process

**请求方式:**

- GET

**参数：**

| 参数     | 是否必须 |  说明  |
| ------ | :--: | :--: |
| id     |  是   | 章节id |
| course |  是   |  学科  |
**返回结果:**

*成功*

```json
{
  code: 0
  data: [
    {KN: '知识点1', KNId:1, process:0.7}
  ]
}
```

*失败*

```json
{
  code: 40005,
  errorMessage: '找不到该章节!'
}
```

| 参数         |    描述    |
| ---------- | :------: |
| data[list] | 知识点达成度数据 |
| list-KN    |   知识点    |
| list-KNId  | 知识点唯一标识  |
| process    |  知识点达成度  |


##### 10. 闯关纪录列表 

**描述：**

- 获取闯关纪录列表

**请求URL：**

- /api/raid/logs

**请求方式:**

- GET

**参数：**

无

**返回结果：**

```json
{
  code: 0,
  data: [
    {
      id: 1,
      date: '2017-06-30 14:02',
      course: 'chi',
      status: 'success',
      title: '第一单元',
      checkpoint: 2
    }
  ]
}
```

| 参数         |            描述             |
| ---------- | :-----------------------: |
| id         |         闯关纪录唯一标识          |
| date       |  闯关时间（YYYY-mm-dd hh:mm）   |
| course     |     课程（详情见***学科说明***）     |
| status     | 闯关状态(success:成功, failed:) |
| title      |           关卡名称            |
| checkpoint |           关卡层级            |


##### 11. 闯关报告

**描述：**

- 获取指定关卡纪录闯关报告

**请求URL：**

- /api/raid/log/:id/report

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
  code: 0,
  id: 1,
  questions: 5,
  CQ: 4,
  CR: 0.8,
  CPR: 0.9,
  PCR: 0.8，
  change: 0.03,
  offer: 0.4
}
```

*失败*

```json
{
  code: 40006,
  errMessage: '找不到该闯关纪录!'
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
| id        |     闯关纪录唯一标识      |


##### 12. 闯关纪录试题

**描述：**

- 获取指定已完成闯关试题数据

**请求URL：**

- /api/raid/log/:id

**请求方式:**

- GET

**参数:**

| 参数   | 是否必须 |   说明   |
| ---- | :--: | :----: |
| id   |  是   | 闯关纪录id |

**返回结果:**

*成功*

```json
{
  code:0,
  data: [
    {
      type: 'choice',
      id: '1',
      title: '1．下列各组词语中，没有错别字的一组是（）'，
      selection: {
        A: '涟漪        踌躇      气势磅薄     诎诎',
        B: '萧瑟        寂寥      诲莫如深     星辉斑澜',
        C: '尸骸        作揖      直截了当     切齿拊心',
        D: '夜缒        诽红      陨身不恤     秋毫无犯',
      },
      answer: 'A',
      CA: 'A',
      IC: true,
      analysis: 'XXXX',
    }
  ]
}
```

*失败*

```json
{
  error: 40007,
  errMessage: '找不到该纪录!'
}
```

| 参数        |                    描述                    |
| --------- | :--------------------------------------: |
| type      | 试题类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| id        |                   题目标识                   |
| title     |                    题干                    |
| selection |            选择题选项(***选择题限定***)            |
| answer    |                   我的答案                   |
| CA        |                   正确答案                   |
| IC        |          是否正确（true:正确，false:错误）          |
| analysis  |                   答案解析                   |



##### 13. 闯关游戏

**描述：**

- 获取指定知识点闯关游戏纪录

**请求URL：**

- /api/raid/KN/:id

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
  code: 0,
  course: 'math',
  module: '模块1',
  chapter: '第一章',
  KN: '知识点1',
  KNid: '1',
  checkpoint: 2,
  ability: 760,
  pass: 200
}
```

*失败*

```json
{
  code: 40008,
  errMessage: '找不到该知识点！'
}
```

| 参数         |  描述   |
| ---------- | :---: |
| course     |  学科   |
| module     |  模块名  |
| chapter    |  章节   |
| KN         |  知识点  |
| checkpoint | 关卡层数  |
| ability    | 总能力值  |
| pass       |  过关   |
| KNId       | 知识点标识 |



##### 14. 闯关练习

**描述：**

- 获取指定知识点指定关卡试题

**请求URL：**

- /api/raid/KN/:id/checkpoint/:checkpoint

**请求方式:**

- GET

**参数:**

| 参数         | 是否必须 |  说明   |
| ---------- | :--: | :---: |
| id         |  是   | 知识点id |
| checkpoint |  是   |  关卡   |

**返回结果:**

*成功*

```json
{
  code: 0,
  duration: 1800,
  course: 'his',
  module: '模块1',
  capter: '第一章',
  KN: '知识点1',
  KNId:'1',
  checkpoint: 2,
  data: [
    {
    id: '1',
    question: '第一次十字军东征的时间是？',
    selection: {
      A: '1096-1099',
      B: '1147-1148',
      C: '1189-1193',
      D: '1201-1204'
    },
  ]
}
```

*失败*

```json
{
  code: 40009,
  errMessage: '该关卡不存在!'
}
```

| 参数         |     描述      |
| ---------- | :---------: |
| course     |     学科      |
| module     |     模块名     |
| chapter    |     章节      |
| KN         |     知识点     |
| checkpoint |    关卡层数     |
| KNId       |    知识点标识    |
| duration   | 做题时长(单位: 秒) |
| data[list] |     选择      |
| id         |    题目标识     |
| question   |     题干      |
| selection  |    题目选项     |



##### 15. 提交闯关练习

**描述：**

- 提交指定知识点指定关卡试题

**请求URL：**

- /api/raid/KN/:id/checkpoint/:checkpoint

**请求方式:**

- POST

**参数：**

```json
[
  {
    id: '1',
    answer: 'A'
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
{code:0}
```

*失败*

```json
{
  code: 40009,
  errMessage: '该关卡不存在!'
}
```



##### 16. 排名

**描述：**

- 获取排名情况

**请求URL：**

- /api/advanced/rank

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |            说明            |
| ---- | :--: | :----------------------: |
| q    |  否   | class:班级, grade:年纪 (默认为) |

**返回结果:**

```json
{
  code: 0,
  name: '小飞机',
  classRank: 22,
  classSize: 55,
  gradeRank: 121,
  gradeSize: 200,
  points: 3600,
  ranking: [
    {rank:1, name: '张三', points: 4700}
  ]
}
```

| 参数             |         描述         |
| -------------- | :----------------: |
| name           |        我的名字        |
| classRank      | 班级排名(***班级排名限定***) |
| classSize      | 班级人数(***班级排名限定***) |
| gradeRank      | 年级排名(***年纪排名限定***) |
| gradeSize      | 年级人数(***年纪排名限定***) |
| ranking[lisnt] |        排行榜         |
| ranking-rank   |         排名         |
| ranking-name   |        学生姓名        |
| ranking-points |        学生分数        |
| points         |        我的分数        |



##### 17. 学习管理

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
  code: 0,
  questions: 90
  totalMistakes: 90,
  KN: '古代诗文阅读',
  KNId: '1',
  children: [
    {
      KN: '文章内容归纳，中心概括',
      KNId: '2'
      children: [
        KN: '常见实词',
      	KNId: '3'
      ]
    },
    {
      KN: '文章内容的理解'
      KNId: '4',
      collection: 9,
      mistakes: 4,
    }
  ]
}
```

| 参数            |   描述   |
| ------------- | :----: |
| KN            |  知识点   |
| KNId          | 知识点标识  |
| children      |  子知识点  |
| collection    |  好题数量  |
| mistakes      |  错题数量  |
| questions     | 总的做题数量 |
| totalMistakes | 总的错题数量 |



##### 18. 收藏题目列表

**描述:**

- 收藏的好题以及错题列表

**请求URL：**

- /api/practise/favorite

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |                  说明                  |
| ---- | :--: | :----------------------------------: |
| q    |  否   |          mistake：错题，无参数时为好题          |
| date |  否   |       时间(YYYY-MM)  ***错题限定***        |
| type |  否   | task:作业, exam:试卷, raid:闯关 ***错题限定*** |

**返回结果:**

```json
{
  code: 0,
  data: {
    type: 'choice',
    id: '1',
    title: '1．下列各组词语中，没有错别字的一组是（）'，
    selection: {
       A: '涟漪        踌躇      气势磅薄     诎诎',
       B: '萧瑟        寂寥      诲莫如深     星辉斑澜',
       C: '尸骸        作揖      直截了当     切齿拊心',
       D: '夜缒        诽红      陨身不恤     秋毫无犯',
    },
    from: {
      date:'2017-05-30',
      task: 'task',
      course: 'chi'
    }
  }
}
```

| 参数             |                    描述                    |
| -------------- | :--------------------------------------: |
| data[list]     |                   题目列表                   |
| data-type      | 试题类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| data-title     |                   试题名称                   |
| data-selection |            选择题选项(***选择题限定***)            |
| from           |                   试题来源                   |
| from-date      |                   练习时间                   |
| from-task      |          练习类型(task:作业,exam:试卷)           |
| from-course    |                    科目                    |
| id             |                   题目id                   |



##### 19. 收藏题目

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
| pid  |  是   | 试卷id |

**返回结果:**

```json
{code: 0}
```



##### 20.好题解析

**描述:**

- 获取指定收藏试题信息

**请求URL：**

- /api/practise/favorite/:id

**请求方式:**

- GET

**参数：**

| 参数   | 是否必须 |  说明  |
| ---- | :--: | :--: |
| id   |  是   | 题目ID |

**返回结果:**

*成功*

```json
{
  code: 0,
  type: 'choice',
  id: '1',
  title: '1．下列各组词语中，没有错别字的一组是（）'，
  selection: {
    A: '涟漪        踌躇      气势磅薄     诎诎',
    B: '萧瑟        寂寥      诲莫如深     星辉斑澜',
    C: '尸骸        作揖      直截了当     切齿拊心',
    D: '夜缒        诽红      陨身不恤     秋毫无犯',
  },
  from: {
    date:'2017-05-30',
    task: 'task',
    course: 'chi'
  },
  answer: 'A',
  CA: 'A',
  IC: true,
  CR: 0.6,
  analysis: 'XXXXX',
  KN: 'XXXX',
  remark: {
    type: 'media,
    url: 'XXX',
  }
  cause: [
    {type: 'text', content:'看错'},
    {type: 'media', url: 'XXX'}
  ]
}
```

*失败*

```json
{code: 40010, errMessage: '找不到该试题！'}
```

| 参数            |                    描述                    |
| ------------- | :--------------------------------------: |
| type          | 试题类型(choice:单选题,  blank: 填空题，  subjective:主观题目) |
| title         |                   试题名称                   |
| selection     |            选择题选项(***选择题限定***)            |
| from          |                   试题来源                   |
| from-date     |                   练习时间                   |
| from-task     |      练习类型(task:作业,exam:试卷,raid:闯关)       |
| from-course   |                    科目                    |
| answer        |                   我的答案                   |
| CA            |                   正确答案                   |
| IC            |          是否正确（true:正确，false:错误）          |
| CR            |                   正确率                    |
| analysis      |                   答案分析                   |
| KN            |                   知识点                    |
| remark        |                    备注                    |
| remark-type   |                   备注类型                   |
| remark-url    |              ***多媒体备注限定***               |
| cause         |                   错误原因                   |
| cause-type    |      输出类型(media:多媒体（语音）, text: 文本)       |
| cause-content |                ***文本限定***                |
| cause-url     |             ***多媒体限定***，语音地址             |



##### 21. 提示信息

**描述:**

- 获取推送信息 

**请求URL：**

- /api/message/all

**请求方式:**

- GET

**参数：**

```json
{
  code: 0,
  data: [
    {
      type: 'practice',
      title: '第一单元检测'
      course: 'math',
      deadline: '2017-06-22',
      auth: '王老师'
    }
  ]
}
```

| 参数          |                    描述                    |
| ----------- | :--------------------------------------: |
| type        | practice: 新布置的作业, correct: 批改的作业, proposal：推荐 |
| title       |                   练习标题                   |
| course      |                    科目                    |
| deadline    |           截止日期(***新布置作业限定***)            |
| correctTime |            批改时间(***批改作业限定***)            |
| time        |             推送日期(***推荐限定***)             |

