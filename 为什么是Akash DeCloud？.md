## 为什么是Akash DeCloud？

### Akash最近的活动正进行得如火如荼，社区和媒体也是非常热闹。今天已经开始了挑战赛第三阶段第二周的第三个任务了，经过深度参与发现，Akash表现出了巨大的潜力，社区已经部署了数百个应用程序，正如我们在链上看到的那样：



```
$ akash query market lease list --node https://rpc.edgenet.akash.forbole.com:443 --count-total | grep total
total: "913"
```





完成第一组挑战后，我开始着手在Akash DeCloud中部署我们的[北斗七星](https://edgenet.akash.bigdipper.live/)（[Big Dipper）](https://edgenet.akash.bigdipper.live/)区块浏览器，以了解迁移真实应用程序的路径。作为具有小型后端数据库的简单Web应用程序，它似乎非常适合在Edgenet中进行测试（非常类似于第1周挑战2中的应用程序）。

坦白说，我对流程的简单性感到震惊。



这是测试挑战的一小步，但却是云计算的一大步。Akash正在慢慢的实现目标：

为 DeFi 而设的 DeCloud 和世界上第一个去中心化云计算市场。







## 部署Big Dipper

### 第1步-转换

GitHub上的北斗七星存储库包含一个[docker-compose文件](https://raw.githubusercontent.com/forbole/big-dipper/master/docker-compose.yml)，可用于本地开发/测试。要将其部署到Akash，我们要做的就是将其转换为[Akash SDL文件](https://docs.akash.network/v/master/documentation/sdl)。鉴于两者的键名非常相似，这是一件容易的事（当然，在前几次尝试中我遇到了一些语法错误，但这些错误很容易解决）。您可以[在此处](https://raw.githubusercontent.com/forbole/awesome-akash/master/deploy-bd-akash.yaml)找到我们的SDL文件进行比较。

### 第2步-部署

[文档中](https://docs.akash.network/v/master/guides/deploy)概述了使用SDL文件创建部署的过程。虽然这部分在初次尝试时并不顺利，但我了解了一些调试过程的有用方法。一旦避免了最初的语法错误，我遇到的下一个问题就是：





```
$ akash tx deployment create deploy-bd-akash.yaml --from $KEY_NAME --node $AKASH_NODE --chain-id $AKASH_CHAIN_ID -y --fees 5000uakt
Enter keyring passphrase:
Error: group westcoast: invalid total CPU (1000 > 2000 > 0 fails)
```





我已将每个容器的cpu要求设置为1个单位。Akash团队有意将每次部署的总数限制为1个单位，以确保提供者有足够的容量来应对挑战。

为了向前发展，我检查了本地运行实例上的资源使用情况，并据此认为，将mongodb分为0.4，将前端分为0.6可能是可行的。





### 第3步-调试

随着cpu值的更新，我能够创建部署并获得租约。我遇到的下一个问题是，在[将清单发送](https://docs.akash.network/v/master/guides/deploy#upload-manifest)给提供程序之后，租约最初看起来是活动的，但是几秒钟后它将消失并关闭。



```
atch "akash provider lease-status --node $AKASH_NODE --dseq $DSEQ --oseq $OSEQ --gseq $GSEQ --provider $PROVIDER --owner $ACCOUNT_ADDRESS"

```



运行上面的命令显示mongodb实例已启动，但Web实例显示为不可用。几秒钟后，浏览到该站点返回了http503。然后，Web实例崩溃了，浏览器的响应更改为404，

`lease-status`命令返回了`Error: server response: 500 Internal Server Error`。（注意：Akash团队已经意识到了无用的500错误，并将对此进行改进）。

为了进一步解决此问题，我需要查看容器内部发生了什么。使用`--help`akash cli实用程序的标志，我能够找到以下有用的命令来流传输容器日志并了解正在发生的情况：

```
akash provider service-logs --node $AKASH_NODE --dseq $DSEQ --oseq $OSEQ --gseq $GSEQ --provider $AKASH_PROVIDER --owner $ACCOUNT_ADDRESS -f --service web
```

问题是...这是另一个语法错误，这次是在传递给Web容器的env变量METEOR_SETTINGS中，这导致它崩溃。







### 第4步-成功！

固定变量后，该应用程序便能够成功启动-现在可以从[http://edgenet.decloud.bigdipper.live/获得](http://edgenet.decloud.bigdipper.live/)。

很难高估此过程的顺利程度，也很难高估Akash必须破坏云计算市场的潜力。我们期待着Mainnet 2的发布，并进行更多的试验，并期待网络的进一步普及/发展！



在我们开始之前，顺便说一下Akash。



第三阶段将加速我们对去中心化和开放的云计算市场的愿景，并作为Mainnet 2的发射台。

#### **奖励** **_____**。

奖励机会的详细时间安排可以查看【Akash挑战网站】（https://akash.network/challenge/）。第三阶段奖励共分配**50万AKT**，包括。

1. **指导性挑战**
2. **开放式挑战**
3. 3. **网络支持挑战**
4. **社区内容挑战**。
5. **奖金挑战**



#### **本周周五直播_____**。

不要错过本周五12月11日太平洋标准时间上午9点/世界协调时间下午5点的第三阶段每周直播，由我们的首席执行官Greg Osuri和首席技术官Adam Bozanich主持，我们的高级全球MarComm经理Michael Gushansky。

我们将提供独家更新、问答和特别奖品。







#### **不要错过最新的第三阶段更新**。

有关Akash的更多信息，请使用以下链接了解更多。

#### **Don’t Miss the Latest Phase 3 Updates**

For more information about Akash, please use the following links to learn more.

Official Website: https://akash.network/?lang=zh-hans

Twitter: https://twitter.com/akashnet_

Telegram: https://t.me/AkashNW



