# Aegis-Matrix (神盾矩阵) 🛡️
**基于 OKX Onchain OS 的自愈型智能合约免疫与抢跑拦截系统**

## 1. 架构概述
Aegis-Matrix 并非传统的静态代码审计工具，而是一个部署在 Onchain OS 节点层的动态免疫系统。它通过 WebSocket 持续监听 Mempool (内存池) 中的 Pending 交易。当检测到与受保护金库的交互时，引擎会在本地 Fork 链上状态并进行毫秒级模拟执行。
如果大模型神经引擎判定该执行轨迹将导致严重的 TVL 流失（如零日漏洞攻击），系统将自动构建 `pause()` 交易，并以黑客 1.5 倍的 Gas Price 抢先打包，实现物理级拦截。

## 2. 核心模块
* **Mempool Scanner:** 基于 `web3.py` 的异步 WSS 订阅，捕获未确认交易。
* **Neuro-Symbolic Engine:** 本地状态模拟 (Dry-run) 结合大模型推理，识别恶意资金抽离。
* **Front-Running Executor:** MEV 级抢跑发单器，确保防御补丁先于攻击交易落地。

## 3. 快速复现指南 (Windows 环境)

### 步骤 A: 环境准备
1. 克隆仓库：`git clone https://github.com/hkai614119-star/aegis-matrix.git`
2. 进入目录：`cd aegis-matrix`
3. 安装依赖：`pip install -r requirements.txt`
4. 复制并配置 `.env` 文件，填入你的 OKX X Layer WSS 节点地址和测试钱包私钥。

### 步骤 B: 启动攻防演练靶场
打开终端 1，启动 Aegis 防御节点：
```bat
.\scripts\windows_start_node.bat
