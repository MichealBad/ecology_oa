
# 泛微 OA 接口说明

## 1 部署环境说明

说明       |url       
------------|-----------
正式环境 | 稍微等候
测试环境 | asked

## 2 流程发起接口

### 2.1 (市内)外出公干申请流程

#### 2.1.1 请求说明
> 请求方式：POST<br>
请求URL ：[/mobile/plugin/yh/WorkOutFlow.jsp](#)


#### 2.1.2 请求参数

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
 workcode | strig | √ | 员工编号
 description | string | √ | 流程标题
 mainMap | object(map) | √ | 主表数据
 detLists | array\<array\<map\>\> | √ | 明细表数据

##### mainMap 主表数据

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
 shenqrq | string(yyyy-HH-mm) | √ | 申请日期
 waicsy | string |  | 外出事由1
 zhugjl | string | √ | 直接上级的 id

##### detLists 明细表数据

-特别说明:array\<array\<map\>\> 意思为[多个明细表，每个明细表有多条内容]
例如 array[1] 表示第一个明细表的多条数据 array[1][0] 的 map 代表第一个明细表的第一条数据；如果只有一条，把 map 封装成 array[][] 就行。

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
 waikscsj | string(yyyy-HH-mm) | √ | 外出开始日期
 waickssj1 | string(hh:mm) | √ | 外出开始时间
 waicjssj | string(yyyy-HH-mm) | √ | 外出结束日期
 waicjssj2 | string(hh:mm) | √ | 外出结束时间
 chucdd | string | √ | 外出地点

##### 请求参数示例
```json
{
  "workcode": "001702",
  "description": "外出公干测试",
  "mainMap": {
    "waicsy": "浪",
    "shenqrq": "2018-12-26",
    "zhugjl": "14688",
    "chansfy": "1"
  },
  "detLists": [
    [
      {
        "chucdd": "地球",
        "waicjssj2": "23:59",
        "waickssj1": "00:00",
        "waikscsj": "2018-12-26",
        "waicjssj": "2018-12-27"
      }
    ]
  ]
}
```

#### 2.1.3 返回参数

字段       |字段类型      |字段说明
------------|-----------|------
code | integer | 0: 成功
data | object(map) | 附加数据
msg | string | 描述

##### data 数据

字段       |字段类型      |字段说明
------------|-----------|------
requestId | string | 创建成功的流程编号

##### 返回参数示例
```json
{
  "code": 0,
  "data": {
    "requestId": "1789"
  },
  "msg": "success"
}
```

### 2.2 出差申请流程

#### 2.2.1 请求说明
> 请求方式：POST<br>
请求URL ：[/mobile/plugin/yh/BizTripFlow.jsp](#)

#### 2.2.2 请求参数

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
workcode | strig | √ | 员工编号
description | string | √ | 流程标题
mainMap | object(map) | √ | 主表数据

##### mainMap 主表数据

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
 chucdd | string | √ | 出差地点(出差起止点)
 kaisrq | string(yyyy-HH-mm) | √ | 开始日期
 kaissj | string(hh:mm) | √ | 开始时间
 jiesrq | string(yyyy-HH-mm) | √ | 结束日期
 jiessj | string(hh:mm) | √ | 结束时间
 chucts | integer |  | 出差天数
 shifxydp | integer | √ | 是否需要订票(0: 是 1: 否)
 dingplx | integer | √ | 订票类型(0: 机票 1: 火车票 2: 高铁 3: 其他)
 dingpxxsm | string | √ | 订票详细说明
 chucgzap | string |  | 出差工作安排


##### 请求参数示例
```json
{
  "workcode": "001702",
  "description": "出差测试",
  "mainMap": {
    "kaisrq": "2018-12-26",
    "dingplx": "2",
    "dingpxxsm": "哪有什么理由",
    "kaissj": "00:00",
    "shifxydp": "1",
    "shijkssj": "00:00",
    "chucts": "1",
    "chucgzap": "交给你们了",
    "chucdd": "月球表面",
    "jiesrq": "2018-12-27",
    "shijccts": "1",
    "shenqrq": "2018-12-26",
    "shijjssj": "12:00",
    "sqsyao": "我王境泽想出差",
    "shijksrq": "2018-12-26",
    "bum": "1958",
    "jiessj": "12:00",
    "zhuangjl": "14688",
    "shijjsrq": "2018-12-27",
    "chucts1": "1"
  }
}
```

#### 2.2.3 返回参数

字段       |字段类型      |字段说明
------------|-----------|------
code | integer | 0: 成功
data | object(map) | 附加数据
msg | string | 描述

##### data 数据

字段       |字段类型      |字段说明
------------|-----------|------
requestId | string | 创建成功的流程编号

##### 返回参数示例
```json
{
  "code": 0,
  "data": {
    "requestId": "1791"
  },
  "msg": "success"
}
```

## 3 流程结束回传

### 3.1 (市内)外出公干申请流程回传
#### 3.1.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
detList | array\<map\> | 明细数据
main | object | 主表数据


##### main 主表数据

字段       |字段类型      |字段说明
------------|-----------|------
gongzjjr | string | 忽略
liush | string | 流程编号
waickssj | string | 外出开始时间(忽略)
waicdd | string | 外出地点
shenqrgs | string | 申请人公司
waicjssj | string | 外出结束时间(忽略)
gangw | string | 岗位
waicsy | string | 外出事由1
shenqr | string | 申请人
waicsj01 | string | 外出结束时间(忽略)
chansfy | string | 是否产生费用
shenqrq | string | 申请日期
xianggfj | string | 相关附件(忽略)
bum | string | 部门
waicsj | string | 外出时间(忽略)
xiangglc | string | 相关流程(忽略)
gongh | string | 工号
zhugjl | string | 直接上级
sqsyao | string | 外出事由
zhij | string | 职级


##### detList 明细数据

字段       |字段类型      |字段说明
------------|-----------|------
id | string | 忽略
waikscsj | string | 外出开始日期
waickssj1 | string | 外出开始时间
waicjssj | string | 外出结束日期
waicjssj2 | string | 外出结束时间
chucdd| string | 外出地点


### 3.2 出差申请流程回传
#### 3.2.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
main | object | 主表数据


##### main 主表数据

字段       |字段类型      |字段说明
------------|-----------|------
kaisrq | string | 开始日期
dingplx | string | 订票类型
jiesrq | string | 结束日期
shenqrgs | string | 申请人公司
shenqr | string | 申请人
dingpxxsm | string | 订票详细说明
chucdd | string | 出差地点
shijjssj | string | 实际结束时间(忽略)
xianggfj | string | 相关附件(忽略)
shijkssj | string | 实际开始时间(忽略)
shifxydp | string | 是否需要订票
xiangglc | string | 相关流程(忽略)
chucgzap | string | 出差工作安排
shijccts | string | 实际出差天数
sqsyao | string | 申请事由
zhuangjl | string | 主管/经理
liush | string | 流程编号
jiessj | string | 结束时间
chucts | string | 出差天数
gangw | string | 岗位
kaissj | string | 开始时间
shenqrq | string | 申请日期
chucqjgzjjr | string | 出差期间工作交接人
bum | string | 部门
shijjsrq | string | 实际出差结束日期(忽略)
shijksrq | string | 实际出差开始日期(忽略)
gongh | string | 工号
shifxygzjj | string | 是否需要工作交接
chucts1 | string | 出差天数
zhij | string | 职级


### 3.3 费用报销申请流程回传
#### 3.3.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
detList | array\<map\> | 明细数据
liush | string | 流程编号


##### detList 明细数据

字段       |字段类型      |字段说明
------------|-----------|------
huilxz | string | 汇率(作废)
baoxje | string | 本位币报销金额
feiyms | string | 费用描述（摘要）
yuanbje | string | 原币金额
huilv | string | 汇率
hezje | string | 本位币核准金额
bizchance | string | 业务机会
biz | string | 本位币
feiycdbm | string | 费用承担部门(成本中心) id
yewulb | string | 业务类别
huil | string | 汇率【作废】
id | string | 忽略
feix | string | 费项
chengdje | string | 承担金额
chengdbm | string | 费用承担部门（成本中心）忽略
chengbzx | string | 成本中心
bib | string | 原币币别
feiylx | string | 费用类型 编码



### 3.4 出差费用报销申请流程回传
#### 3.4.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
detList | array\<map\> | 明细数据
liush | string | 流程编号


##### detList 明细数据

字段       |字段类型      |字段说明
------------|-----------|------
huil3 | string | 汇率
qitje | string | 其他金额
huil2 | string | 汇率
zhusfbxje | string | 住宿费明细
shinjtf | string | 路费明细
yuanbje | string | 原币报销金额
qtfy | string | 其他费用
bizchance | string | 业务机会
biz | string | 本位币
huil | string | 汇率（作废）
yewulb | string | 业务类别
chefbxje | string | 报销金额
id | string | 忽略
chaeyysm | string | 超额原因说明
feiygsbm | string | 费用归属部门 id
heji | string | 本位币报销金额
benwbhzje | string | 本位币核准金额
bib | string | 原币币别
chengdbm | string | 承担部门 id
chengbzx | string | 费用承担部门（成本中心）id
feiylx | string | 费用类型
chucqzdd | string | 出差起止地点
