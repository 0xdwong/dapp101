---
sidebar_position: 1
---


# Foundry 安装与使用

![image](https://book.getfoundry.sh/images/foundry-banner.png)

**Foundry是一个智能合约开发工具链。**

Foundry 可管理合约**依赖项**、**编译**、**运行测试**、**部署合约**，并可利用命令行和链上进行交互。

- 仓库       : https://github.com/foundry-rs/
- 文档       : https://book.getfoundry.sh/
- TG群       : https://t.me/foundry_rs/  
- 登链Foundry文章： https://learnblockchain.cn/search?word=foundry


## 安装 Foundry

最便捷的安装方式———脚本安装：

```sh
curl -L https://foundry.paradigm.xyz | bash
```

安装后，将有 forge、cast、anvil 三个运行程序。


单独运行 `foundryup` 将安装最新的（每晚）预编译的二进制文件：forge、cast、anvil和chisel。请参阅 参考资料 来了解foundryup --help更多选项，例如从特定版本或提交进行安装。


## 工具链:

1. [Forge](https://book.getfoundry.sh/forge/)  ：管理、测试、构建和部署智能合约！
2. [Cast](https://book.getfoundry.sh/cast/) ：用于执行以太坊 RPC 调用的命令行工具。调用智能合约、发送交易或检索链上数据 ！
3. [Anvil](https://book.getfoundry.sh/anvil/)  ：本地测试网节点。可使用它从前端测试合约或进行 RPC 交互！

## Foundry 视频资源

| URL                                                          | Description                                                  | Author                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [![img](https://i.ytimg.com/vi/tgs5q-GJmg4/hq720.jpg)](https://youtube.com/playlist?list=PLO5VPQH6OWdUrKEWPF07CSuVm3T99DQki) | [Foundry 上初级视频的播放列表](https://youtube.com/playlist?list=PLO5VPQH6OWdUrKEWPF07CSuVm3T99DQki) | [Smart Contract Programmer](https://www.youtube.com/@smartcontractprogrammer) |
| [![img](https://i.ytimg.com/vi/hOB1Yiuxojk/maxresdefault.jpg)](https://www.youtube.com/watch?v=hOB1Yiuxojk) | [使用 Foundry 进行智能合约开发的完整介绍](https://www.youtube.com/watch?v=hOB1Yiuxojk) | [Axelar](https://www.youtube.com/@Axelarcore)                |



##  VSCode 插件安装

![alt text](https://img.learnblockchain.cn/7g/202404291315037.png)

## 作业

1. Managing Your DAPP Project with Foundry.
2. Try using the Foundry test your bank.sol (option).

