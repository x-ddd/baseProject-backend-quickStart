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

![plume log](/image/plume.png)