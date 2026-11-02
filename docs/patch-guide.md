# 安装指南（v1.1）

补丁是 **UPS 一步差异补丁**：直接打在**日版原盘的 Track 1.bin** 上，不需要任何中间版本，也不需要先装别的补丁。
三碟各有自己的补丁，**不能混用**，也不能跨碟打。

| 碟 | 补丁 | 大小 | 对应原盘 | 底包 CRC32 |
|---|---|---|---|---|
| Disc 1 | `3x3Eyes_Disc1_zh_v1.1.ups` | 234,993,256 B（224.1 MiB） | SLPS-01497 Track 1 | `9880cffa` |
| Disc 2 | `3x3Eyes_Disc2_zh_v1.1.ups` | 331,089,513 B（315.8 MiB） | SLPS-01498 Track 1 | `be2ecda1` |
| Disc 3 | `3x3Eyes_Disc3_zh_v1.1.ups` | 372,847,304 B（355.6 MiB） | SLPS-01499 Track 1 | `9e61224f` |

补丁在仓库 **Releases → v1.1** 下，是单个文件直下，不需要拼分卷：

```
https://github.com/realsquall1980-art/3x3-eyes-tenrinou-genmu-zh/releases/download/v1.1/3x3Eyes_Disc1_zh_v1.1.ups
https://github.com/realsquall1980-art/3x3-eyes-tenrinou-genmu-zh/releases/download/v1.1/3x3Eyes_Disc2_zh_v1.1.ups
https://github.com/realsquall1980-art/3x3-eyes-tenrinou-genmu-zh/releases/download/v1.1/3x3Eyes_Disc3_zh_v1.1.ups
```

国内直连卡住时，把整个链接前面加 `https://gh-proxy.com/`：

```
https://gh-proxy.com/https://github.com/realsquall1980-art/3x3-eyes-tenrinou-genmu-zh/releases/download/v1.1/3x3Eyes_Disc1_zh_v1.1.ups
```

**下完先对 SHA-256**（仓库根目录 `SHA256SUMS.txt`）。半截损坏的补丁打上去，工具报的错和"底包不对"是同一句，很容易误判成补丁坏了。

## 一、准备底包

1. 取得**日版原盘**三碟，每碟目录里通常有 `Track 1.bin`（数据轨）、若干音轨与 `.cue`。
2. **只改 `Track 1.bin`**。音轨与 `.cue` 保持原样，不改名、不重排。
3. 底包校验（对不上就是拿错盘：别区版本、rip 版、或者这张盘已经打过补丁）：

| 碟 | CRC32 | SHA-1 | SHA-256 |
|---|---|---|---|
| Disc1 | `9880cffa` | `22c8248fae3346c5106cd14da14d9d4586119de5` | `070bd1ceaf49d7c04a7355d4924579b0ca7ae77c317a8354f747e043b60ef928` |
| Disc2 | `be2ecda1` | `c171f39c0da86bf6fe9e9e8d728b431ca060eadf` | `b8780cfa09ee928459a40287aaa499b1bf22b719834f8433402a542c4ef48350` |
| Disc3 | `9e61224f` | `c0db94774970fbc339dcbb22d30594bda6134cc4` | `a6391e48eb0ec77ba01d9560cc096b7da1b1966cc7da030a5404b3ed816c166f` |

原盘 `Track 1.bin` 大小：Disc1 517,244,784 B ／ Disc2 549,834,096 B ／ Disc3 611,214,240 B。

## 二、打补丁

**Windows**：RomPatcher.js（网页版，拖进去即可）或 UPS Patcher
**macOS / Linux**：`flips`

```bash
flips --apply 3x3Eyes_Disc1_zh_v1.1.ups "Disc1/Track 1.bin" "Disc1/Track 1 (zh).bin"
# 打完后把结果覆盖回 Track 1.bin 的原名（先备份原文件）
```

| 工具回话 | 意思 |
|---|---|
| `The patch was applied successfully!` | 成功 |
| `This patch is not intended for this ROM` ／ `source file is invalid` | **底包不对**（拿错碟／拿错版本／已打过补丁），不是补丁坏了 |
| `patch is already applied` | 已经打过了，不用重复 |

打完核对成品（打好的 Track 1.bin）：

| 成品 | 字节 | SHA-256 |
|---|---|---|
| Disc 1 成品 | 517,244,784 | `19f111c0ca22bdc54ecae63d75e974f0b25f8c534066f684c47d590c577940c4` |
| Disc 2 成品 | 549,834,096 | `5cf79fa0a6956c189b07d125b0314dbacbd22bd4babd55c92a47575bde9e91c0` |
| Disc 3 成品 | 611,214,240 | `8002d029089a132c25b7bae8a253dd1cc32844ece77bb81ed01ef2c7ff4beedd` |

## 三、运行

1. 三碟都打好后，用模拟器打开 **Disc 1 的 `.cue`**（不是 bin）。
2. 换盘：游戏提示换碟时，用模拟器的 Disk Control / 更换光盘**追加** Disc 2、Disc 3 的 `.cue`，不要关闭模拟器重开（会丢内存里的存档状态）。
3. 存档：本作存档写在记忆卡镜像里。早期版本留下的存档与新字表码位不兼容，存档槽下方说明会显示怪字 —— **覆盖重存一次即可恢复中文**，不必删档。

## 四、掌机 / 前端（CHD）

前端多半只吃 `.chd`，用 `chdman` 从**打好补丁的 cue** 转：

```bash
chdman createcd -i "Disc1.cue" -o "Disc1.chd"
chdman verify -i "Disc1.chd"        # 可选：完整性自检
```

三碟各转一次，放同一目录，用 `.m3u` 串起来（模拟器打开 m3u，不要打开单个 chd）：

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
| `This patch is not intended for this ROM` | 底包不是对应碟的日版 Track 1（拿错碟／别区版本／已打过补丁／补丁文件没下完整） |
| 打开 cue 直接黑屏 | 用的是没打补丁的 Track 1，或打开了 bin 而不是 cue |
| 文字整片空白、只剩图块字 | 补丁只打了一半（中途中断），重打一次；或补丁文件损坏，对 SHA-256 |
| 存档槽说明是怪字 | 早期存档的旧码位，覆盖重存一次 |
| 换碟失败 | 模拟器里没追加第二、三碟，或用了不同版本的 cue |

## 六、本版的范围与已知问题

见仓库根目录 [README.md](../README.md) 第三、六节。要点：选项页与「三只眼变身」图文精选仍是盘上**图片字**（本版未改）；存档槽缩略图未取得屏幕证据（下一版处理）。
