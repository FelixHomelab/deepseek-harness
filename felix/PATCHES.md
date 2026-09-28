# felix/patches：DSH 主程序的两处补丁（仅跟踪）

本分支**不用于部署**。DSH 是 monorepo，发布的 `@deepseek-ai/dsh` 只是薄 CLI，
真正代码在大量 `@deepseek-ai/*` 子包里；从源码重建整套子包既不可行也不可持续。
部署仍是：npm 固定版本 + Felix-Homelab 仓库里对应版本的补丁脚本在构建期打补丁；
补丁目标找不到时构建直接失败，强制人工复核。

- `patches/unlock-remote-settings.py`：放开客户端「浏览器 host 为 localhost
  才能编辑设置」的门控——平台经子域访问，用户需要能配置模型/提供商（服务端无校验）。
- `patches/allow-nonloopback-host.py`：把「拒绝绑定 0.0.0.0」改为由
  `DSH_ALLOW_NON_LOOPBACK=1` 环境变量门控（容器内独立网络 + 宿主回环发布 + 网关鉴权）。

同步：上游升级后重跑两个脚本；结构变化会报错，人工复核后更新补丁与 DSH 版本号。
