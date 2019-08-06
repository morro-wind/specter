## 安裝要求：
**系統：** Linux
**Java環境:** 推薦JAVA8
**Zookeeper**

使用Zookeeper 保存集羣的元數據信息和消費者信息

## kafka 常規配置
**broker.id**
每個broker 都需要一個標識符，使用broker.id 來表示。它的默認值是0,也可以被設置成其他任意整數。這個值在整個Kafka 集羣裏必須是唯一的。建議設置成與機器名具有相關性的整數，這樣在進行維護時，將ID號映射到機器名就沒那麼麻煩了。

**port**
使用配置樣本啓動kafka， 會監聽9092端口。不建議使用root權限啓動

**zookeeper.connect**
用於保存broker 元數據的Zookeeper 地址。該配置參數用冒號分隔的一組hostname:port/path 列表，每一部分的含義如下：
* hostname 是Zookeeper 服務器的機器名或IP地址;
* port 是Zookeeper 的客戶端連接端口;
* /path 是可選的Zookeeper 路徑，作爲Kafka 集羣的chroot 環境。如果不指定，默認使用根路徑。

如果chroot 路徑不存在，broker 會在啓動的時候創建。

> **爲什麼使用chroot 路徑**
在Kafka 集羣裏使用chroot 路徑是一種最佳實踐。Zookeeper 羣組可以共享給其他應用程序，即使還有其他Kafka 集羣存在，也不會產生衝突。最好是在配置文件裏指定一組Zookeeper 服務器，用分號把它們隔開。一旦有一個Zookeeper 服務器宕機，broker 可以連接到Zookeeper 羣組的另一個節點上。

**log.dirs**
Kafka 把所有消息都保存在磁盤上，存放這些日志片段的目錄是通過log.dirs 指定的。它是一組用逗號分隔的本地文件系統路徑。如果指定了多個路徑，那麼broker 會根據“最少使用”原則，把同一個分區的日志片段保存到同一個路徑下。要注意，broker 會往擁有最少數目分區的路徑新增分區，而不是往擁有最小磁盤空間的路徑新增分區。

**num.recovery.threads.per.data.dir**
對於如下3種情況，Kafka 會使用可配置的線程池來處理日志片段：
* 服務器正常啓動，用於打開每個分區的日志片段;
* 服務器崩潰後重啓，用於檢查和截短每個分區的日志片段;
* 服務器正常關閉，用於關閉日志片段。

這些線程只是在服務器啓動和關閉時會用到。默認每個日志目錄只使用一個線程。設置此參數時需要注意，所配置的數字對應的是log.dirs指定的單個日志目錄。總線程= *log.dir 指定路徑個數。

**auto.create.topics.enable**
默認情況，Kafka 會在如下幾種情形下自動創建主題：
* 當一個生產者開始往主題寫入消息時;
* 當一個消費者開始從主題讀取消息時;
* 當任意一個客戶端向主題發送元數據請求時。

