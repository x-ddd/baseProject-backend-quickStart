# CRUD

> 示例代码在`io/bm/modules/example`目录下 </p>
> 叠甲环节：由于编者水平有限balabala…… 这里的`demo`尽可能覆盖了一些常见的写法，未尽之处及不足之处欢迎友友们反馈捏😊

## 分页

分页接口`controller`层如下：
```java,editable


	/**
	 * 查询用户分页列表
	 * value 接口名称
	 * module 接口所属模块
	 * recordParams 是否打印请求参数
	 * recordResult 是否打印返回结果
	 * recordException 是否打印异常信息
	 * @param param 分页相关参数
	 * @return 分页用户列表
	 */
	@BmLog(value = "查询用户分页列表",module = "测试业务模块",recordParams = true,recordResult = true,recordException = true)
	@PostMapping(value = "/queryUserList", name = "查询用户分页列表")
	public Object queryUserList(@RequestBody JSONObject param) {
		Page<JSONObject> page = userService.queryUserList(param);
		return page;
	}


```
`controller`层没啥可说的，不过还是让我们来看一下`BmLog`吧

日志系统`BmLog`是通过注解实现的，只需要在对应的接口上面添加`BmLog`注解即可，前两个参数分别为接口名称和对应的模块。
后面三个参数可省略，默认为`true`

让我们关注一下`service`层，这里才是分页接口需要注意的地方

```java,editable


	public Page<JSONObject> queryUserList(JSONObject param) {
		// 用户名
		String sname = param.getString("sname");
		//排序方式 desc asc
		String sort = param.getString("sort");
        //当前页码
        long pageNumber = param.getLong("pageNumber");
        //分页最大数量
		long pageSize = param.getLong("pageSize");
		if (BeanUtil.isEmpty(pageNumber)){
			GracefulResponse.raiseException("500", "pageNumber不能为空");
		}
		if (BeanUtil.isEmpty(pageSize)){
			GracefulResponse.raiseException("500", "pageSize不能为空");
		}

		QueryWrapper queryWrapper = QueryWrapper.create().from("quickstart_user").as("user")
				// 不应该使用 * 通配符，而是明确列出需要的字段
				.select("dept.dept_name","dept.belong_id", "user.sname","user.user_id")
				.leftJoin("quickstart_user_dept").as("user_dept").on("user.user_id = user_dept.user_id")
				.leftJoin("quickstart_dept").as("dept").on("dept.dept_id = user_dept.dept_id")
				.where(wrapper -> {
					wrapper.eq("user.is_flag", DictValue.IF_YES);
					if (StrUtil.isNotBlank(sname)){
						wrapper.like("user.sname",sname);
					}
					if (BeanUtil.isNotEmpty(sort)){
						log.info("in sort");
						if (sort.equals("desc")){
							// 这里第二个参数指的是是否ASC
							wrapper.orderBy("user.update_date",Boolean.FALSE);
						}
						if (sort.equals("asc")){
							wrapper.orderBy("user.update_date",Boolean.TRUE);
						}
					}
				});

		JSONObject pageParam = new JSONObject();
		pageParam.put("pageNumber",pageNumber);
		pageParam.put("pageSize",pageSize);

		Page page = null;
		try {
			// 这里第一个参数用来构建 Page 对象，第二个参数是查询条件，第三个参数是返回结果的类型
			page = QueryUtil.page(pageParam.to(Page.class), queryWrapper, JSONObject.class);
		} catch (SQLException e) {
			GracefulResponse.raiseException("500", "查询失败");
		}
		return page;
	}

```
`page`有以下三个重载的方法，其中第一个通过实体类的分页方式是**不能用的**，
并且查阅源码来看，第三个方法实际上就是帮你把`params`构建为`Page`对象后再调用的第二个方法，因此我们的示例代码中使用的是第二个方法


```java

<T> Page<T> page(T entity) 

<T> Page<T> page(Page<T> page, QueryWrapper wrapper, Class<T> clazz)

Page<JSONObject> page(JSONObject params, QueryWrapper wrapper)

```

还需要注意的是：
第三个方法中的`params`参数是用来构建`Page`对象，而分页条件是通过`wrapper`参数来构建的


又及，`queryWrapper`的`select`子句中不建议把所有的字段都传给前端，应该只传前端需要的字段

## 查

`get`簇分为三类，`getById`、`getOne`以及`list`，前两个返回一个实体类对象，`list`返回一个实体列表
并且这些簇有很多重载的方法，大家可以依据业务需要选择不同的方法

这里以`getById`为例：

```java

	/**
	 * 获取用户信息
	 *
	 * @param param 用户ID
	 * @return 用户信息
	 */
	@PostMapping(value = "/queryUserInfo", name = "根据id获取用户信息")
	public JSONObject queryUserInfo(@RequestBody JSONObject param) {
		JSONObject userInfo = null;
		if (BeanUtil.isNotEmpty(param.getLong("userId"))) {
			userInfo = userService.queryUserInfo(param);
		}else {
			GracefulResponse.raiseException("500", "缺少必传参数：userId");
		}
		return userInfo;
	}

```
我们在`controller`层构建了查询参数`param`并提供给`userService.queryUserInfo()`，而`queryUserInfo()`会调
用`QueryUtil.getById()`构筑对应的`sql`语句并执行，和别的`get`簇的方法不同的是，它会返回一个`JSONObject`对象。

当然，你并不用担心`param`中会有多余的参数，`QueryUtil.getById()`会自动移除那些不在表中的字段。

`userService.queryUserInfo()`如下:

```java
    /**
	 * 根据id获取用户信息
	 * @param params 用户id
	 * @return 用户信息
	 * @throws SQLException
	 */
	public JSONObject queryUserInfo(JSONObject params) throws SQLException {
		JSONObject userInfo = QueryUtil.getById("quickstart_user", params);
		return userInfo;
	}
```


## 增




## 删 


## 改

