# 拦截器

## Springmvc 的拦截器配置步骤？
1. 首先在spring-mvc.xml文件中添加拦截器配置

```xml
<!-- 拦截器 -->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**" />
        <!-- 指向自定义拦截器类 -->
        <bean class="com.jk.interceptors.SecurityInterceptor">
            <!-- 不需要权限验证的地址 -->
            <property name="excludeUrls">
                <list>
                    <value>/loginController/verifyCheck</value><!-- 验证码校验 -->
                </list>
            </property>
        </bean>
    </mvc:interceptor>
</mvc:interceptors>
```

2. 创建自定义拦截器类实现HandlerInterceptor接口重写preHandle方法。该方法会在Controller的方法执行前会被调用，可以使用这个方法来中断或者继续执行链的处理。当返回true时，处理执行链会继续，当返回false时，则不会去执行Controller的方法。
```java 

public class SecurityInterceptor implements HandlerInterceptor {
    //日志记录
    private static final Logger logger = Logger.getLogger(SecurityInterceptor.class);
    private List<String> excludeUrls;       // 不需要拦截的资源
    public List<String> getExcludeUrls() {
        return excludeUrls;
    }
    public void setExcludeUrls(List<String> excludeUrls) {
        this.excludeUrls = excludeUrls;
    }
    /**
    * 完成页面的render后调用
    */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object object, Exception exception) throws Exception {
    }
    
    /**
    * 在调用controller具体方法后拦截
    */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object object, ModelAndView modelAndView) throws 	        Exception {
    }
    
    /**
    * 在调用controller具体方法前拦截
    */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object object) throws Exception {
        String requestUri = request.getRequestURI();
        String contextPath = request.getContextPath();
        String servletPath = requestUri.substring(contextPath.length());
        String url = StringUtils.substringBeforeLast(servletPath, ".");
        // logger.info(url);
        if (url.indexOf("/baseController/") > -1 || excludeUrls.contains(url)) {// 如果要访问的资源是不需要验证的
        return true;
        }
        Account account = (Account) request.getSession().getAttribute("account");
        if (account == null || account.getId().equalsIgnoreCase("")) {          // 如果没有登录或登录超时
            request.setAttribute("msg", "您还没有登录或登录已超时，请重新登录，然后再刷新本功能！");
            request.getRequestDispatcher("/error/noSession.jsp").forward(request, response);
            return false;
        }
    
    //		if (!user.getResources().contains(url)) {               // 如果当前用户没有访问此资源的权限
    //			request.setAttribute("msg", "您没有访问此资源的权限！<br/>请联系超管赋予您<br/>[" + url + "]<br/>的资源访问权限！");
    //			request.getRequestDispatcher("/error/noSecurity.jsp").forward(request, response);
    //			return false;
    //		}
    return true;
    }
}
```
