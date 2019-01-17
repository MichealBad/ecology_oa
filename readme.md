## 部署说明

- 拷贝代码到本地: git clone http://47.104.165.194:3000/yhglobal.com/YHWeaver.git
- 进入 src/main 下，使用 maven 构建项目: mvn clean install
- 将构建好的 war (crm 项目下的 target) 放置于 tomcat 目录的 webapps/ROOT/ 下
- 启动 tomcat : ./bin/startup.sh
- 关闭 tomcat 的时候，最好用 kill -9 PID 杀掉(ps -aux\|grep eas_proxy 可以查到 PID)。



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
 applyDate | string(yyyy-HH-mm) | √ | 申请日期
 outgoReason | string |  | 外出事由
 supervisor | string | √ | 直接上级的工号

##### detLists 明细表数据

-特别说明:array\<array\<map\>\> 意思为[多个明细表，每个明细表有多条内容]
例如 array[1] 表示第一个明细表的多条数据 array[1][0] 的 map 代表第一个明细表的第一条数据；如果只有一条，把 map 封装成 array[][] 就行。

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
 outgoDate | string(yyyy-HH-mm) | √ | 外出开始日期
 outgoTime | string(hh:mm) | √ | 外出开始时间
 returnDate | string(yyyy-HH-mm) | √ | 外出结束日期
 returnTime | string(hh:mm) | √ | 外出结束时间
 outgoPlace | string | √ | 外出地点

##### 请求参数示例
```json
{
  "workcode": "001702",
  "description": "外出公干测试",
  "mainMap": {
    "outgoReason": "浪",
    "applyDate": "2018-12-26",
    "supervisor": "001702"
  },
  "detLists": [
    [
      {
        "returnDate": "2018-12-27",
        "outgoDate": "2018-12-26",
        "outgoTime": "00:00",
        "outgoPlace": "地球",
        "returnTime": "23:59"
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
requestId | string | | 流程编号, 该参数不为空的时候，重新提交到该流程；用于被回退的流程
workcode | string | √ | 员工编号
description | string | √ | 流程标题
mainMap | object(map) | √ | 主表数据

##### mainMap 主表数据

字段       |字段类型      |必填  |字段说明
------------|-----------|:-----------:|------
outgoDays | string |  | 出差天数
actOutgoDays | string |  | 实际出差天数
applyDate | string |  | 申请日期
applyReason | string |  | 申请事由
supervisor | string | √ | 上级工号
outgoDate | string(yyyy-HH-mm) | √ | 出差开始日期
outgoTime | string(hh:mm) | √ | 出差开始时间
returnDate | string(yyyy-HH-mm) | √ | 出差结束日期
returnTime | string(hh:mm) | √ | 出差结束时间
outgoPlace | string | √ | 出差地点
outgoBizArrangement | string |  | 出差工作安排
needTicket | string | √ | 是否需要订票(0: 需要 1: 不需要)
ticketType | string |  | 订票类型(0: 机票 1: 火车票 2: 高铁 3: 其他)
ticketStatement | string |  | 订票详细说明


##### 请求参数示例
```json
{
  "workcode": "001254",
  "description": "出差测试",
  "mainMap": {
    "needTicket": "0",
    "outgoDays": "1",
    "applyReason": "我王境泽想出差",
    "ticketType": "2",
    "outgoDate": "2018-12-26",
    "outgoTime": "00:00",
    "returnTime": "12:00",
    "returnDate": "2018-12-27",
    "actOutgoDays": "1",
    "ticketStatement": "哪有什么理由",
    "applyDate": "2018-12-26",
    "outgoBizArrangement": "交给你们了",
    "outgoPlace": "月球表面",
    "supervisor": "001702"
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
result | integer | 流程结果 0: 成功 其它: 失败 该流程只是归档，默认为 0
nodesLog | object | 节点运行的情况，该流程只是归档，默认为空
requestId | string | 流程 id



### 3.2 出差申请流程回传
#### 3.2.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
result | integer | 流程结果 0: 成功 其它: 失败
nodesLog | object | 节点运行的情况
requestId | string | 流程 id

##### nodesLog 参数

字段       |字段类型      |字段说明
-----------|-----------|------
remark | string | 如果流程被回退，该字段为回退批注

### 3.3 费用报销申请流程回传
#### 3.3.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
detList | array\<map\> | 明细数据
serialNo | string | 流程编号


##### detList 明细数据

字段       |字段类型      |字段说明
------------|-----------|------
bizChance | string | 业务机会
originCurrencyType | string | 原币币别
exRate | string | 汇率
bizCategory | string | 业务类别
costCategory | string | 费用类型
localCrnReimAmount | string | 本位币报销金额
expenseDesc | string | 费用描述（摘要）
originCurrencyAmount | string | 原币金额



### 3.4 出差费用报销申请流程回传
#### 3.4.1 回传参数

字段       |字段类型      |字段说明
------------|-----------|------
detList | array\<map\> | 明细数据
serialNo | string | 流程编号


##### detList 明细数据

字段       |字段类型      |字段说明
------------|-----------|------
bizChance | string | 业务机会
originCurrencyType | string | 原币币别
bizCategory | string | 业务类别
costCategory | string | 费用类型
outgoPlace | string | 出差起止地点
excessReason | string | 超额原因说明
reimAmount | string | 报销金额
exRate | string | 汇率
localCrnReimAmount | string | 本位币报销金额
originCurrencyAmount | string | 原币报销金额
