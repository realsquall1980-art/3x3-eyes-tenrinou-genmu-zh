# 补丁位置（v7.3）

三个 UPS 补丁单文件 224 / 316 / 356 MiB，超过 GitHub 仓库 100 MiB 单文件上限，所以切成 `v73/*.zip.00N` 分卷存放。

- **Windows**：7-Zip 打开 `Disc1.ups.zip.001` → 提取（自动拼后续分卷）
- **macOS / Linux**：`cat Disc1.ups.zip.00* > Disc1.ups.zip && unzip Disc1.ups.zip`

拼好后可用仓库根目录 `SHA256SUMS.txt` 核对补丁 SHA-256。

旧的 v6.9 补丁已删除：UPS 编码不符合规范（`flips` 等常规工具一律报 `This patch is not intended for this ROM`），且底包不是日版原盘，任何人都装不上。
