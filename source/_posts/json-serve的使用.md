---
title: json-serve的使用
date: 2021-02-24 22:44:44
categories: 工具
---

**一、安装json-serve**

1.npm install -g json-server

如果报错：[Hostname/IP does not match certificate's altnames: Host: registry.cnpmjs.org. is not in the cert's altnames: DNS:r.cnpmjs.org](https://www.cnblogs.com/xscn1230/p/nuonuo1230.html)

解决办法：**npm set strict-ssl false**

2.json-server -v

3.新建文件夹/serve/data.json,在data.json中添加json数据**，如下：**

```json
{  
    "data": {    
        "success": 1,   
        "msg": "操作成功！",    
        "body": {      
            "deviceStateList": [  
                [        
                    {      
                        "statusTime": "2020-8-21 12:58:35",      
                        "duration": "",        
                        "status": "1",          
                        "statusName": "运行"       
                    },        
                    {         
                        "statusTime": "2020-8-21 12:58:35",          
                        "duration": "",          
                        "status": "2",        
                        "statusName": "故障停机"  
                    }      
                ],  
                [     
                    {     
                        "statusTime": "2020-8-21 12:58:35",   
                        "duration": "",          
                        "status": "1",       
                        "statusName": "运行" 
                    },      
                    {        
                        "statusTime": "2020-8-21 12:58:35",        
                        "duration": "",      
                        "status": "2",         
                        "statusName": "故障停机"      
                    }       
                ]     
            ]
        }
    }
}
```

4.运行（cmd）

﻿json-server data.json

**Tips**:json-server --watch data.json   实时监听data.json的变化

**二、在vue中使用json-server**

1.根目录下创建data.json

2.在package.json中的scripts中配置

```json
"json": "json-server data.json --port 3003"
```

﻿3.npm run json运行

4.访问http://localhost:3003/data