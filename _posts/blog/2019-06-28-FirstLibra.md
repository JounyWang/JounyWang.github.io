---
layout:     post
title:      Facebook Libra 转账交易初体验
category: blog
description: 天秤座
---


# 第一笔交易 · Libra


这篇文章将尝试在 Libra 区块链上进行第一笔交易。


在开始之前确保你对 Libra 有基本的了解。

## 准备

以下所有程序命令的前提条件:

  * 确保操作系统为 Linux or macOS
  * 稳定的网络环境
  * git 已安装
  * `yum`or `apt-get` 已安装 Linux system.

从我的情况来看，整个流程会占用约8G存储，请提前确保存储情况，保留冗余。内存不要太小，可能会死机。


## 提交交易的所需步骤



  1. [克隆代码](https://developers.libra.org/docs/my-first-transaction#clone-and-build-libra-core).
  2. [构建 Libra CLI client 并连接到测试网](https://developers.libra.org/docs/my-first-transaction#build-libra-cli-client-and-connect-to-the-testnet).
  3. [创建 A 和 B 账户](https://developers.libra.org/docs/my-first-transaction#create-alice-s-and-bob-s-account).
  4. [账户生成 Libra 币](https://developers.libra.org/docs/my-first-transaction#add-libra-coins-to-alice-s-and-bob-s-accounts).
  5. [提交第一笔交易记录到区块链](https://developers.libra.org/docs/my-first-transaction#submit-a-transaction).


## 克隆并构建源代码

### 克隆源代码
    
    
    git clone https://github.com/libra/libra.git
    
    


    
### 构建源代码
    
    
    
    
切入 Libra 源代码文件夹根目录
    
    

    
    cd libra
    ./scripts/dev_setup.sh
    


这篇文章发生时间是6月20号，也就是Facebook 刚宣布 Libra 的第二天，当时bug非常多，执行下来需要手动解决很多问题。

颇用了一些功夫去解决这些错误，等解决完毕然后发现源代码也修补了这个问题，是很无奈的，感觉浪费了时间精力。

另一个方面也凸显出社区更新情况，确实非常活跃。

既然程序在不断更新，这一步我遇到的各种bug都不再细说了。
    
    
构建脚本如果执行失败请查阅:

[Troubleshooting](https://developers.libra.org/docs/my-first-transaction#setup)




## 构建 Libra CLI Client 并连接到测试网


    
这一步用时较长
    

    
    
    
    
    ./scripts/cli/start_cli_testnet.sh
    
    

同时也是CPU占用最高的时候。

竟然到了 300% …

[![4440D88E-B463-48D5-BA7B-E2487AD2ED1D.jpeg](https://i.loli.net/2019/06/28/5d16138a0eb7c86144.jpeg)](https://i.loli.net/2019/06/28/5d16138a0eb7c86144.jpeg)



执行完成会显示如下:
    
    
    
    
    
    usage: <command> <args>
    
    Use the following commands:
    
    account | a
      Account operations
    query | q
      Query operations
    transfer | transferb | t | tb
      <sender_account_address>|<sender_account_ref_id> <receiver_account_address>|<receiver_account_ref_id> <number_of_coins> [gas_unit_price (default=0)] [max_gas_amount (default 10000)] Suffix 'b' is for blocking.
      Transfer coins from account to another.
    help | h
      Prints this help
    quit | q!
      Exit this client
    
    
    Please, input commands:
    
    libra%
    
    
如果失败建议分时段多尝试，可能测试网络繁忙，我遇到了这种情况。
    

如果仍然失败请查阅:


 [Troubleshooting](https://developers.libra.org/docs/my-first-transaction#client-build-and-run).
    
    
    


    
    
    
    
## 创建 A B 账户
    
    
    
    
一旦成功连接到区块链测试网，我们就可以开始创建账户了，也就是钱包地址。
    
    
    
### Step 1: 检查 CLI Client 在运行

即上一步执行完并进入测试网后
    
输入
    
    account
    
显示如下
    
    
    
    libra% account
    usage: account <arg>
    
    Use the following args for this command:
    
    create | c
      Create an account. Returns reference ID to use in other operations
    list | la
      Print all accounts that were created or loaded
    recover | r <file path>
      Recover Libra wallet from the file path
    write | w <file name>
      Save Libra wallet mnemonic recovery seed to disk
    mint | mintb | m | mb <receiver account> <number of coins>
      Mint coins to the account. Suffix 'b' is for blocking
    
    
    
    
    
### Step 2: 创建 A 账户钱包地址
    
    
    
    
注意 创建账户并不会更新区块链，只是在本地生成秘钥对
    
    
    
    
创建 A 账户，输入以下命令
    
    
    
    
    libra% account create
    

在手机上连接我 Google 的 VPS 操作

    
[![754BC880-D17B-4F76-8A05-3B81CD87EEBD.jpeg](https://i.loli.net/2019/06/28/5d161389acc2f58912.jpeg)](https://i.loli.net/2019/06/28/5d161389acc2f58912.jpeg)
    
成功的输出样例
    
    
    
    
    
    >> Creating/retrieving next account from wallet
    Created/retrieved account #0 address 3ed8e5fafae4147b2a105a0be2f81972883441cfaaadf93fc0868e7a0253c4a8
    
    
    
    
    
#0 是 A 账户的索引，只是用来连接到 A 账户，这个索引对于区块链来讲是无意义的。A 账户真正在区块链上有所体现是往 A 账户生成一些 Libra 币 或者 别的账户往 A 账户N进行转账。
这个索引只能用在放下的命令行，用来连接到 A 账户。
    
    
    
    
### Step 3: 创建 B 账户
    
    
    
    
重复上一步命令
    
    
    
    
    libra% account create
    
    
    
    
成功的输出样例
    
    
    
    
    
    >> Creating/retrieving next account from wallet
    Created/retrieved account #1 address 8337aac709a41fe6be03cad8878a0d4209740b1608f8a81566c9a7d4b95a2ec7
    
    
    
    
    
    #1 是 B 账户的索引，同我上面的解释。更多细节查阅 [创建 A 账户](https://developers.libra.org/docs/my-first-transaction#step-2-create-alice-s-account)
    
    
    
    
### Step 4 (可选1): 查看账户列表
    
    
    
    
执行如下命令
    
    
    
    
    libra% account list
    
    
    
    
成功则输出样例
    
    
    
    
    
    User account index: 0, address: 3ed8e5fafae4147b2a105a0be2f81972883441cfaaadf93fc0868e7a0253c4a8, sequence number: 0
    User account index: 1, address: 8337aac709a41fe6be03cad8878a0d4209740b1608f8a81566c9a7d4b95a2ec7, sequence number: 0
    
    
    
    
    
sequence number 代表从这个账户发出的交易，每次交易执行并记录在区块链上都会增加。更多细节请查阅[sequence number](https://developers.libra.org/docs/reference/glossary#sequence-number).
    
    
    
    
## 增加 Libra 币到 A B 账户
    
    
    
    
    铸币和增加是通过 Faucet实现的. Faucet 是测试网的一项服务，不会存在于主网。关于主网查阅[mainnet](https://developers.libra.org/docs/reference/glossary#mainnet). It creates Libra with no real-world value. Assuming you have [created Alice’s and Bob’s account](https://developers.libra.org/docs/my-first-transaction#create-alice-s-and-bob-s-account), with index 0 and index 1 respectively, you can follow the steps below to add Libra to both accounts.
    
    
    
    
### Step 1: 增加 110 个 Libra 到 A 账户
    
    
    
    
铸币并增加的命令
    
    
    
    
    libra% account mint 0 110
    
    
    
    
    
    
* 0 为 A 账户索引
    
    
* 110  是往 A 账户增加的数量
    
    
成功则输出样例
    
    
    
    
    
    >> Minting coins
    Mint request submitted
    
    
    
    
    
    
如果这步出现错误请查阅

[Troubleshooting](https://developers.libra.org/docs/my-first-transaction#minting-and-adding-money-to-account)
    
    
    
    
### Step 2: 增加 52 个 Libra 币 到 B 账户
    
    
    
    

    
    
    
    
    libra% account mint 1 52
    
    

    
    
    
    
输出
    
    
    
    
    
    >> Minting coins
    Mint request submitted
    
    
    
    
    
    If your account mint command did not submit your request successfully, refer to
    [Troubleshooting](https://developers.libra.org/docs/my-first-transaction#minting-and-adding-money-to-account)
    
    
    
    
### Step 3: 检查余额
    
    
    
    
查询命令如下
    
    
    
    
    libra% query balance 0
    
    
    
    
输出
    
    
    
    
    Balance is: 110
    
    
    
    
查询 B 账户
    
    
    
    
    libra% query balance 1
    
    
    
    
输出
    
    
    
    
    Balance is: 52
    
    
    
    
## 提交交易
    
    
    
    

    
    
    
    
### 查询当前 Sequence number 
    
    
    
    
    
    libra% query sequence 0
    >> Getting current sequence number
    Sequence number is: 0
    libra% query sequence 1
    >> Getting current sequence number
    Sequence number is: 0
    
    
    
    
    

    
    
    
    
### 转账
    
    
    
    
从 A 往 B 转 10 个 Libra 币
    
    
    
    
    libra% transfer 0 1 10
    
    
    
    
    
    
输出
    
    
    
    
    
    >> Transferring
    Transaction submitted to validator
    To query for transaction status, run: query txn_acc_seq 0 0 <fetch_events=true|false>
    
    
    
    
    
    
    
你已经提交了交易到测试网的检验点，在[mempool](https://developers.libra.org/docs/reference/glossary#mempool)，但并不意味着已经出现在区块链上。如果系统繁忙或者过载堵塞，就需要时间去看到结果。

你需要用这个命令多次确认:
    
    query account_state 0

    
    
如果转账出现错误请查阅
    
[Troubleshooting](https://developers.libra.org/docs/my-first-transaction#the-transfer-command).
    
    
    

    
    
    
    
### 查询 Sequence Number 
    
    
可以看到各有了一次记录
    
    
    libra% query sequence 0
    >> Getting current sequence number
    Sequence number is: 1
    libra% query sequence 1
    >> Getting current sequence number
    Sequence number is: 0
    
    
    
    
    

    
    
    
    
### 转账完成之后查询双方余额
    
    
    

    
    
    
    
    
    libra% query balance 0
    Balance is: 100
    libra% query balance 1
    Balance is: 62
    
    
    
    
    
### Congratulations!
    
    
    
    
已经成功的进行第一笔转账交易!

---
---

如需转载，请务必保留如下信息:


    原文作者:Lin
    原文标题:Facebook Libra 转账交易初体验
    原文链接:https://www.linjie.fun/FirstLibra


---
    


如有任何技术问题，请通过[邮箱](mailto:jounywang2014@gmail.com)或微信与我联系。




    
    
    
    
