# 行程\任务 提醒系统

名称：Schedule

为了应对生活中时长忘记的事情，我们需要使用备忘录，但是备忘录存在以下问题

+ 输入麻烦（最大痛点），基本只能支持手写输入
+ 跨平台差，没办法针对当前我所拥有的多个系统进行同步
+ 没有提醒功能，当前无论是备忘录还是提醒事项，都没有办法做到完全的准时或者分时提醒你
+ 完成时需要主动去查找然后删除
+ 没有整理的功能，无法查看历史报表，没有形成正反馈，无法持续保持记录热情
+ ……



针对于这些问题，提出如下需求：

+ 使用Siri+快捷指令 调用接口，同时Siri自动提供语音识别功能，可以快速记录任务，也需要通过Siri对完成的任务进行完成\删除，并可以给出简要的评价。
+ 对 任务 描述进行分割，输入的内容包括任务的结束时间，大概需要的时间，任务的紧急性，任务的类型（可能只是一个想法或者想说的一句话）
+ 拓展性也ok，可以后期开放给多人使用，但是这要求对于用户的隐私做一个保密，目前大致的想法是用户手握一个秘钥进行加密，我们可以获取其他用户的加密文本，不过没有密码无法进行解密，而且秘钥不支持找回，用户需要自己承担保密责任。
+ 生成报表功能，这个报表可以作为平时任务安排的依据，也可以作为回顾的展板（也推荐将报表生成概要文档，每日作为起床的提醒事项来使用）
+ 按照任务的类型，可以多段提醒、定时提醒等等方式提醒
+ 后续进行补充……



使用场景：

我当前有一个任务需要发布，于是我说：“Hey，Siri，新建任务。”<sup>①</sup>

Siri帮我调用了新建任务的快捷指令。然后返回问我任务内容。

我说：“明天下午3点前完成张老师交代的代码审查任务，紧急程度高，任务需要时间30分钟。”

Siri把这些话解析完成之后调用接口，并把我的id，口令传输上去。

系统接口回去了这些信息。首先对文本进行解析，获取其中的内容，然后对任务内容使用口令进行加密，然后存储进入系统之中，返回格式化结果。

Siri朗读结果。



①：文本解析为：

+ Hey，Siri，新建任务：调用Siri进行快捷指令，包括新建任务、说些想法、新建行程、（永久）删除行程、完成任务等等。
+ 明天下午3点前完成张老师交代的代码审查任务，紧急程度高，预计用时30分钟，结束前三小时每小时提醒我一次。
  这一块均为Siri解析完成后调用相应接口时上传的内容，对这部分内容的解析：
    + 明天下午3点前完成张老师交代的代码审查任务：【识别依据：首句】任务内容
    + 紧急程度高：【识别依据：紧急程度】任务程度，可以分为低中高特等。紧急程度同时作为任务提醒的依据，开发第一版不支持用户自定义提醒方式，可到后续开发接口，用户自定义。
    + 预计用时30分钟：【识别依据：预计用时】 分钟、小时、不说都可以













针对于上述功能，将系统模块划分为：

| 模块名称               | 模块功能                                                     | 内容细分                                                     |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 核心-接口-任务         | 调用模块解析任务文本获得有用信息，针对于这些信息处理（加密解密等）后进行增删改查 | 任务新建<br>任务完成+评价<br>任务删除<br>                    |
| 次要-接口-报表         | 对于数据库中的新建任务、已完成任务、删除任务做出统计         | 任务数<br>任务完成率<br>任务平均完成时间<br>任务类型总结<br>任务耗时... |
| 核心-工具-文本解析     | 提供任务文本解析功能，将文本解析成一个对象等，返回           | 任务结束时间*(绝对时间、相对时间(相对时间可根据年月日进行分类))<br> |
| 核心-工具-加密解密实现 | 对文本进行加密解密                                           | 使用对称加密算法加密解密                                     |
| 核心-身份-用户         | 用户进行身份注册、修改信息等                                 | 用户回传身份时使用                                           |
| 次要-接口-批量修改功能 | 用户更换口令，其预约信息需要全部修改                         | 旧口令改新口令，所有加密信息全部修改                         |
|                        |                                                              |                                                              |
|                        |                                                              |                                                              |
|                        |                                                              |                                                              |





