# 权限

## 你们的权限是怎么做的，如何精确到按钮级别，说一下实现思路。(可以选择提问:一张表能不能做权限，两张表能不能做?不用表能不能做)
我们当时用了5张表:user表 role表 power表 user_role表  role_power表 ,用户登录时,我们根据账户密码查询看是否合法,合法的话我们用了userId 去user_role表里查询了该用户的所有的roleId, 拿到roleId 的集合后,在将这个roleId的集合作为条件去role_power里面去查询所有的数据,在根据本表里的status查看该数据是否被限制,拿到所有满足条件,并且不被限制的数据的powerId字段作为一个新的集合,再将该集合作为条件去power表里去查询拿到所有的权限. 我们的power有object_power的字段存的是对哪个对象可以进行什么操作,将所有的数据作为一个集合,将用户的Id作为Key,值集作为value存放到redis中.我们自定义的拦截器,拦截了所有的请求,看不是不登录,如果是登录就直接通过,不是的话,用ID作为条件去redis中查找,找到说明已经登录,没有则打回登录页面进行登录,登录的话再看请求的路径在value的集合当中有吗,有这说明该用户有这个权限. 就可以对相应的资源进行相应的操作,没有这不让通过.

我们当时用了5张表:user表 role表 power表 user_role表  role_power表 ,用户登录时,我们根据账户密码查询是否合法,合法的话我们用userId 去user_role表里查询了该用户的所有roleId, 拿到roleId 的集合后,作为条件去role_power里面去查询所有的数据,在根据status查看该数据是否被限制,拿到不被限制的powerId字段,作为一个新的集合,去power表里查询拿到所有的权限. 我们的power表有object_power的字段存的是对哪个对象可以进行什么操作,

将所有的数据作为一个集合,将用户的Id作为Key,值集 作为value存放到redis中.我们自定义的拦截器,拦截了所有的请求,然后验证是否登录,如果是登录就直接通过,不是的话,用ID作为条件去redis中查找,找到说明已经登录,没有则打回登录页面进行登录,登录的话再看请求的路径在value的集合当中有吗,有这说明该用户有这个权限. 就可以对相应的资源进行相应的操作,没有这不让通过.

---
管理权限设置这一块，也是我单独负责的，用了大概六张表左右，这个是控制到按钮的操作。四张信息表，这个用户表,角色表,权限表，还有一张是按钮的那张表，有两个关联信息表，用户角色的关联表，还有角色权限的信息表,后台任何一个用户登录之后，会去用户表里边先确认自己的用户信息是否合法。如果合法的话，我们会去用户与角色的中间表里面查这个当中具有的所有角色ID集合，然后查到集合之后，呢我们会拿这个集合去角色与权限这个中间表里,查到当前用户所具有的所有权限的ID这个角色集合，查到这个集合后,把这个角色统一返回，因为在我们的角色权信息表里边有个字段,它这个字段，算是按钮的一个ids。当用户在查到他所有权限集合的时候，同样也能查到他所具有的所有按钮id集合，然后我们会把这些信息呢全部返回到页面去。

权限展示这块呢,我们是用了layui的树形菜单来展示的,按钮这块的话，我们是直接使用了EL标签来控制的。而且我们当时还做了一个拦截器的功能，就是说我们在每次登录的时候会首先来拦截这个url请求。用户的每个请求都会被这个拦截器拦截,如果当前请求是该用户不具备权限的请求，我们会在后台拦截当前用户，然后直接把url给打到登录页面去。

`普通5表权限：`

当时我们做权限用了6张表，有用户表，角色表，用户角色关联表，权限树表，角色权限关联表，详细菜单表，当用户登陆时我们通过获取session里的用户id，通过5表关联查询出左边的导航树，然后我们配置一个拦截器，当用户发送一个请求时，我们在拦截器里通过request.getRequestURI()获取到用户请求的路径，然后通过用户的id查询出用户所拥有的权限路径列表，判断当前请求地址是否在所用有访问权限的请求列表之内，如果存在则返回true方法这个请求，否则跳转到没有权限的提示页面中。几张表都能做权限，一张两张三张都可以，不用表也可以，用txt或者任何可持久化的能保存数据的东西
