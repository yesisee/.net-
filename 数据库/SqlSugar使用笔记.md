### 1.SqlSugar跨服务器跨库联表查询
`在主数据库中创建链接服务器`

![image](https://github.com/user-attachments/assets/e52d145a-6e15-484b-9b85-57335eedbbde)
![image](https://github.com/user-attachments/assets/e3e9230c-8622-44be-9037-1200f12a0282)
![image](https://github.com/user-attachments/assets/9c5065ff-b66d-4bfa-b23c-fc6a90f157f4)

`SqlSugar中跨服务器跨库的联表查询方式`
```C#
//对象UserWorkDetail和SysUser不属于同一个数据库
//先将上下文通过AsTentant()改为租户模式
//再根据QueryableWithAttr根据数据接口的特性获取数据库的连接配置
var query = Context.AsTenant().QueryableWithAttr<UserWorkDetail>()
  .LeftJoin<SysUser>((userWorkDetail, user) => userWorkDetail.UserId == user.UserId, "[JSKJ_SSO].[JSKJ_SSO].dbo.[sys_user]")
  .Where((userWorkDetail, user) => deptIds.Contains(user.DeptId)
                           && userWorkDetail.WorkStatus == WorkStatus.Working
                           && user.Status == 0)
  .Select(userWorkDetail => userWorkDetail.UserId)
  .Distinct().ToSqlString();
```
