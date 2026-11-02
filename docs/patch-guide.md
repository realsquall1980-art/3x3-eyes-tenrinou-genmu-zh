# 安装指南（v7.3）

补丁是 **UPS 一步差异补丁**：直接打在**日版原盘的 Track 1.bin** 上，不需要任何中间版本。
三碟各有自己的补丁，不能混用。

| 碟 | 补丁 | 大小 | 对应原盘 | 底包 CRC32 |
|---|---|---|---|---|
| Disc 1 | `3x3Eyes_Disc1_zh_v73.ups` | 224 MiB | SLPS-01497 Track 1 | `9880cffa` |
| Disc 2 | `3x3Eyes_Disc2_zh_v73.ups` | 316 MiB | SLPS-01498 Track 1 | `be2ecda1` |
| Disc 3 | `3x3Eyes_Disc3_zh_v73.ups` | 356 MiB | SLPS-01499 Track 1 | `9e61224f` |

补丁在仓库 `patches/v73/` 下，**按分卷 zip 存放**（单文件超过 GitHub 仓库 100 MB 限制）：

- **Windows**：用 7-Zip 打开 `Disc1.ups.zip.001`，直接「提取」（7-Zip 会自动拼后续分卷）
- **macOS / Linux**：
  ```bash
  cd patches/v73
  cat Disc1.ups.zip.00* > Disc1.ups.zip && unzip Disc1.ups.zip
  ```

每碟解出一个 `.ups`，就是补丁本体。

## 一、准备底包

1. 取得 **Redump 日版**三碟，每碟目录里通常有 `Track 1.bin`（数据轨）+ `Track 2..n.bin`（音轨）+ `.cue`。
2. **只改 `Track 1.bin`**。音轨与 `.cue` 保持原样，不要动、不要重命名。
3. 底包校验（对不上就是拿错盘：别区版本、rip 版、或者已经打过补丁）：

| 碟 | CRC32 | SHA-1 | SHA-256 |
|---|---|---|---|
| Disc1 | `9880cffa` | `22c8248fae3346c5106cd14da14d9d4586119de5` | `070bd1ceaf49d7c04a7355d4924579b0ca7ae77c317a8354f747e043b60ef928` |
| Disc2 | `be2ecda1` | `c171f39c0da86bf6fe9e9e8d728b431ca060eadf` | `b8780cfa09ee928459a40287aaa499b1bf22b719834f8433402a542c4ef48350` |
| Disc3 | `9e61224f` | `c0db94774970fbc339dcbb22d30594bda6134cc4` | `a6391e48eb0ec77ba01d9560cc096b7da1b1966cc7da030a5404b3ed816c166f` |

## 二、打补丁

**Windows**：RomPatcher.js（网页版，拖进去即可）或 UPS Patcher
**macOS / Linux**：`flips`

```bash
flips --apply 3x3Eyes_Disc1_zh_v73.ups "Disc1/Track 1.bin" "Disc1/Track 1 (zh).bin"
# 打完后把结果覆盖回 Track 1.bin 的原名（先备份原文件）
```

打补丁工具一般会报三种结果：

- `The patch was applied successfully!` → 成功
- `This patch is not intended for this ROM` / `source file is invalid` → **底包不对**（版本不符或已打过补丁），不是补丁坏了
- `patch is already applied` → 这个盘已经打过了，不用重复打

打完可用 `SHA256SUMS.txt` 里 `work-bin/` 那三行核对成品（对应的是打好的 Track 1.bin）。

## 三、运行

1. 三碟都打好后，用模拟器直接打开 **Disc1 的 `.cue`**（不是 bin）。
2. 换盘：游戏提示换碟时，用模拟器的 Disk Control / 更换光盘功能**追加** Disc2、Disc3 的 `.cue`，不要关闭模拟器重开（会丢存档状态）。
3. 存档：本作存档在记忆卡镜像里。v4 以前的旧存档与新字表码位不兼容，存档槽说明栏会显示怪字 —— **重存一次（覆盖原槽）即恢复中文**，不必删档。

## 四、掌机（SwanStation / RetroArch 等）转 CHD

掌机前端多半只吃 `.chd`，用 `chdman` 从**打好补丁的 cue** 转：

```bash
chdman createcd -i "Disc1.cue" -o "Disc1.chd"
chdman info -i "Disc1.chd"     # 确认碟号与轨道信息
```

三碟各转一次，放进同一个游戏目录，元数据（`.m3u` / 前端配置）按前端要求写：

```
3x3eyes_zh.m3u
  Disc1.chd
  Disc2.chd
  Disc3.chd
```

仓库**不提供** CHD 成盘（等同于原版盘内容），只提供 UPS 补丁。

## 五、常见错误

| 症状 | 原因 |
|---|---|
| `This patch is not intended for this ROM` | 底包不是对应碟的 Redump 日版 Track 1（拿错碟 / 拿错版本 / 已打过补丁） |
| 补丁报成功，进游戏黑屏或停在开机画面 | 打到了音轨或整盘 bin，而不是 `Track 1.bin` |
| 游戏能进但没有中文 | 打的是别的碟的补丁，或模拟器加载了未打补丁的那份 cue |
| 存档槽说明栏是怪字 | 旧存档里的旧码位 —— 重存一次即恢复 |

## 六、旧版本

v6.9 及更早的补丁**已作废并删除**：它们的 UPS 编码不符合规范（`flips` 等常规工具一律报 `This patch is not intended for this ROM`），而且底包也不是日版原盘。请直接使用 v7.3。
