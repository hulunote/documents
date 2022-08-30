# R2D2WebHook管理接口

- [R2D2WebHook管理接口](#r2d2webhook管理接口)
  - [注册Webhook](#注册webhook)
  - [更新Webhook信息](#更新webhook信息)
  - [获取已注册的webhook信息](#获取已注册的webhook信息)
  - [添加(自己注册的)Webhook用户](#添加自己注册的webhook用户)
  - [移除(自己注册的)Webhook用户](#移除自己注册的webhook用户)
  - [获取(自己注册的)Webhook的所有用户](#获取自己注册的webhook的所有用户)
  - [获取已加入的所有Webhook信息](#获取已加入的所有webhook信息)
  - [更新自己的Identity](#更新自己的identity)
  - [离开Webhook](#离开webhook)

## 注册Webhook

`POST https://www.hulunote.com/r2d2/webhook/register`

* 请求body参数:
  
  ```json
  {
    "name": "webhook名称",
    "desc": "描述",
    "secret": "与葫芦交互的secret，不可泄漏，泄漏可以重新更新",
    "user-server-url": "https://this-is-a-url", // 用户server的url信息
    "self-identity": "注册者自己的身份信息" // 可不传
  }
  ```

* 响应结果：
  
  ```json
  {
    "id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
    "name": "new-test2",
    "description": "test-decs",
    "secret": "this is new secret",
    "server-url": "https://this-is-a-url",
    "is-active": false,
    "created-at": "2022-06-13T11:37:46Z",
    "updated-at": "2022-06-13T11:37:46Z"
  }
  ```

* 响应结果 - 错误:

  ```json
  {
    "error": "用户已存在同名的wehbook记录"
  }
  ```

## 更新Webhook信息

`POST https://www.hulunote.com/r2d2/webhook/update-register`

* 请求参数：
  
  ```json
  {
    "name": "webhook名称", // 不可重复
    "desc": "描述",
    "secret": "与葫芦交互的secret，不可泄漏，泄漏可以重新更新",
    "user-server-url": "https://this-is-a-url" // 用户server的url信息
  }
  ```

* 响应结果：

  ```json
  {
    "success": true
  }
  ```

* 响应结果 - 错误:

  ```json
  {
    "error": "用户已存在同名的wehbook记录"
  }
  ```

## 获取已注册的webhook信息

`POST https://www.hulunote.com/r2d2/webhook/get-registered-infos`

* 请求参数：(无)
  
  ```json
  { }
  ```

* 响应结果：

  ```json
  {
    "success": true,
    "registers": [
        {
            "id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
            "name": "new-test2",
            "description": "test-decs",
            "secret": "this is new secret",
            "server-url": "https://this-is-a-url",
            "is-active": false,
            "created-at": "2022-06-13T11:37:46Z",
            "updated-at": "2022-06-13T11:37:46Z"
        }
    ]
  }
  ```

## 添加(自己注册的)Webhook用户

`POST https://www.hulunote.com/r2d2/webhook/add-webhook-user`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "account-id": "用户ID", // 与username只需一个即可
    "username": "用户名称"   // 与account-id只需一个即可
  }
  ```

* 响应结果：

  ```json
  {
    "success": true
  }
  ```

## 移除(自己注册的)Webhook用户

`POST https://www.hulunote.com/r2d2/webhook/remove-webhook-user`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "account-id": "用户ID" // 获取的列表可以获取，用户无感知
  }
  ```

* 响应结果：

  ```json
  {
    "success": true
  }
  ```

## 获取(自己注册的)Webhook的所有用户

`POST https://www.hulunote.com/r2d2/webhook/get-all-webhook-users`

* 请求参数：

  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9"
  }
  ```

* 响应结果：

  ```json
  {
    "success": true,
    "users": [
        {
            "id": "f9721064-eb09-11ec-995d-b7ff714b075d",
            "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
            "account-id": 1,
            "identity": "******",
            "created-at": "2022-06-13T11:14:21Z",
            "updated-at": "2022-06-13T11:14:21Z"
        },
        {
            "id": "0f2b122e-eb0b-11ec-9b12-c756eec9af4d",
            "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
            "account-id": 4,
            "identity": "******",
            "created-at": "2022-06-13T11:22:07Z",
            "updated-at": "2022-06-13T11:22:07Z"
        }
    ]
  }
  ```

## 获取已加入的所有Webhook信息

`POST https://www.hulunote.com/r2d2/webhook/get-myself-webhook-users`

* 请求参数：(无)
  
  ```json
  { }
  ```

* 响应结果：

  ```json
  {
    "success": true,
    "infos": [
        {
            "id": "3f1f5e5c-eb0d-11ec-9b67-f72b2f7091f0",
            "register-id": "3f1995e4-eb0d-11ec-b3a9-db33eb977200",
            "webhook-name": "new-test2",
             "webhook-description": "test-decs",
            "webhook-server-url": "https://this-is-a-url",
            "identity": "",
            "account-id": 9366,
            "created-at": "2022-06-13T11:37:46Z",
            "updated-at": "2022-06-13T11:37:46Z",
            
        }
    ]
  }
  ```

## 更新自己的Identity

`POST https://www.hulunote.com/r2d2/webhook/update-webhook-user-idenity`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9",
    "new-identity": "新的身份信息"
  }
  ```

* 响应结果：

  ```json
  {
    "success": true
  }
  ```

## 离开Webhook

`POST https://www.hulunote.com/r2d2/webhook/leave-webhook`

* 请求参数：
  
  ```json
  { 
    "register-id": "f96c87fc-eb09-11ec-b141-b7768de0fdc9"
  }
  ```

* 响应结果：

  ```json
  {
    "success": true
  }
  ```
