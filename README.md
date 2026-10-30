# FotMob 原始响应 — 全量完整版（2844 行）

本仓库存放 `fotmob_raw_responses` 表**全部 2,844 行**的 `raw_response` 未截断完整数据。
是 `hd101666/fotmob-raw-full`（100 行版）的**全量补充**，两者并列、互不覆盖。

## 克隆

```bash
git clone https://github.com/hd101666/fotmob-raw-full-2844.git
```

## 文件

| 文件 | 大小 | 说明 |
|---|---|---|
| `fotmob_raw_responses_FULL_2844.csv` | 289,475,362 字节 | 2,844 行 × 20 列，`raw_response` 未截断 |
| `README_FULL_2844.md` | 5,814 字节 | 完整说明（分布、核对方法、安全扫描） |
| `.gitattributes` | 12 字节 | 锁定 CSV 按原始字节存储，禁止换行转换 |

> ⚠ **本仓库主文件是压缩包**，需解压后使用：
> ```bash
> unzip fotmob_raw_full_2844_20261030.zip
> # 得到 fotmob_raw_responses_FULL_2844.csv (289,475,362 字节) + README_FULL_2844.md
> ```
> ZIP SHA-256：`d293905a3a5c589e40aacc48e741ee286ff98a986b65c2aa5ef800e0feef6738`
> 上传原始 CSV(289MB) 受本机 0.26MB/s 带宽限制未直传，故以 ZIP 形式提供；
> **解压后 CSV 的 SHA-256 仍为 `9bb40a4f...2ed5`**，可用下方指标校验。

## 关键指标

| 指标 | 数值 |
|---|---|
| 总行数 | **2,844** |
| 列数 | **20** |
| 原始 CSV 大小 | 289,475,362 字节（289.48 MB / 276.07 MiB） |
| **原始 CSV SHA-256** | `9bb40a4fd9cc4a3588e445138d7f782757a1c242db8eb4630c33bbb39b362ed5` |
| raw_response 最大 | 1,396,215 |
| raw_response 最小 | 14,643 |
| raw_response 中位 | 41,532 |
| raw_response 均值 | 88,082 |

## 与 100 行版的关系

| 核验项 | 结果 |
|---|---|
| 列名 / 列顺序是否一致 | ✅ 完全一致（20 列全同） |
| **前 100 行是否逐行逐列一致** | ✅ **0 处差异** |
| 与仓库截断版样本对应 | ✅ 其他 19 列 0 差异，raw_response 前 600 字符 100/100 吻合 |

相关仓库：
- `hd101666/fotmob-raw-full` — 100 行完整版（14,656,358 字节）
- `hd101666/football-predictor` 分支 `hit-system-export` — 截断版样本

## 快速核对

```bash
git clone https://github.com/hd101666/fotmob-raw-full-2844.git
cd fotmob-raw-full-2844
python -c "
import csv,sys,hashlib,statistics
csv.field_size_limit(10**9)
p='fotmob_raw_responses_FULL_2844.csv'
raw=open(p,'rb').read()
print('bytes  ', len(raw))
print('sha256 ', hashlib.sha256(raw).hexdigest())
rows=list(csv.DictReader(open(p,encoding='utf-8-sig')))
print('rows   ', len(rows), 'cols', len(rows[0]))
L=[len(r['raw_response']) for r in rows]
print('max/min/median', max(L), min(L), int(statistics.median(L)))
"
```

期望输出：`bytes 289475362` / `sha256 9bb40a4f...2ed5` / `rows 2844 cols 20` / `max/min/median 1396215 14643 41532`

## 安全

- 无密钥、token、密码、cookie、`.env`
- 已分块全扫：真实凭据 0 命中、私钥/AWS/GitHub/Slack/Bearer 0 命中、JSON 内凭据类键名 0 命中
- `observation_id`（SHA-1）与 `raw_sha256`（SHA-256）为标识符/内容哈希，非凭据
- 内容为 FotMob 公开赛事统计，不含私人信息

## 注意

本仓库为**某一时点的全表快照**。若源数据库后续新增观测记录，行数会超过 2,844，
需重新导出并更新本仓库。
