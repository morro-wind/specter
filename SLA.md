SLA(Service Level Agreement) 服务等协议

定义。以下定义使用于Gmail SLA。
	“中断”是指大于5%的用户错误率（对于域名）。中断以服务器端错误率为依据计算。
	“中断时段”指连续中断十分钟的时间（对于域名）。不足十分钟的间歇中断将不计入中断时段。
	“月正常运行时间百分比”指日历月的总分数减去所有中断时段的中断分钟数，然后除以日历月的总分钟数。
	“预订中断”指Google 提前5天向客户进行通告的中断时段。每个日历年的预订中断将不超过12小时。对于此Gmail SLA，预订中断将不被视为中断，亦不计入中断时段。
	“服务”指在本协议下，由Google 向您提供的Google 服务。
	“服务信用”指：（a）受影响月份的发票收费总额的10%（如果任何日历月的月正常运行时间百分比在99.0%到99.9%之间）;或者(b)受影响月份的发票收费总额的25%（如果任何日历月的月正常运行时间百分比在95.0%到99.0%之间）;（c）受影响月份的发票收费总额的50%（如果任何日历月的月正常运行时间百分比低于95.0%）。


高并发和高可用跟架构息息相关，和框架关系不大。

99.99%的内部目标代表了平衡成本、复杂性和可用性的最佳位置。

*服务可用性*
是中断频率和持续时间的函数。它是通过以下方式衡量的：
*停机频率：* MTTF（平均停机时间）
*持续时间：* MTTR（平均修复时间）。持续时间根据用户的经历定义：从故障开始持续到正常行为恢复。
可用性在数学上定义为使用适当单位的MTTF/（MTTF+MTTR）


当考虑一个新的系统或服务时，当重构或改进一个现有系统或服务时，一个架构/设计评审可以识别出内部与外部的依赖。


第三步包括计算单个组件的可用性。MTBF（Mean time between failure 故障之间的平均时间）和MTTR（Mean time to repair 修复的平均时间）值针对每个组件进行估计。对于硬件组件，MTBF信息可以从硬件制造商的数据表中获得。如果硬件是在内部开发的，则硬件组将为板提供MTBF信息。对硬件的MTTR估计基于操作员对系统的监视程度。这里我们估计大约2小时的硬件MTTR。

MTBF（Mean Time Between Failure）是平均故障间隔的意思，代表两次故障的间隔时间，也就是系统正常运转的平均时间。这个时间越长，系统稳定性越高。
MTTR（Mean Time To Repair）表示故障的平均恢复时间，也可以理解为平均故障时间。这个值越小，故障对于用户的影响越小。
一旦已知MTBF和MTTR，就可以使用以下公式来计算组件的可用性：



评估软件的MTBF 这个活儿还是比较费劲的。软件MTBF实际上是软件重新启动的时间。中间的间隔可以用系统的缺陷率来估计。在这里，我们估计MTBF大约是4000小时。MTTR是重新启动失败处理器的时间。我们的处理器支持自动重启，所以我们估计软件MTTR大约5分钟。请注意，5分钟似乎是在更高的一面。但MTTR应包括以下内容：

由于信号处理器软件崩溃而中止的活动中浪费的时间
检测信号处理器故障的时间
失败的处理器重新启动并返回服务时所花费的时间