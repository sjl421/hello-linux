## Nginx-location 配置规则
- Nginx 几乎是当下绝大多数公司在用的 web 应用服务，熟悉 Nginx 的配置，对于我们日常的运维工作是至关重要的，下面就 Nginx 的 location 配置进行梳理：

### location 匹配的是变量
- 我们知道 nginx 的 location 段匹配的是某个变量，具体如下：
  ```bash
  $request_uri
  ```

### location 匹配的种类
- 格式：location [ 空格 | = | ~ | ~* | !~ | !~* ｜ @ ] /uri/ {}
- **规则解释：**
- `=`  表示精确匹配，如果找到，立即停止搜索并立即处理此请求
- `~`  表示执行一个正则匹配，区分大小写匹配
- `~*` 表示执行一个正则匹配，不区分大小写匹配
- `!~` 区分大小写不匹配
- `!~*`不区分大小写不匹配
- `^~` 即表示只匹配普通字符（空格）。使用前缀匹配，^表示“非”，即不查询正则表达式。如果匹配成功，则不再匹配其他location
- `@`  指定一个命名的 location，一般只用于内部重定向请求。例如 error_page, try_files
- `/`  表示通用匹配，任何请求都会匹配到
  
### location搜索优先级顺序
- 精确匹配 > 字符串匹配( 长 > 短 [ 注: ^~ 匹配则停止匹配 ]) > 正则匹配( 上 > 下 )
- 在 nginx 的 location 和配置中 location 的顺序没有太大关系。正 location 表达式的类型有关。相同类型的表达式，字符串长的会优先匹配。
- **优先级排列：**
- 1）等号类型（=）的精确匹配优先级最高，精确匹配只能命中一个。一旦匹配成功，则不再查找其他匹配项
- 2）`^~` 类型表达式，即字符串匹配，使用匹配最长的最为匹配结果。一旦匹配成功，则不再查找其他匹配项
- 3）正则表达式类型`（~ ~*）`的优先级次之。如果有多个 location 的正则能匹配的话，则使用正则表达式最长的那个
- 4）常规字符串匹配类型。按前缀匹配
    
