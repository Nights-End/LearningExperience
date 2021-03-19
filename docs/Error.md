# .NET MVC全局异常处理

一直知道有.NET有相关的配置，但没有实际做过，以为改下设定就可以，结果实际使用的时候还是遇到不少问题，所以要记录一下。

## IIS配置

刚开始不想改程序代码，所以直接就想到了IIS里面的错误页配置配置，一开始反复测试，设置改了很多，但是没有效果，后来发现是静态页的配置，还没有进入MVC的程序部分，所以对于.NET MVC这种动态页是不生效的,应该使用.NET错误页选项

### 静态错误页配置

静态页配置流程如下：

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gaf6v3j21400l3wi5.jpg)
![](http://ww1.sinaimg.cn/large/aa003451gy1g161gae3ywj21400l30uq.jpg)
![](http://ww1.sinaimg.cn/large/aa003451gy1g161gaet35j21400l30v0.jpg)

如上图所示，IIS中配置对错误的响应有三种方式，默认情况是第三个，本地访问显示详细错误信息，外部地址访问显示自定义页面，这样方便开发者调试，如果没有设置专门的错误页会使用IIS自带的样式，也就是第二张图中的配置，根据路径我们可以找到这样一个文件夹，里面都是错误提示的静态页，对应不同的状态代码

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gaepmlj20hm0igdhy.jpg)

我们可以把IIS设置为均使用自定义错误页看下效果，或者直接通过文件访问

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gaemc0j210u0jy75p.jpg)
![](http://ww1.sinaimg.cn/large/aa003451gy1g161gad8gcj20gk0700t3.jpg)

上面那张是详细的静态404错误，可以看到会暴露我们系统路径，下面则是默认的自定义错误页

静态错误的默认页有相应的设置，看似可以修改，有“文件”、“执行URL”、“重定向”三种，但是实际设置一下就会发现报错：锁定错误

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gagnoyj20d30dgdga.jpg)

通过这个错误我们去搜索解决方法可以看到一些人说将web.config中的httperror节下的defaultPath解锁即可，但似乎这是IIS7以前的设置，在IIS10中并没有相应的选项，看到一些说明提到可能是官方使用了更加安全的管理机制，因为发现这边的配置是静态页相关，不符合我的需要，没有深入研究，如果一定要使用这种可以看看这篇博客，试试能否通过系统命令解决锁定的问题

[win7 IIS Web.config节点锁定问题](https://blog.csdn.net/oyiboy/article/details/40918511)

### .NET错误页配置

.NET错误页的设置与静态页差不多，除了入口不一样，配置的选项也不太相同，但是整体意思一样

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gakl2uj213l0ksadm.jpg)
![](http://ww1.sinaimg.cn/large/aa003451gy1g161gaks22j21400l375x.jpg)

可以看到这里要求是绝对URL，所以实际使用起来应该是不太方便，所以没有找到太多相关资料。另外，需要web.conig中的customError设为On，部分异常如500会自动跳转到MVC的默认错误页Home/Error

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gajwsdj205x09rt8p.jpg)

使用IIS的错误页处理虽然不用改代码，但是维护起来局限性很多，最终还是应该通过程序进行全局异常捕获

## 程序设置

通过程序控制的方法我想到两种，一个是使用全局配置文件Global.asax中的Application_Error方法，另一个是使用MVC的过滤器，默认的四种过滤器中就包含异常过滤

### 全局异常配置Global.asax

这种方法对于WebForm和MVC都是通用的，在ASP.NET中，只要网站程序抛出未捕获的异常都会触发Application_Error事件。

使用此方法一定要把GlobalFilter全局过滤器中的HandleErrorAttribute注册取消掉，也可以将配置文件中的customErrors节点关闭，否则HTTP 500的错误将不会被Application_Error事件捕获。

![](http://ww1.sinaimg.cn/large/aa003451gy1g161gak7qvj20fm0423yb.jpg)
![](http://ww1.sinaimg.cn/large/aa003451gy1g161gakzpgj20br033mwy.jpg)

捕获到异常之后我们可以很容易地跳转到静态页面

```csharp
protected void Application_Error(object sender, EventArgs e)
{
    Exception exception = Server.GetLastError();
    var httpStatusCode = (exception as HttpException)?.GetHttpCode() ?? 700; //如果为空则走自定义
    var httpContext = ((MvcApplication)sender).Context;
    httpContext.ClearError();

    switch (httpStatusCode)
    {
        case 404:
            httpContext.Response.Redirect("~/Error/404.htm");
            break;
        default:
            httpContext.Response.Redirect("~/Error/500.htm");
            break;
    }
}
```

在一般情况下我们也可以指向一个控制器

```csharp
protected void Application_Error(object sender, EventArgs e)
{
    Exception exception = Server.GetLastError();
    var httpStatusCode = (exception as HttpException)?.GetHttpCode() ?? 700; //如果为空则走自定义
    var httpContext = ((MvcApplication)sender).Context;
    httpContext.ClearError();

    var routeDic = new RouteValueDictionary
    {
        {"controller", "Home"},
        { "action","Error"}
    };
    httpContext.Response.RedirectToRoute("Default", routeDic);
}
```

但是在实际的业务中遇到了一些http请求的问题，在处理一部分代码抛出的异常时会出现“服务器无法在已发送HTTP标头之后······”这一系列异常，如“设置状态”、“追加标头”等，这个时候跳转要使用另一种写法

```csharp
protected void Application_Error(object sender, EventArgs e)
{
    Server.ClearError();
    Response.TrySkipIisCustomErrors = true;
    var routeData = new RouteData();
    IController controller = new HomeController();
    routeData.Values.Add("controller", "Home");
    routeData.Values.Add("action", "Error");
    controller.Execute(new RequestContext(new HttpContextWrapper(Context), routeData));
    Response.End();
}
```

这里要注意的一点是如果要使用Area中的控制器不能写成routeData.Values.Add，而是使用DataTokens

```csharp
routeData.DataTokens.Add("area", "TestArea");
```

### MVC过滤器Filter

MVC有四种过滤器：Authorization、Exception、Action、Result，我们要用的的就是Exception异常过滤器

当我们新建一个MVC项目后，异常过滤器就已经自动在程序中注册了，先从上一节所说的全局配置文件开始，Global.asax这个文件中的Application_Start方法会在程序启动时运行，其中的即默认注册了全局过滤器，如图

![](http://ww1.sinaimg.cn/large/aa003451gy1g1m6lji1ltj20dk056mx0.jpg)

我们可以进入RegisterGlobalFilters方法查看，这个方法中默认注册了一个异常处理过滤器，也就是说默认状态的MVC程序发生异常时会被程序捕获处理，处理方式是跳转至错误页面，也就是上一篇文章说的Layout文件夹下面的Error页

![](http://ww1.sinaimg.cn/large/aa003451gy1g1m6ljg4c6j20fe03z3yb.jpg)

但使用异常过滤器有一个大前提是要在Web.config中打开自定义错误处理的设置,customErrors节点要设置为“On”,这一设置默认是关闭的，也就是说要手动加上才行

```xml
<system.web>
  <compilation debug="true" targetFramework="4.6.1"/>
  <httpRuntime targetFramework="4.6.1"/>
  <customErrors mode="On">
  </customErrors>
</system.web>
```

需要注意的是 **404** 错误，这种类型的异常并不会被过滤器捕获

![](http://ww1.sinaimg.cn/large/aa003451gy1g1m6ljh5ccj20ab04h0sj.jpg)

![](http://ww1.sinaimg.cn/large/aa003451gy1g1m6ljgx7oj20m8034dfu.jpg)

但是可以在web.config中添加节点进行自定义配置，跳转到相应的页面

```xml
<customErrors mode="On">
  <error redirect="~/Error/NotFound" statusCode="404" />
</customErrors>
```

```csharp
public class ErrorController : Controller
{
    // GET: Error
    public ActionResult Index()
    {
        return View();
    }

    public ActionResult CustomHttpError()
    {
        ViewBag.Message = "有错误";
        //HandleErrorInfo
        //ExceptionContext

        return View();
    }
    public ActionResult NotFound()
    {
        return View();
    }
}
```

![](http://ww1.sinaimg.cn/large/aa003451gy1g1m6mv1fx8j20gp07174h.jpg)

#### 自定义过滤器

MVC默认的异常过滤器可以满足基本的需要，但是如果要对一些异常进行特殊处理就需要我们自定义过滤器的内容，可以通过重写OnException方法达到这个目的

```csharp
public class CustomHandleErrorAttribute : HandleErrorAttribute
{
    public override void OnException(ExceptionContext filterContext)
    {
        /* 调用基类的OnException方法，实现基础的功能。
         * 如果要完全的自定义，就不需要调用基类的方法
         */
        base.OnException(filterContext);

        /* 此处可进行记录错误日志，发送错误通知等操作
         * 通过Exception对象和HttpException对象可获取相关异常信息。
         * Exception exception = filterContext.Exception;
         * HttpException httpException = new HttpException(null, exception);
         */
    }
}
```

示例代码

```csharp
public class MyErrorHandler : FilterAttribute, IExceptionFilter
{
    public void OnException(ExceptionContext filterContext)
    {
        if (filterContext.ExceptionHandled || !filterContext.HttpContext.IsCustomErrorEnabled)
            return;

        var statusCode = (int) HttpStatusCode.InternalServerError;
        if (filterContext.Exception is HttpException)
        {
            statusCode = filterContext.Exception.As<HttpException>().GetHttpCode();
        }
        else if (filterContext.Exception is UnauthorizedAccessException)
        {
            //to prevent login prompt in IIS
            // which will appear when returning 401.
            statusCode = (int)HttpStatusCode.Forbidden;
        }
        _logger.Error("Uncaught exception", filterContext.Exception);

        var result = CreateActionResult(filterContext, statusCode);
        filterContext.Result = result;

        // Prepare the response code.
        filterContext.ExceptionHandled = true;
        filterContext.HttpContext.Response.Clear();
        filterContext.HttpContext.Response.StatusCode = statusCode;
        filterContext.HttpContext.Response.TrySkipIisCustomErrors = true;
    }

    protected virtual ActionResult CreateActionResult(ExceptionContext filterContext, int statusCode)
    {
        var ctx = new ControllerContext(filterContext.RequestContext, filterContext.Controller);
        var statusCodeName = ((HttpStatusCode) statusCode).ToString();

        var viewName = SelectFirstView(ctx,
                                       "~/Views/Error/{0}.cshtml".FormatWith(statusCodeName),
                                       "~/Views/Error/General.cshtml",
                                       statusCodeName,
                                       "Error");

        var controllerName = (string) filterContext.RouteData.Values["controller"];
        var actionName = (string) filterContext.RouteData.Values["action"];
        var model = new HandleErrorInfo(filterContext.Exception, controllerName, actionName);
        var result = new ViewResult
                         {
                             ViewName = viewName,
                             ViewData = new ViewDataDictionary<HandleErrorInfo>(model),
                         };
        result.ViewBag.StatusCode = statusCode;
        return result;
    }

    protected string SelectFirstView(ControllerContext ctx, params string[] viewNames)
    {
        return viewNames.First(view => ViewExists(ctx, view));
    }

    protected bool ViewExists(ControllerContext ctx, string name)
    {
        var result = ViewEngines.Engines.FindView(ctx, name, null);
        return result.View != null;
    }
}
```
