# 多链助记词碰撞批量余额查询工具

## 概述

这是一个多链加密货币钱包助记词暴力破解工具，支持 Bitcoin (BTC)、Ethereum (ETH)、Binance Smart Chain (BNB) 和 Solana (SOL) 四条主要区块链。该工具通过生成随机助记词，派生对应的钱包地址，并查询这些地址的余额来寻找有资金的钱包。

## 功能特性

- **多链支持**: 支持 BTC、ETH、BNB、SOL 四条主要区块链
- **兼容性**: 与欧意 (OKX) SDK 兼容的地址生成方式
- **国内友好**: 使用中国大陆可访问的 API 服务
- **彩色输出**: 使用 colorama 库提供彩色终端输出
- **容错机制**: 多个 API 节点备份，确保查询稳定性

## 技术架构

### 派生路径配置

| 区块链 | 派生路径 | 地址格式 |
|--------|----------|----------|
| BTC | `m/44'/0'/0'/0/0` | Legacy (P2PKH, 1开头) |
| ETH | `m/44'/60'/0'/0/0` | 标准以太坊地址 |
| BNB | `m/44'/60'/0'/0/0` | 与 ETH 相同 |
| SOL | `m/44'/501'/0'/0'` | Solana HD 派生 |

### API 服务配置

#### Bitcoin (BTC)
- mempool.space
- blockstream.info  
- blockcypher.com

#### Ethereum (ETH)
- eth.drpc.org
- ethereum-rpc.publicnode.com
- rpc.payload.de
- eth.merkle.io

#### Binance Smart Chain (BNB)
- bsc.drpc.org
- bsc-rpc.publicnode.com
- rpc.ankr.com/bsc
- bsc.meowrpc.com

#### Solana (SOL)
- solana.drpc.org
- solana-rpc.publicnode.com
- rpc.ankr.com/solana
- api.mainnet-beta.solana.com

## 核心功能模块

### 1. 助记词生成
```python
mnemo = Mnemonic("english")
words = mnemo.generate(strength=128)  # 生成12词助记词
```

### 2. 地址派生

#### Bitcoin 地址生成
- 使用 bitcoinlib 库进行 HD 钱包派生
- 生成 Legacy 格式地址 (P2PKH)
- 支持标准的 BIP44 派生路径

#### Ethereum/BNB 地址生成
- 使用 eth_account 库
- 支持标准的以太坊地址格式
- BNB 使用与 ETH 相同的派生路径

#### Solana 地址生成
- 自定义 HD 派生实现
- 使用 Ed25519 曲线
- 支持 Solana 标准的 HD 派生

### 3. 余额查询

每个区块链都配置了多个 API 节点，采用故障转移机制：
- 按顺序尝试每个节点
- 成功获取余额后立即返回
- 只在所有节点都失败时输出错误信息


## 使用方法

``` ![image](https://github.com/user-attachments/assets/785a7434-f345-4ccc-9ce5-d252ba985a15)


### 程序输出示例
```
多链助记词碰撞批量余额查询 (BTC/ETH/BNB/SOL)
🇨🇳 使用中国大陆可访问的API服务
🔗 与欧意SDK兼容的地址生成
运行模式: 单线程
派生路径:
  BTC: m/44'/0'/0'/0/0 (Legacy格式)
  ETH: m/44'/60'/0'/0/0
  BNB: m/44'/60'/0'/0/0
  SOL: m/44'/501'/0'/0' (HD派生)
开始检查助记词...

检查第 1 个助记词 - abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about
BTC m/44'/0'/0'/0/0 1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2 余额: 0
ETH m/44'/60'/0'/0/0 0x9858EfFD232B4033E47d90003D41EC34EcaEda94 余额: 0
BNB m/44'/60'/0'/0/0 0x9858EfFD232B4033E47d90003D41EC34EcaEda94 余额: 0
SOL m/44'/501'/0'/0' 11111111111111111111111111111112 余额: 0

## 安全警告

⚠️ **重要提示**: 
- 此工具仅用于教育和研究目的
- 暴力破解他人钱包是非法行为
- 请遵守当地法律法规
- 不要用于任何非法活动

## 技术细节

### HD 钱包派生算法
工具实现了完整的 BIP32/BIP44 标准：
- 主密钥派生使用 HMAC-SHA512
- 子密钥派生支持硬化和非硬化路径
- 与主流钱包软件兼容

### 容错机制
- 多 API 节点备份
- 网络请求超时控制 (8秒)
- 静默失败处理
- 自动重试机制

### 性能优化
- 单线程设计，避免 API 限流
- 请求间隔控制 (0.1秒)
- 内存高效的地址生成

## 配置说明


### 修改派生路径
```python
DERIVATION_PATHS = {
    'btc': "m/44'/0'/0'/0/0",
    'eth': "m/44'/60'/0'/0/0", 
    'bnb': "m/44'/60'/0'/0/0",
    'sol': "m/44'/501'/0'/0'",
}
```

### 添加新的 API 节点
在对应的 `*_RPC_NODES` 列表中添加新的节点地址。

## 许可证

本项目仅供学习和研究使用，请勿用于非法用途。
