# API接口

## 1. 添加扫描任务

#### 请求接口
接口：`/api/add`
方法：`POST`
类型：`JSON`

#### 请求参数

|参数|类型|必填|描述|例子|
|---|---|---|---|---|
|key|string|是|`config`文件中配置的`secret_key`|`{"key":"your_secret_key"}`|
|target|string或list|是|需要扫描的git地址，默认为master分支，如需指定分支或tag可在git地址末尾加上`:master`|单个项目扫描：`{"target": "https://github.com/wufeifei/dict.git:master"}`；<br>多个项目扫描：`{"target": ["https://github.com/wufeifei/dict.git:master", "https://github.com/wufeifei/autossh.git:master"]}`|

#### 响应例子
```json
{
    "code": 1001, # 状态码为1001则表示逻辑处理正常
    "result": {
        "msg": "Add scan job successfully.", # 消息
        "sid": "afbe69gxpy6h" # 扫描的任务ID（调用任务状态查询时需要用到）
    }
}
```

## 2. 查询扫描任务状态

#### 请求接口
接口：`/api/status`
方法：`POST`
类型：`JSON`

#### 请求参数

|参数|类型|必填|描述|例子|
|---|---|---|---|---|
|key|string|是|`config`文件中配置的`secret_key`|`{"key":"your_secret_key"}`|
|sid|string|是|扫描的任务ID|

#### 响应例子
```json
{
    "code": 1001, # 状态码为1001则表示逻辑处理正常
    "result": {
        "msg": "success", # 消息
        "status": "done", # 扫描状态
        "report": "?sid=afbe69v8jjme", # 扫描报告页
        "sid": "sfbe69y2sjge" # 扫描的任务ID
    }
}
```

# 完整的例子
## 启动HTTP服务
```bash
sudo python cobra.py -H 127.0.0.1 -P 80
```

## 添加扫描任务
```bash
# 添加一条任务
curl -H "Content-Type: application/json" -X POST -d '{"key":"your_secret_key", "target":"https://github.com/wufeifei/grw.git:master"}' http://127.0.0.1/api/add

# 添加多条任务
curl -H "Content-Type: application/json" -X POST -d '{"key":"your_secret_key", "target":["https://github.com/wufeifei/cobra.git:master", "https://github.com/wufeifei/grw.git:master"]}' http://127.0.0.1/api/add
```

## 查询任务状态
```bash
curl -H "Content-Type: application/json" -X POST -d '{"key":"your_secret_key","sid": "e3ea91nd1f4"}' http://127.0.0.1/api/status
```

## 查询扫描报告
```bash
curl -H "Content-Type: application/json" -X POST -d '{"key":"your_secret_key","task_id": "your_task_id"}' http://127.0.0.1/api/report
```

# Web 报告页

## 任务汇总报告
```
http://127.0.0.1/?sid=afbe69p7dxva
```

## 扫描详情报告
```
http://127.0.0.1/report/afbe69p7dxva/sfbe69plo5qs
```
---
下一章：[高级功能配置](https://wufeifei.github.io/cobra/config)