# 如何解决Mybatis中数据库字段名和实体类属性名不一致

## 驼峰转换
实体类属性名为驼峰式命名法，而数据库字段名为全小写用_间隔<br>
例如 LastName ===> last_name
```
<setting name="mapUnderscoreToCamelCase" value="true"/>
```

## 别名
```
<select id="getDeptByIdStep" resultMap="MyDeptStep" >
	select id,dept_name departmentName from tbl_dept where id=#{id}
</select>
```

## ResultMap
使用自定义ResultMap，column指定数据库字段名，property指定实体类属性名
```
<resultMap type="com.guigu.mybatis.bean.Employee" id="MySimpleEmp">
	<id column="id" property="id"/>
	<result column="last_name" property="lastName"/>
	<result column="email" property="email"/>
	<result column="gender" property="gender"/>
</resultMap>
	
<select id="getEmpById"  resultMap="MySimpleEmp">
	select * from tbl_employee where id=#{id}
</select>
```
