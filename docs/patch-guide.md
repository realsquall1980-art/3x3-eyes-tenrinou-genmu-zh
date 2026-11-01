# 安装指南（v6.9）

补丁是 **UPS 差异补丁**，必须打在**日版原盘**上。三碟各有自己的补丁，不能混用。

| 碟 | 补丁 | 对应原盘 |
|---|---|---|
| Disc 1 | `3x3Eyes_Disc1_zh_v69.ups` | SLPS-01497 Track 1 |
| Disc 2 | `3x3Eyes_Disc2_zh_v69.ups` | SLPS-01498 Track 1 |
| Disc 3 | `3x3Eyes_Disc3_zh_v69.ups` | SLPS-01499 Track 1 |

## 一、准备底包

1. 取得 **Redump 日版**三碟，每碟目录里通常有 `Track 1.bin`（数据轨）+ `Track 2..n.bin`（音轨）+ `.cue`。
2. **只改 `Track 1.bin`**。音轨与 `.cue` 保持原样，不要动、不要重命名。
3. 底包校验：拿 Redump dat 里的 SHA-1 与手上的 `Track 1.bin` 对一遍；对不上就是拿错盘（别区版本、rip 版、被改过的盘）。

## 二、打补丁

**Windows**：UPS Patcher / RomPatcher.js（网页版，拖进去即可）
**macOS / Linux**：`flips`

```bash
# macOS 示例：三碟分别打
flips --apply 3x3Eyes_Disc1_zh_v69.ups "Disc1/Track 1.bin" "Disc1/Track 1 (zh).bin"
# 打完后把结果覆盖回 Track 1.bin 的原名（先备份原文件）
```

打补丁工具一般会报三种结果：

- `applied successfully` → 成功
- `source file is invalid` → **底包不对**（版本不符或已打过补丁），不是补丁坏了
- `patch is already applied` → 这个盘已经打过了，不用重复打

## 三、运行

1. 三碟都打好后，用模拟器直接打开 **Disc1 的 `.cue`**（不是 bin）。
2. 换盘：游戏提示换碟时，用模拟器的 Disk Control / 更换光盘功能**追加** Disc2、Disc3 的 `.cue`，不要关闭模拟器重开（会丢存档状态）。
3. 存档：本作存档在记忆卡镜像里。**v4 以前的旧存档会乱码**（新字表码位不同），首次进入请删旧档重开。

## 四、掌机（SwanStation / RetroArch 等）转 CHD

掌机前端多半只吃 `.chd`，用 `chdman` 从**打好补丁的 cue** 转：

```bash
chdman createcd -i "Disc1.cue" -o "Disc1.chd"
chdman info -i "Disc1.chd"     # 确认碟号与轨道信息
```

三碟各转一次，放进同一个游戏目录，元数据（.m3u / 前端配置）按前端要求写：

```
3x3eyes_zh.m3u
  Disc1.chd
  Disc2.chd
  Disc3.chd
```

仓库**不提供** CHD 成盘（等同于原版盘内容），只提供 UPS。

## 五、常见错误

| 现象 | 原因 | 处置 |
|---|---|---|
| 补丁工具报 `source file is invalid` | 底包不是该碟的日版 Track 1，或用了合并整盘 bin | 换回 Redump 原版对应碟的 Track 1 |
| 打完仍显示日文 | 打错碟（Disc2 补丁打到 Disc1） | 按上表逐碟核对 |
| 进游戏黑屏、无音乐 | 只保留了 Track 1，音轨或 `.cue` 被改 | 保留原盘音轨与 cue，用 cue 启动 |
| 换碟后卡住 | 用「重新打开游戏」代替了换盘 | 用 Disk Control 追加下一碟 |
| 旧存档乱码 | v4 以前存档码位不匹配 | 删旧档，重开新档 |
| 人物介绍/暂停菜单空白 | 用的是 v6.4 及以前版本 | 换 v6.9 |
| 下载卡在几百 KB | GitHub 文件域名国内线路问题 | 链接前加 `https://gh-proxy.com/` 再下 |

## 六、下载疑难（中国大陆）

Release 资产走 `objects.githubusercontent.com`，国内直连常见「几 KB/s 后断流」。把链接改写为：

```
https://gh-proxy.com/https://github.com/realsquall1980-art/3x3-eyes-tenrinou-genmu-zh/releases/download/v6.9/3x3Eyes_Disc1_zh_v69.ups
```

下完先 `shasum -a 256` 对一遍 [SHA256SUMS.txt](../SHA256SUMS.txt)，再打补丁——代理站偶尔会吐残缺文件，校验能挡住。
