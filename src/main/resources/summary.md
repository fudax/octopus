本菜去年比较幸运地进了一个算是比较好的公司，性价比略高，不怎么加班，办公环境、人事行政服务都不错，唯独这项目团队成员真心让人难忍。

- ***反对文化*** 有那么两个技术半吊子，特别爱甩锅，选择性遗忘症严重，专门开会讨论好的方案，后面实施（尤其是有人忘了逻辑或者新人加入不理解然后出现问题）的时候只要有一点问题就喷。比如：你这个逻辑做得不对呀，为什么要搞这么复杂，谁让你这么搞的……我就回他：第一这个逻辑没问题，第二这是我们当初一起开会定的方案……马上否认：啥时候开的会，我都不知道……旁边的人说的确是一起开会定的，马上转话：我们不讨论这个了，回头看看怎么弄吧。类似的问题出过很多次，而且整个团队养成了以否定别人（可能主要是我这个新人吧）为乐的氛围，来个需求请他们做方案、设计，要么不知道咋做，要么做的惨不忍睹（但凡CSDN翻过几篇博客都能做好）。作为整个平台的设计者，最开始我还耐心地解释为何不用最新的技术/版本、为啥不用陈旧的技术/版本而选择当前的方案，半年后我就放弃了，真心徒劳。讲一个小故事，这个事一说可能有几个大佬就知道我是谁了。

> 我们的平台最多只能算内部ERP之类的工具，本来就想做个单体集群，为了满足大家树新蜂的欲望，拆开了模块化，美其名曰微服务，但是几个核心服务共享一个DB……然后有一次设计复审会议，邀请了业务那边一个比较资深的架构师（水平肯定甩我几条街，我毕竟是测试半路出家的开发），一个小哥为了diss我的设计，联合业务架构师要我把联表查业务实体（万级）和实体属性定义表（5~20条，支持自定义增减）改成多次DAO，也就是把业务数据查出来，再花费两次数据库链接把业务属性也查出来，用java8的streem去合并，跟我说“streem有paralell可以支持并发的你不知道吗？”“JVM里面sort比mysql联表效率高多了”……还好我读过书，这还不算，后面更多类似的场景变成了拆服务，走第三方接口查然后合并数据……我就问网络开销和数据库链接不需要时间吗？一个资深架构师能不知道这些？纯碎是为了否定原先的设计而否定啊！后面业务架构师私下里才告诉我，你就给着他们自由发挥的空间嘛，你毕竟是架构师，你只需要把握大方向……这服务拆分都不管，还有啥更大的方向？写PPT吗我日~~~

- ***强刷存在感*** 为了证明自己是对的，做事情不考虑后果和影响。
  - 线上主机root密码人尽皆知，上线前建议改密码不听。
  - 上线之前要求做数据库异地备份，结果只做本地备份。
  - 上面两点直接导致有一天晚上有人直接把我们的线上环境（k8s）所在主机给系统重装了，所有用户数据丢失……
  - 为了推翻之前的设计做倒退性的重构（细节就不说了），重构开始直接把旧的接口下线，数据库表删除，导致各服务联调无法开展。一问之下，准备上线也是直接这么干，就问了一句：线上的用户数据怎么办，不准备要了吗？对方直接开始无能暴怒……然后还是乖乖地去按照PM地意思去做迁移。关键是，这个被重构的服务之前是我设计的，重构没有征询我一句意见，甚至没有告诉过我要重构这个意向和计划，也不来咨询背后的业务逻辑……最后改了一大圈，联调发现无法支持现有业务逻辑之后，设计还是无限靠回最初的那一版，所不同的是把DB的设计从符合第三范式变成连第二范式都不符合了，揉合了几张表到一起，让单表数据量增加了几倍；然后接口合并到一起，前端通过magic value来区分不同的用途。
  - 仅仅是为了反对别人的方案，宁可违反信息安全规定，抛弃项目组约定好的规则，在请求参数里明文传敏感信息，当然内网应用也无所谓啦。
  - 还有一点就是，一波做测试、测试开发出身的人，按道理应该很反感一个接口多种用途的这种设计，因为这种一旦修改就会要回归很多关联的地方，不明白为什么自己做开发的时候谜之喜爱这种做法，美其名曰复用……复用是这个概念？

- ***猪队友*** 无脑无担当队友的换了一拨又一拨，尤其是前端，业务逻辑摘的干干净净，后端返回了一切可返回的属性，只要按照swagger文档和UI原型图实现就可以了，居然还是搞不定。遇到问题400（参数错误）、404（URL错误），锅一股脑甩到后端去。除了个别靠谱的，其他统一的风格是在群里喊：XXX你这个接口不对啊，报错……连个请求参数都不给，直接来张DevTools console的报错截图，在我看来，这连入门的测试工程师的水平都不如。查了一会，告诉他你没按照接口文档来，他一般第一反应是：咋，你接口又变了？咋不通知我……直到逼得你出具gitlab变更记录告诉他一直如此，他才贱兮兮的私信你说这个问题我们私聊吧。

- ***规划跑偏*** 我们做的是效率工具中的研发过程管理平台，自己的项目却按照最原始的瀑布来做，这本身问题也不大，有些公司瀑布也用的很优秀。问题在于形式化严重，一个简单孤立的CRUD功能，一定要需求文档、方案设计、测试用例评审这些……如果牵涉到复杂的业务流程也就算了，然而并不是……可能是为了讲故事讲的大一点，投入了3倍我所需要的人力，把我的时间都花费在跟项目组成员打交道上面。最开始的愿景是汲取同业优秀产品的特性，做一个自己公司好用的平台，后面做着做着变成了全面朝别人的产品靠拢，而且支持的特性远不如那些经过市场考验的产品……然而，公司很早便已经采购了那些产品的企业版。

***总结*** 我供职过优秀的公司、合作过优秀的项目团队，也供职过垃圾的公司、合作过凑合/不错的项目团队，但是优秀的公司、这谜一样的项目团队是第一次遇到。

- 如果是我亲自招聘，这波人80%进不来，因为我宁可带两个靠谱的人加两个月的班来完成。
- 我带着这个项目从0到1构建起来，可惜后面就不能继续陪伴它的成长和成熟了——虽然产品已经偏离了原先的设计。
- 无论如何，感谢leader和老板给我这么一次机会，脱离原来那家只会找各种由头扣工资的极品垃圾公司，来这里做一次虽然很不敏捷但是还算完整工程化实践，2020年还是进步良多，虽然已经36岁了。
- 产品负责人对产品规划、架构师对技术没有绝对的话语权，以open之名行diss之实，是非常可怕的事情——抑或，这一切的一切，都是事先设计好的，而我的上一任可能比我聪明的多，早早就跑了。而我，却最终也没能忍到春节后拿走原本属于我的那小十万奖金，只能考虑从基金里面赚回来吧。
