# Dok Design

## 设计目标

从 DSL 中生成面向 markdown / html / pdf 的 API 文档

- 将 DSL 翻译成 中间结构体(IS, Intermediate Structure)
- 将 IS 翻译成不同平台的可读文档格式
- 支持从多种语言的注释中提取 DSL 并生成 API 文档

### DSL 样例

#### base.dok

```
DEF BASE
DOMAIN: xxx.com
HEADERS:
	jwt-token: string # token
COOKIES:
	sess: string # cookie
ENDDEF
---
```

#### user.dok

```
INCLUDE: base.dok
EXTENDS:
	- BASE
URL: /1.0/users/list
DESCRIBTION: describtion
METHOD: POST
QUERY:
	skip: int ? =0 #description
	limit: int
BODY:
	skip: int ?
	limit: int
OK_RES:
	STATUS: 200
	BODY:
		- users:
			username: string
			id: string
			role: string
ERR_RES:
	RES_STATUS: 400
	RES_BODY: JSON
		code: number
		message: string
TEST:
	QUERY: skip=0,limit=10
	BODY: skip=0,limit=10
---
```

### 代码注释导出

以 Python 为例

```python
def list_users(skip, limit):
	"""
	...DSL
	"""
	pass
```

每一个块注释代表一个 section

### Render

---

### GET /1.0/users/list

describtion

#### Query Params

| Name| Type| Optional | Description|
|---|---|---|---|
| `skip` | `int` | `false` | description |
| `limit` | `int` | `true` | |


#### Data Params

| Name| Type| Optional | Description|
|---|---|---|---|
| `skip` | `int` | `false` | description |
| `limit` | `int` | `true` | |

#### Success Response

##### Status Code

200

##### Body

```json
{
	"users": [{
		"username": "string",
		"id": "string",
		"role": "string"
	}]
}
```

#### Error Response

##### Status Code

400

##### Body

```json
{
	"code": "number",
	"message": "string",
}
```

---