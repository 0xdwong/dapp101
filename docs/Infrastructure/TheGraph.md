---
sidebar_position: 1
slug: thegraph
---

# 去中心化的数据库-TheGraph

## 没有 TheGraph 前

TheGraph 尚未出现前，我们需要这样来收集业务数据（比如，NFT租赁市场中的NFT出租记录）：

1. 自行部署或者使用第三方的节点服务；
2. 通过 RPC 监听链上的 NFT 租赁 Event 事件信息，并根据条件读取存储在链上的数据；
3. 将监听到的数据清洗为标准的业务数据，并存入数据；
4. 7＊24 小时运行数据采集程序和节点程序，并对外提供 API 服务。



## 更好的解决方案-TheGraph 

Learn: https://thegraph.com/ecosystem/learn/

- 构建速度更快、数量减少 100% 的服务器

  无需运行自己的数据服务器、构建索引基础设施或解析原始数据。

- 每月支出减少 60-98%

  通过利用 The Graph 竞争激烈的数据市场，减少运行昂贵基础设施的成本和时间。

- 将恢复能力提高 99.99%+ 正常运行时间

  通过全球分布的贡献者网络可确保应用程序正常运行并保持其数据 24*7 流动。

![](https://img.learnblockchain.cn/7g/202404291316178.png)

## GraphQL简介

1. 网站：https://graphql.org/
2. 例子：
   1. [查询UniswapV3数据](https://api.thegraph.com/subgraphs/name/uniswap/uniswap-v3/graphql)
   2. [查询UniswapV3](https://api.thegraph.com/subgraphs/name/uniswap/uniswap-v3?source=uniswap)
   3. [uniswap-v2-dev](https://api.thegraph.com/subgraphs/name/ianlapham/uniswap-v2-dev)
3. 调用 GraphQL：https://graphql.org/graphql-js/graphql-clients/

```js
fetch("/graphql", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Accept: "application/json",
  },
  body: JSON.stringify({ query: "{ hello }" }),
})
  .then(r => r.json())
  .then(data => console.log("data returned:", data))
```

## 如何在TheGraph上部署一个子图

详见：https://thegraph.com/docs/en/developing/creating-a-subgraph/


### 创建前

1. 部署合约到网络中，如 Sepolia；
2. 获取合约的

1. 部署合约
2. 获取合约 ABI
3. 采集子图 Subgraph 项目
4. 初始化字图
5. Graph codegen & graph build. & grpah deploy
   1. Example: https://api.studio.thegraph.com/proxy/64062/mybank/v0.0.3/graphql
   2. Contract Address: [0x3FD542687000843F7B1d79945E8D1b7B44c09FD1](https://sepolia.etherscan.io/address/0x3fd542687000843f7b1d79945e8d1b7b44c09fd1)
6. Add new contract (`grpah add `)
7. Query:  call by http (GraphQL)