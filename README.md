# Forge Template

一个功能增强的 Foundry 项目模板，提供便捷的 Makefile 命令、自动化部署地址管理和多网络配置支持。

## ✨ 特性

- 🛠️ **Makefile 支持** - 简化常用命令，一键部署到本地或测试网
- 💾 **部署地址管理** - 自动保存和加载合约部署地址
- 🔐 **Keystore 账户** - 使用 `cast wallet` 安全管理私钥
- 🌐 **多网络配置** - 预配置本地 Anvil 和 Sepolia 测试网
- ✅ **自动验证** - Sepolia 部署自动进行合约验证

## 📁 项目结构

```
forge-template/
├── src/              # 合约源码
├── script/           # 部署脚本
├── test/             # 测试文件
├── deployments/      # 部署地址记录 (JSON)
├── lib/              # 依赖库
├── Makefile          # 便捷命令
├── foundry.toml      # Foundry 配置
└── .env.example      # 环境变量示例
```

## 🚀 快速开始

### 0. 创建项目

```bash
forge init --template https://github.com/shaoguoji/forge-template
```

### 1. 环境配置

复制并配置环境变量：

```bash
cp .env.example .env
```

编辑 `.env` 文件：

```bash
ETHERSCAN_API_KEY=<你的 Etherscan API Key>
LOCAL_RPC_URL=http://127.0.0.1:8545
SEPOLIA_RPC_URL=https://1rpc.io/sepolia
```

### 2. 配置 Keystore 账户

使用 `cast wallet` 创建和管理加密的密钥库账户：

```bash
# 本地测试账户 (使用 Anvil 默认助记词)
cast wallet import anviltest --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80

# 导入真实账户 (用于 Sepolia)
cast wallet import shaoguoji --interactive
```

> 💡 Keystore 账户存储在 `~/.foundry/keystores/`，每次使用时需输入密码

### 3. 安装依赖

```bash
forge install
```

## 📖 使用指南

### Makefile 命令

```bash
# 查看帮助
make help

# 编译合约
make build

# 运行测试
make test

# 启动本地 Anvil 链
make anvil

# 部署到本地
make deploy local

# 部署到 Sepolia (带合约验证)
make deploy sepolia

# 清理构建产物
make clean
```

### 网络配置说明

| 网络    | RPC                     | 账户       | 合约验证 |
| ------- | ----------------------- | ---------- | -------- |
| local   | http://127.0.0.1:8545   | anviltest  | ❌       |
| sepolia | https://1rpc.io/sepolia | shaoguoji  | ✅       |

### 部署地址管理

部署脚本会自动保存合约地址到 `deployments/` 目录：

```
deployments/
├── Counter_31337.json    # 本地链 (chainId: 31337)
└── Counter_11155111.json # Sepolia (chainId: 11155111)
```

**保存地址** (`Deploy.s.sol` 中):
```solidity
_saveDeployment("Counter", address(counter));
```

**加载地址**:
```solidity
address counterAddr = _loadDeployedAddress("Counter");
```

## 🔧 自定义配置

### 添加新网络

1. 在 `.env` 添加 RPC URL：
   ```bash
   MAINNET_RPC_URL=https://eth.llamarpc.com
   ```

2. 在 `foundry.toml` 添加配置：
   ```toml
   [rpc_endpoints]
   mainnet = "${MAINNET_RPC_URL}"
   
   [etherscan]
   mainnet = { key = "${ETHERSCAN_API_KEY}" }
   ```

3. 在 `Makefile` 添加对应规则

### 添加新合约

1. 在 `src/` 创建合约
2. 在 `script/Deploy.s.sol` 添加部署逻辑
3. 使用 `make deploy local|sepolia` 部署

## 📚 依赖

- [Foundry](https://book.getfoundry.sh/) - 智能合约开发工具链
- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) - 安全的合约标准库

## 📄 License

MIT
