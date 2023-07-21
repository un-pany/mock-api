# Easy Mock

地址：https://github.com/easy-mock/easy-mock

# 登录模块

## 获取验证码

```js
{
  code: 0,
  data: "@image(100x40, #dcdfe6, #000000, png, V3Admin)",
  message: "获取验证码成功"
}
```

## 登录

```js
{
  code: function({
    _req
  }) {
    return ['admin', 'editor'].includes(_req.body.username) ? 0 : 1
  },
  data: {
    token: function({
      _req
    }) {
      return ['admin', 'editor'].includes(_req.body.username) ? "token-" + _req.body.username : ""
    }
  },
  message: function({
    _req
  }) {
    return ['admin', 'editor'].includes(_req.body.username) ? "登录成功" : "账号不存在"
  }
}
```

## 获取用户详情

```js
{
  code: 0,
  data: {
    username: function({
      _req
    }) {
      return _req.header.authorization.split('-')[1]
    },
    roles: function({
      _req
    }) {
      return [_req.header.authorization.split('-')[1]]
    }
  },
  message: "获取用户详情成功",
  _res: {
    status: function({
      _req
    }) {
      return ["Bearer token-admin", "Bearer token-editor"].includes(_req.header.authorization) ? 200 : 401
    },
    data: {
      code: 1,
      data: {},
      message: "无效 Token"
    }
  }
}
```

## 表格模块

## 增

```js
{
  code: 0,
  data: {},
  message: "新增成功"
}
```

## 删

```js
{
  code: 0,
  data: {},
  message: "删除成功"
}
```

## 改

```js
{
  code: 0,
  data: {},
  message: "修改成功"
}
```

## 查

```js
{
  code: 0,
  data: {
    total: 100,
    list: function({
      _req,
      Mock
    }) {
      const res = []
      const username = _req.query.username
      const phone = _req.query.phone
      if (username || phone) {
        res.push(
          Mock.mock({
            id: "@id",
            username: username ? username : "@name",
            phone: phone ? phone : /^1[3-9]\d{9}$/,
            email: "@email",
            "roles|1": ["admin", "editor"],
            status: "@boolean",
            createTime: "@datetime"
          })
        )
      } else {
        const length = Math.min(_req.query.size, 100)
        for (let i = 0; i < length; i++) {
          res.push(
            Mock.mock({
              id: "@id",
              username: "@name",
              phone: /^1[3-9]\d{9}$/,
              email: "@email",
              "roles|1": ["admin", "editor"],
              status: "@boolean",
              createTime: "@datetime"
            })
          )
        }
      }
      return res
    }
  },
  message: "获取表格数据成功",
  _res: {
    status: function({
      _req
    }) {
      return ["Bearer token-admin", "Bearer token-editor"].includes(_req.header.authorization) ? 200 : 401
    },
    data: {
      code: 1,
      data: {},
      message: "无效 Token"
    }
  }
}
```
