# Overview

| 限定符和类型 | 方法和说明 |
|--------------|------------|
| `static int` | `delete(String tableName, QueryWrapper wrapper)` 根据包装器删除数据 |
| `static <T> int` | `delete(T entity)` 根据主键删除记录 |
| `static <T> int` | `delete(String tableName, T entity)` 根据实体对象删除记录 |
| `static JSONObject` | `getById(String tableName, JSONObject params)` 根据查询条件获取实体列表。 |
| `static <T> T` | `getById(String tableName, T entity)` 根据实体对象获取一条数据|
| `static <T> T` | `getOne(QueryWrapper wrapper, Class<T> clazz)` 根据查询条件获取一条数据，并将其转换为指定的Java对象类型。 |
| `static <T> T` | `getOne(T entity)` 查询单个对象，根据实体对象中的属性作为查询条件。 |
| `static long` | `insert(String tableName, JSONObject params)` 根据查询条件获取实体列表。 |
| `static <T> List<T>` | `list(QueryWrapper wrapper, Class<T> clazz)` 根据查询条件获取实体列表。 |
| `static <T> List<T>` | `list(T entity)` 查询列表，根据实体对象中的属性作为查询条件。 |
| `static <T> <any>` | `page(<any> page, QueryWrapper wrapper, Class<T> clazz)` 分页查询方法 |
| `static <any>` | `page(JSONObject params, QueryWrapper wrapper)` 分页查询方法 |
| `static <T> <any>` | `page(T entity)` 分页查询，根据实体对象中的属性作为查询条件。 |
| `static long` | `saveOrUpdate(JSONObject params)` 保存或更新数据。 |
| `static long` | `saveOrUpdate(String tableName, JSONObject params, QueryWrapper wrapper)` 根据查询条件获取实体列表。 |
| `static <T> long` | `saveOrUpdate(String tableName, T entity)` 保存或更新 |
| `static int` | `update(String tableName, JSONObject params, QueryWrapper wrapper)` 根据查询条件更新数据 |
| `static int` | `update(String tableName, JSONObject params, QueryWrapper wrapper, Boolean ignoreNull)` 根据查询条件更新数据 |
| `static int` | `updateById(String tableName, JSONObject params)` 根据查询条件获取实体列表。 |

ps. 上表是`java doc` 生成的又转的`markdown`,难免有遗漏的地方，所以仅供参考，以源码为准

# 数据库环境

- 155:3307
- gdjt_cost_data
- 所涉及到的表：
    - `quickstart_dept` 组织架构表
    - `quickstart_user_dept` 用户-组织架构关联表
    - `quickstart_user` 用户表
