总结概括为以下10条：

不要直接部署裸的Pod。
为工作负载选择合适的Controller。
使用Init容器确保应用程序被正确的初始化。
在应用程序工作负载启动之前先启动service。
使用Deployment history来回滚到历史版本。
使用ConfigMap和Secret来存储配置。
在Pod里增加Readiness和Liveness探针。
给Pod这只CPU和内存资源限额。
定义多个namespace来限制默认service范围的可视性。
配置HPA来动态扩展无状态工作负载。