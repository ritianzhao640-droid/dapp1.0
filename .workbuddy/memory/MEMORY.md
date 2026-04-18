# 长期记忆

## 项目：MEYieldVault DApp (ZRTA)
- **仓库**: https://github.com/ritianzhao640-droid/zrta.git (main 分支)
- **链**: BNB Chain
- **代币**: REMIX (0xb8bf1754c7b102b6845f55d6e2f463b838bc777)
- **金库合约**: 0xe515D4e904F0e2127a1e82E53F1Ec86a03635979
- **Lens 合约**: 0x8fD9d57fb12C7548d5fDfAdF828c40B418cA0152b
- **BurnReferralDistributor**: 0x4d2e6eAAbaA95610136562ff59463d47Fd836620

## 合约架构（已验证）
```
交易税(BNB) → Vault → 自动质押到 Lista → 获得 slisBNB
                              ↓
        营销钱包(40%)  |  燃烧池(40%)  |  持币分红(20% slisBNB)  |  持币分红(1% WBNB, 独立合约)
```

## 关键接口映射（2026-04-18 确认）
| 功能 | 合约 | 方法 |
|------|------|------|
| slisBNB 持币分红(20%) 领取 | Vault | `vault.claim()` |
| slisBNB 持币分红查询 | Vault | `vault.pendingClaim(user)` |
| WBNB 持币分红(1%) 领取 | holderDividendContract | `divContract.claim()` |
| WBNB 持币分红查询 | holderDividendContract | `divContract.pendingOf(user)` |
| 燃烧分红领取 | Vault | `vault.claimBurnReward()` |
| 燃烧分红查询 | Vault | `vault.pendingBurnReward(user)` |
| 邀请返佣领取 | Vault | `vault.claimInviteReward()` |
| 邀请返佣查询 | Vault | `vault.pendingInviteReward(user)` |
| WBNB 分红合约地址 | Vault | `vault.holderDividendContract()` |

## 已知问题与解决方案
- **Lens 缓存延迟**：质押/销毁后 Lens 数据不会立即更新，需要延迟刷新（5s + 12s 两级重试）
- **BscScan WebFetch 可用**：agent-browser 未安装，但可用 WebFetch 抓取 BscScan 页面获取合约接口信息
