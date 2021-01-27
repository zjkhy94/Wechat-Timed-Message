# 使用Github Actions workflows向微信推送定时消息

[![last-commit](https://img.shields.io/github/last-commit/HollowMan6/Wechat-Timed-Message-Through-Actions)](../../graphs/commit-activity)
![Python package](../../workflows/Python%20package/badge.svg)

[![Followers](https://img.shields.io/github/followers/HollowMan6?style=social)](https://github.com/HollowMan6?tab=followers)
[![watchers](https://img.shields.io/github/watchers/HollowMan6/Wechat-Timed-Message-Through-Actions?style=social)](../../watchers)
[![stars](https://img.shields.io/github/stars/HollowMan6/Wechat-Timed-Message-Through-Actions?style=social)](../../stargazers)
[![forks](https://img.shields.io/github/forks/HollowMan6/Wechat-Timed-Message-Through-Actions?style=social)](../../network/members)

[![Open Source Love](https://img.shields.io/badge/-%E2%9D%A4%20Open%20Source-Green?style=flat-square&logo=Github&logoColor=white&link=https://hollowman6.github.io/fund.html)](https://hollowman6.github.io/fund.html)
[![GPL Licence](https://img.shields.io/badge/license-GPL-blue)](https://opensource.org/licenses/GPL-3.0/)
[![Repo-Size](https://img.shields.io/github/repo-size/HollowMan6/Wechat-Timed-Message-Through-Actions.svg)](../../archive/master.zip)

[![Total alerts](https://img.shields.io/lgtm/alerts/g/HollowMan6/Wechat-Timed-Message-Through-Actions.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/HollowMan6/Wechat-Timed-Message-Through-Actions/alerts/)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/HollowMan6/Wechat-Timed-Message-Through-Actions.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/HollowMan6/Wechat-Timed-Message-Through-Actions/context:python)
[![](https://images.microbadger.com/badges/image/hollowman6/send-message-to-wechat.svg)](https://microbadger.com/images/hollowman6/send-message-to-wechat)

(English version is down below)

### 好用记得收藏(右上角**加星★Star**)哦!

[微信消息推送脚本](Wechat-Timed-Message-Through-Actions.py)

[工作流存放文件夹](.github/workflows)

支持[Fork本仓库直接使用工作流(推荐)](#使用方法)，[自行创建仓库使用工作流](#自行配置工作流)，[CronTab运行](#crontab)，[Docker运行](#docker)，[Kubernetes运行](#kubernetes)等。

## 使用方法

你需要fork本仓库，之后在你fork的仓库中创建相关Actions Secret并进行相关设置(按下图所示点击1，2，3的次序，即可进入新建Actions secrets的界面):

![](img/secrets.png)

你可以从以下三个推送平台中任选一个或多个来接受推送的消息：

### PushPlus(推荐)

[登录PushPlus](https://pushplus.hxtrip.com/login)，然后在pushplus网站中找到您的token，创建一个Name为`PPTOKEN`，value为您的token值的Actions secret，就可以进行一对一推送信息。

如果需要对多个账号推送信息，即一对多推送，还需要另外新建一个群组，记下群组编码，然后创建一个Name为`PPTOPIC`，value为您的群组编码的Actions secret。

![](https://pushplus.hxtrip.com/doc/img/c1.png)

### Server酱

如使用[Server酱](http://sc.ftqq.com/)来实现，它的配置方法请参考其说明文档。

然后，你只需要创建一个Name为`SERVERCHANSCKEY`，value为[你的SCKEY调用代码值](http://sc.ftqq.com/?c=code)的Actions secret即可自动让仓库的工作流通过Server酱为你推送消息。

### Server酱测试号版

如果要使用[Server酱测试号版](https://sct.ftqq.com/)，请创建一个/修改Name为`SERVERCHANSCKEY`，value为[你的SendKey值](https://sct.ftqq.com/sendkey)的Actions secret。另外创建一个Name为`OPENID`的Actions secret，如果value值为`0`则是通过公众号仅发给自己。否则将value值设定为关注你测试公众号的那个用户的微信号openid，这时将发给自己的同时还会发送给那个指定用户。

如果需要转换回普通的Sever酱请将`OPENID` Actions secret删除即可。

---

上述配置成功后，配置工作流文件，以[工作流1.yml](.github/workflows/1.yml)为模板，创建你自己的工作流或者在提供的工作流上进行修改。你可以任意更改name为无空格的英文字母和数字组合的字符串，cron为你想要发送消息的指定时间(你可以使用[crontab guru](https://crontab.guru/)进行cron表达式的调试，所有时间均为UTC时间，请进行时区换算)(因为Github方的原因，预定运行时间可能会有半小时左右的延迟)。然后创建一个或者两个Actions secrets，一个必须创建，其name为`TITLE[name]`（请将这里的`[name]`修改为workflow的name），value为要发送消息的标题，例如在提供的工作流中，这里的name为`TITLE1`；另一个为可选的，其name为`MSG[name]`，同理进行相应的替换，value为要发送消息的标题。

随后，按下图所示点击1，2，3，4的次序，你可以手动触发工作流的执行来进行测试。
   
![](img/workflow.png)

点开任意一个运行记录，依次点开下图所示1，2，你可以看到运行记录。

![](img/run.png)

如果某次因为某些因素工作流运行失败，GitHub会自动发邮件提醒工作流运行失败。

**新**：增加可选的遇到发送消息失败的情况，自动重启工作流，并等待一段时间后再次发送消息。如果你需要这个功能，则请创建一个Personal Access Token, [获取教程](https://docs.github.com/cn/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token)(第7步令牌的作用域权限你只需要选中workflow这一栏即可)。然后创建一个Name为`GPATOKEN`，value为你的令牌值的Actions Secret。

默认再次发送消息等待时间为30分钟，如果你有需要可以将[这里](
https://github.com/HollowMan6/Wechat-Timed-Message-Through-Actions/blob/main/.github/workflows/1.yml#L42)的`30m`替换为你想要的数值，这里的时间遵循Linux sleep 函数对应时间语法：一个数字后接 `s` 对应秒, `m` 对应分钟等。

如果是因为本仓库程序本身因为失效而导致的报错，你可以取消正在运行中的工作流从而终止这一循环。

## 自行配置工作流

你可以自行创建一个仓库并自行配置工作流进行使用，[示例工作流文件](.github/workflows/1-docker.yml)

### 输入

#### 必须

* TITLE: 消息标题

#### 可选

* MSG: 消息主体
* DELAYS: 设置发送消息时间延迟
* SERVERCHANSCKEY: Server酱 SCKEY
* OPENID: Server酱测试号版 微信公众号用户OpenID
* PPTOKEN: PushPlus Token
* PPTOPIC: PushPlus 群组编码

### 示例

```yaml
- name: 'Send Message to Wechat'
  uses: HollowMan6/Wechat-Timed-Message-Through-Actions@main
  with:
    DELAYS: ${{ github.event.inputs.delays }}
    SERVERCHANSCKEY: ${{ secrets.SERVERCHANSCKEY }}
    OPENID: ${{ secrets.OPENID }}
    PPTOKEN: ${{ secrets.PPTOKEN }}
    PPTOPIC: ${{ secrets.PPTOPIC }}
    TITLE: ${{ secrets.TITLE }}
    MSG: ${{ secrets.MSG }}
```

## Docker

Docker Hub: https://hub.docker.com/r/hollowman6/send-message-to-wechat

如果你需要通过Docker运行，只需要将上述Actions Secret变量名和值分别设置为环境变量(另外增加一个DELAYS为发送消息等待时间，值同[使用方法](#使用方法)步骤6中要求)，然后执行下述命令即可：
```bash
docker run -it \
    -e TITLE="$TITLE" \
    -e MSG="$MSG" \
    -e DELAYS=$DELAYS \
    -e SERVERCHANSCKEY=$SERVERCHANSCKEY \
    -e OPENID=$OPENID \
    -e PPTOKEN=$PPTOKEN \
    -e PPTOPIC=$PPTOPIC \
    hollowman6/send-message-to-wechat
```

**创建**

```bash
docker build -t hollowman6/send-message-to-wechat .
```

该Docker镜像也可以在云服务器中结合Kubernetes的CronJob运行等，可能性无限多。

## CronTab

*注:* 如要在自己的Linux服务器上使用crontab执行定时任务来进行自动发送消息，推荐使用[Docker](#docker)。你也可以clone本仓库，安装好相关Python依赖后改编[entrypoint.sh](entrypoint.sh)文件中python程序的路径，将上述Actions Secret变量名和值分别设置为系统环境变量(另外增加一个DELAYS为发送消息等待时间，值同[使用方法](#使用方法)步骤6中要求)，即可运行。

## Kubernetes

参考配置文件见[K8s](K8s), 只要运行[create.sh](K8s/create.sh)即可创建相关Actions Secret、ConfigMap和CronJob。

你可以[更改这里来设定DELAYS变量](K8s/Wechat-Timed-Message.yml#L6)

还可以[更改这里来设定Cron表达式](K8s/Wechat-Timed-Message.yml#L15)

**警告**：

***仅供测试使用，不可用于任何非法用途！***

***对于使用本代码所造成的一切不良后果，本人将不负任何责任！***

# Send timed message to Wechat through Github Actions workflows

### Please **★Star** if you think it's great!

[Python library dependency](../../network/dependencies)

[Workflows](.github/workflows)

Support [Fork this repository to use workflows(Recommend)](#usage)，[Self-Configure Workflow](#self-configure-workflow)，[run using CronTab](#crontab)，[run with Docker](#docker)，[run with Kubernetes](#kubernetes) etc.

## Usage

you can fork this repository first, and then create Actions Secrets and set related settings in your forked repository (click in the order of 1, 2 and 3 as shown in the figure below).

![](img/secrets.png)

You can choose one or more of the following three push platforms to receive pushed messages:

### PushPlus(Recommended)

First [log into pushplus](https://pushplus.hxtrip.com/login), and then find your token in pushplus website, create a actions secret with the name of `PPTOKEN` and the value of your token value, and then one-to-one push the related information results.

If you need to push the related information to multiple Wechat accounts, that is, one-to-many push, you need to create a group, write down the group code, and then create an actions secret with the name of `PPTOPIC` and the value of your group code.

![](https://pushplus.hxtrip.com/doc/img/c1.png)

### ServerChan

We Use [Server Chan](http://sc.ftqq.com/) to realize its functionality. For its configuration method, please refer to its documentation (In Chinese).

Then, you just need to create an Actions Secret whose name is `SERVERCHANSCKEY` and value is [Your SCKEY](http://sc.ftqq.com/?c=code). Then the workflow can automatically push the relevant information for you.

### ServerChan Testing Subscription Version

If you want to use [ServerChan Testing Subscription Version](https://sct.ftqq.com/), please create/modify the Actions secret with the Name `SERVERCHANSCKEY` and the value [your sendkey value](https://sct.ftqq.com/sendkey). In addition, create a Actions secret with Name as `OPENID`, if the value is `0`, it is only send to yourself. Otherwise, set the value to be the specified user's Wechat openid who subscribed the Testing Subscription account, then it will send it to the designated user and yourself at the same time.

If you need to switch back to normal SeverChan, please delete the `OPENID` actions secret.

---

After the above configuration is successful, configure the workflow file and use [工作流1.yml](.github/workflows/1.yml) as the template to create your own workflow or modify the workflow provided. You can change the name into a string of letters or numbers without spaces. Cron is the specified time when you want to send a message (you can use [crontab guru](https://crontab.guru/) For cron expression debugging, all the time zone is in UTC, please convert the time zone into yours. (Due to the mechanism realized by Github, there may exist a delay for about half an hour.) Then create one or two actions Secrets: one must be created, its name is`TITLE[name]`(please change `[name]` here into the name of workflow), and value is the title of the message to be sent. For example, in the provided workflow, the name is `TITLE1`; the other is optional, its name is `MSG[name]`, and the corresponding replacement is carried out, and value is the title of the message to be sent.

Then, click in the order of 1, 2, 3 and 4 as shown in the figure below. You can manually trigger the execution of workflow to test.

![](img/workflow.png)

Click any running record, and then click in the order of 1 and 2 as shown in the figure below. You can see the running record and error description.
![](img/run.png)

If the workflow fails due to some errors, GitHub will automatically send an email to remind the workflow of failure.

**NEW**: Add the optional option to restart the workflow automatically in case of Send Message to Wechat in failure, and wait for a period of time to re-run workflow again automatically. If you need this, please create a Personal Access Token, [Here's Guides to create](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token)(In step 7 scopes or permissions, you only need to select the `workflow` row). Then create an Actions Secret with the name of `GPATOKEN` and the value with your token.

The default waiting time is 30 minutes. You can replace `30m` [here](
https://github.com/HollowMan6/Wechat-Timed-Message-Through-Actions/blob/main/.github/workflows/1.yml#L42) with the time you want. The time here follows the Linux sleep syntax for time units: a number followed by `s` for seconds, `m` for minutes, etc.

If the error is caused by the repository program itself, you can cancel the running workflow to terminate the loop.


## Self-Configure Workflow

You can create your own repository and configure your own workflow to use, [Example Workflow YAML File](.github/workflows/1-docker.yml)

### Input

#### Required

* TITLE: Your Message Title

#### Optional

* MSG: Your Message Content
* SERVERCHANSCKEY: ServerChan SCKEY
* OPENID: ServerChan Testing Subscription Version Testing Subscription account User OpenID
* PPTOKEN: PushPlus Token
* PPTOPIC: PushPlus Topic

### Example

```yaml
- name: 'Send Message to Wechat'
  uses: HollowMan6/Wechat-Timed-Message-Through-Actions@main
  with:
    DELAYS: ${{ github.event.inputs.delays }}
    SERVERCHANSCKEY: ${{ secrets.SERVERCHANSCKEY }}
    OPENID: ${{ secrets.OPENID }}
    PPTOKEN: ${{ secrets.PPTOKEN }}
    PPTOPIC: ${{ secrets.PPTOPIC }}
    TITLE: ${{ secrets.TITLE }}
    MSG: ${{ secrets.MSG }}
```

## Docker

Docker Hub: https://hub.docker.com/r/hollowman6/send-message-to-wechat

If you need to run through docker, just set the above Actions Secrets name and value as environment variables (In addition, add a DELAYS as the waiting time, and the value is the same requirement as that in step 6 of [usage](#usage)), and then execute the following command:

```bash
docker run -it \
    -e TITLE="$TITLE" \
    -e MSG="$MSG" \
    -e DELAYS=$DELAYS \
    -e SERVERCHANSCKEY=$SERVERCHANSCKEY \
    -e OPENID=$OPENID \
    -e PPTOKEN=$PPTOKEN \
    -e PPTOPIC=$PPTOPIC \
    hollowman6/send-message-to-wechat
```

**Build**

```bash
docker build -t hollowman6/send-message-to-wechat .
```

The docker image here can also be runned in combination with Kubernetes' CronJob in the Cloud Clusters etc. THere're unlimited possibilities.

## CronTab

*PS:* If you want to use crontab on your own Linux server to execute the send message, I recommend using [docker](#docker), otherwise please clone this repository and after installing relevant Python dependencies, adapt the path of the python program in [entrypoint.sh](entrypoint.sh). Set the Actions Aecrets name and value mentioned above as the environment variable respectively (In addition, add a DELAYS as the waiting time, and the value is the same requirement as that in step 6 of [usage](#usage)) to run.

## Kubernetes

You can refer to the configuration file [K8s](K8s).Also create the relevant Secrets ConfigMap and CronJob by running [create.sh](K8s/create.sh)

You can also [change here to set `DELAYS` Variable](K8s/Wechat-Timed-Message.yml#L6)

Also [change here to set Cron expression](K8s/Wechat-Timed-Message.yml#L15)

**Warning**:

***For TESTING ONLY, not for any ILLEGAL USE!***

***I will not be responsible for any adverse consequences caused by using this code.***
