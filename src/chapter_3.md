# BmLog

`BmLog` 是一个用于记录接口调用信息的注解，主要用于Controller层的方法，可以自动记录接口的路径、类型、参数、响应等信息。

## BmLog参数列表:

| 参数 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| value | String | "" | 日志描述，通常是接口功能描述 |
| module | String | "" | 业务模块名称 |
| recordParams | boolean | true | 是否记录请求参数 |
| recordResult | boolean | true | 是否记录响应结果 |
| recordException | boolean | true | 是否记录异常信息 |

## Example

```java, editable

	/**
	 * 查询用户分页列表
	 * value 接口名称
	 * module 接口所属模块
	 * recordParams 是否打印请求参数
	 * recordResult 是否打印返回结果
	 * recordException 是否打印异常信息
	 * @param param 分页相关参数
	 * @return
	 */
	@BmLog(value = "查询用户分页列表",module = "测试业务模块",recordParams = true,recordResult = true,recordException = true)
	@PostMapping(value = "/queryUserList", name = "查询用户分页列表")
	public Object queryUserList(@RequestBody JSONObject param) throws SQLException {
		Page<JSONObject> page = userService.queryUserList(param);
		return page;
	}
```

## PlumeLog

![plume log](https://pic1.imgdb.cn/item/67d93aa088c538a9b5c038e0.png)

## 返回参数示例

```
2025-03-18 09:37:14.550 [INFO ] io.bm.framework.log.BmLogAop: 接口日志: 
{
  "duration": "19ms",
  "requestUrl": "/sysMenu/selectModifyCheck",
  "success": false,
  "module": "菜单模块",
  "requestMethod": "POST",
  "errorMessage": "该菜单已配置完成，如把菜单类型改为目录，则会把配置完成的数据删除，请谨慎操作！",
  "description": "查询目录改菜单功能",
  "methodName": "selectModifyCheck",
  "requestIp": "172.16.0.162",
  "className": "io.bm.modules.sys.controller.SysMenuController",
  "params": {
    "param": {
      "menuId": "1850727595533725696",
      "systemId": "1207142670867730434"
    }
  }
}
```