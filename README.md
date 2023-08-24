# 蓝奏云解析API
蓝奏云解析，支持获取：文件名、分享者、文件大小、描述、文件链接、文件直链

获取到的链接是临时的下载链接，请按需使用。

要求PHP版本>=7.0

# 功能
1. 获取文件名、分享者、文件大小、文件描述、文件链接、文件直链、上传时间
2. 支持带密码文件和带`webpage`参数的文件
3. 支持手机UA和电脑UA获取文件，并且可以进行自动切换
4. 支持缓存链接和验证缓存的链接是否有效
5. 支持多种数据格式响应

# 使用

## 请求
请求类型：`GET/POST`

请求示例：`lanzouyunapi.php?data=wzdc`
| 参数 | 必填 | 默认 | 说明 |
| -- | -- | -- | -- |
| mode | 否 | pc | 获取数据方式（可选值：pc、mobile） |
| data | 是 | | 蓝奏云链接或ID |
| pw | 否 | | 密码 |
| types | 否 | json | 返回的数据类型(可选值: text、json、xml) |
| redirect | 否 | 0 | 重定向 |
| auto | 否 | 1 | 自动切换 |
| page | 否 | 1 | 页数 |

### 功能说明


| 参数 | 说明 |
| -- | -- |
| 1  | 开启 |
| 0  | 关闭 |

#### 重定向

开启重定向(redirect)后，服务器会将获取到的链接进行重定向。如果获取链接失败则会返回错误信息，返回的数据类型由你（传入的types参数）决定。

开启重定向：https://api.wzdc.tk/lanzouyunapi?data=wzdc&redirect=1

#### 自动切换

例如当使用电脑UA获取链接失败就使用手机UA去获取（先用哪个由你传入的`mode`参数决定）。

自动切换默认开启，关闭：https://api.wzdc.tk/lanzouyunapi?data=wzdc&auto=0

## 返回

| 参数 | 说明 | 数据类型 |
| -- | -- | -- |
| code | 状态码 | int |
| msg | 信息 | String |
| data | 文件信息 |  |

### 文件信息
| 参数 | 说明 | 数据类型 |
| -- | -- | -- |
| name | 文件名 | String |
| size | 文件大小 | String |
| user | 分享者 | String |
| desc | 描述 | String |
| time | 上传时间 | String |
| url | 链接 | String |

### 文件夹信息
| 参数 | 说明 | 数据类型 |
| -- | -- | -- |
| name | 文件夹名称 | string |
| desc | 文件夹描述 | string |
| folder | 子文件夹列表 |  |
| list | 文件列表 | |
| have_page | 有下一页 | Boolean |

### 状态码说明
| code | msg | 说明 |
| -- | -- | -- |
| 5  | （显示蓝奏云返回的错误信息） | 此链接是文件夹，请求的参数来自缓存，蓝奏云接口返回了错误信息 |
| 4  | 文件夹 | 此链接是文件夹，请求的参数来自缓存 |
| 3  | 文件夹 | 此链接是文件夹 |
| 2  | 来自缓存 | (部分或全部)数据来自缓存 |
| 1  | 获取直链失败 | 已经成功获取链接，但获取直链失败 |
| 0  | 成功 | 获取成功 |
| -1 | （显示蓝奏云返回的错误信息） | 请求接口时，蓝奏云接口返回的错误信息 |
| -2 | （显示获取到的错误信息，如果没有则显示获取失败） | 获取网页内容时发生错误，会尝试再次获取蓝奏云提示的错误信息 |
| -3 | 获取失败 | HTML解析错误 |
| -4 | 缺少参数 | 缺少参数 |
| -5 | （显示蓝奏云返回的错误信息） | 此链接是文件夹，蓝奏云接口返回了错误信息 |

# 其他说明
1. 文件夹查看多页内容需要等待一定时间才能查看更多页内容（官方限制）。对于页数较多的文件夹，缓存可以解决等待时间长的问题。
2. 建议将计算机时区设置为UTC+8。
3. 遇到无法解析请附带无法解析的文件分享链接[提交issues](https://github.com/wzdc/lanzouyunapi/issues]
4. 文件夹不支持`type=text`
