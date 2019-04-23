整个CI的九项关键原则：

1. 维护单一代码库
2. 自动化构建
3. 让构建可以自测试
4. 所有提交都要在一台持续集成机器上进行构建
5. 让构建保持快速
6. 在类生产环境上进行测试
7. 让任何人都可以轻松的取得最新的可执行版本
8. 每个人都可以看到发生了什么事情
9. 自动化部署


# More than a process
Continuous Integration is backed by several important principles and practices.
持续集成有几个重要原则和实践的支持


## The practices
* Maintain a single source repository
* Automate the build
* Make your build self-testing
* Every commit should build on an integration machine
* Keep the build fast
* Test in a clone of the production environment
* Make it easy for anyone to get the latest executable version
* Everyone can see what’s happening 
* Automate deployment
## How to do it
* Developers check out code into their private workspaces
* When done, commit the changes to the repository
* The CI server monitors the repository and checks out changes when they occur
* The CI server builds the system and runs unit and integration tests
* The CI server releases deployable artefacts for testing
* The CI server assigns a build label to the version of the code it just built
* The CI server informs the team of the successful build
* If the build or tests fail, the CI server alerts the team
* The team fixes the issue at the earliest opportunity
* Continue to continually integrate and test throughout the project
## Team responsibilities
* Check in frequently
* Don’t check in broken code
* Don’t check in untested code
* Don’t check in when the build is broken
* Don’t go home after checking in until the system builds

Many teams develop rituals around these policies, meaning the teams effectively manage themselves, removing the need to enforce policies from on high.