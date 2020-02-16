#Concept
1.Virtual User。 Gatling可以处理请求之间的逻辑，并且可以处理虚拟用户，每个虚拟用户可以拥有自己的数据也可以使用不同的浏览器路径。
2.Scenario。有一些工具将虚拟用户视作线程，但是Gatling将用户视作消息，因此很容易模拟多个虚拟用户的测试场景。
3.测试人员通过定义scenarios来模拟虚拟用户的操作，一个标准的电子商务的程序的scenario包括：
    Access home page
    Select a browse category
    Make a search in this category
    Open a product description
    Go back
    Open another product description
    Buy product
    Log in
    Checkout
    Payment
    Log out
4. scenario("Standard User")
   exec(http("Access Github").get("https://github.com"))
   pause(2, 3)
   exec(http("Search for 'gatling'").get("https://github.com/search?q=gatling"))
   pause(2)
   上面这个scenario包括两个HTTP请求和两个暂停等待时间，等待的时间是在模拟用户思考阅读当前页面的时间。
5.simulation描述了当前测试将如何进行，会运行什么scenario，以及如何运行，例如
    val stdUser = scenario("Standard User") // etc..
    val admUser = scenario("Admin User") // etc..
    val advUser = scenario("Advanced User") // etc..

    setUp(
    stdUser.inject(atOnceUsers(2000)),
    admUser.inject(nothingFor(60 seconds), rampUsers(5) during (400 seconds)),
    advUser.inject(rampUsers(500) during (200 seconds))
    )
6.Seesion. 每个虚拟用户都有一个会话支持。 这些会话是场景工作流中的实际消息。 会话基本上是状态占位符，测试人员可以在其中插入或捕获并存储数据。

7.Feeders. 供测试人员使用的便捷API，可以将来自外部源的数据注入虚拟用户的会话中。

8.Checks. 响应处理器，可以捕获其一部分并验证其是否满足某些给定条件。 例如，在发送HTTP请求时，您可能期望HTTP重定向。 通过检查，您可以验证响应的状态实际上是30倍代码。

检查还可以用于捕获某些元素并将其存储到Session中，以便以后可以重用它们，例如构建下一个请求。

9.Assertions. 断言用于定义Gatling统计信息的接受标准（例如，响应时间的99％），这会使Gatling失败并为整个测试返回错误状态代码。

10.Reports. 默认情况下，报告在模拟结束时自动生成。 它们由HTML文件组成。 因此，它们是便携式的，可以在任何使用Web浏览器的设备上进行查看。

#Simulation structure
一个simulation主要包含一下四部分；
    The HTTP protocol configuration
    The headers definition
    The scenario definition
    The simulation definition

 The HTTP protocol configuration：baseUrl, common headers
 The scenario definition主要包含两部分，exec和pause。exec用来描述一个行为，通常是指对被测程序的行为。pause描述等待时间。
  The simulation definition。定义要注入到服务器的负载，例如：
  setUp(
  scn.inject(atOnceUsers(1)) // (1)
    .protocols(httpProtocol) // (2)
)
--我们将一个用户注入scn场景
--我们在setUp上配置httpProtocol，以便传递基本URL和通用标头。