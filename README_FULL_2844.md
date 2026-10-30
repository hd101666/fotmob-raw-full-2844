# FotMob 原始响应 全量完整版数据包（2844 行）

生成时间：2026-10-30
数据源：`sports_data.db` → `fotmob_raw_responses`（全表）
用途：提供该表**全部行**的 `raw_response` 未截断完整版，供核对

---

## 一、包内文件

| 文件 | 大小 | 说明 |
|---|---|---|
| `fotmob_raw_responses_FULL_2844.csv` | 289,475,362 字节 | 2844 行 × 20 列，`raw_response` 未截断 |
| `README_FULL_2844.md` | 本文件 | 说明 |

**本包不含**：密钥、token、密码、cookie、`.env` 文件。
**未推送** GitHub，未改动 `football-predictor` 与 `hit-system-export`，未覆盖 100 行版截断样本和完整包。

---

## 二、规模与校验值

| 指标 | 数值 |
|---|---|
| **总行数** | **2,844**（不含表头） |
| **列数** | **20** |
| **原始 CSV 未压缩大小** | **289,475,362 字节**（289.48 MB / 276.07 MiB） |
| **压缩包大小** | **29,550,809 字节**（29.55 MB / 28.18 MiB） |
| 压缩率 | 10.2% |
| **原始 CSV SHA-256** | `9bb40a4fd9cc4a3588e445138d7f782757a1c242db8eb4630c33bbb39b362ed5` |

> 包内 CSV 以 UTF-8 with BOM 编码，CRLF 换行。SHA-256 针对上述原始字节计算。

---

## 三、行数核对

| 核对项 | 结果 |
|---|---|
| 数据库 `COUNT(*)` | 2,844 |
| 数据库 `COUNT(observation_id)` | 2,844（无 NULL） |
| 数据库 `COUNT(DISTINCT observation_id)` | 2,844（无重复） |
| 数据库 rowid 范围 | 1 ~ 2844（连续无空洞） |
| 导出 CSV 实际行数 | 2,844 |
| raw_response 为空的行 | 0 |

**结论：导出 2,844 行，与数据库表行数完全一致，未截断、未补行。**

导出顺序为 `ORDER BY rowid`（即表默认插入顺序），与 100 行版使用的 `LIMIT 100` 顺序一致。

---

## 四、raw_response 长度分布（2844 行）

| 统计量 | 字符数 |
|---|---|
| **最大** | **1,396,215** |
| **最小** | **14,643** |
| **中位数** | **41,532** |
| 均值 | 88,082 |
| 总和 | 250,506,618 |

分位分布：

| 分位 | 长度 |
|---|---|
| P10 | 21,780 |
| P25 | 31,218 |
| P50（中位） | 41,532 |
| P75 | 59,782 |
| P90 | 210,996 |
| P99 | 795,795 |

分布特征：主体集中在 3~6 万字符，但尾部极长。12 行超过 100 万字符，
长度最大的 10 行占全部字符量的 4.9%。

---

## 五、20 列清单（与 100 行版完全一致）

| # | 列名 |
|---|---|
| 1 | observation_id |
| 2 | provider |
| 3 | endpoint |
| 4 | source_match_id |
| 5 | source_league_id |
| 6 | request_url |
| 7 | request_started_at_utc |
| 8 | observed_at_utc |
| 9 | http_status |
| 10 | etag |
| 11 | last_modified |
| 12 | content_type |
| 13 | source_version |
| 14 | source_version_note |
| 15 | raw_sha256 |
| 16 | response_bytes |
| 17 | raw_response |
| 18 | first_seen_utc |
| 19 | last_seen_utc |
| 20 | observation_count |

---

## 六、与 100 行版的关系（已验证）

| 核验项 | 结果 |
|---|---|
| 列名 / 列顺序是否与 100 行版一致 | ✅ 完全一致（20 列全同） |
| **前 100 行是否与 100 行完整版逐行逐列一致** | ✅ **完全一致，0 处差异**（100 行 × 20 列全比对） |
| 前 100 行是否与仓库截断版对应 | ✅ 其他 19 列 0 差异；raw_response 前 600 字符 100/100 吻合 |

即：本包前 100 行 = `fotmob-raw-full` 仓库里的那份 100 行完整版，一字不差。
全量包在该基础上向后延续至第 2844 行。

核对脚本：

```python
import csv, sys
csv.field_size_limit(10**9)
new = list(csv.DictReader(open('fotmob_raw_responses_FULL_2844.csv', encoding='utf-8-sig')))
old = list(csv.DictReader(open('fotmob_raw_responses_FULL.csv',      encoding='utf-8-sig')))
assert len(new) == 2844 and len(old) == 100
for i in range(100):
    for c in new[0]:
        assert str(new[i][c] or '') == str(old[i][c] or ''), (i, c)
print('前100行完全一致')
```

---

## 七、安全扫描结果

对 289MB CSV 分块扫描，结果均为 **0 命中**：

| 扫描项 | 结果 |
|---|---|
| `.env` 中全部 KEY/TOKEN/SECRET 真实值比对 | 0 命中 |
| 3 个未纳管 Odds API key 比对 | 0 命中 |
| 私钥 / AWS AKIA / GitHub token / Slack token / Bearer | 0 命中 |
| 硬编码 api_key/secret/password 赋值 | 0 命中 |
| Qiety sessionId 形态（517383 开头 28+ 位） | 0 命中 |
| `set-cookie` 响应头 | 0 命中 |
| JSON 内凭据类键名（token/api_key/secret/password/authorization/cookie/session_id/credentials/client_secret 等） | 0 命中 |

非敏感标识符（已确认可保留）：
- `observation_id`：40 位 SHA-1 观测标识
- `raw_sha256`：64 位 SHA-256 内容哈希
- `etag`：FotMob 弱 ETag，形如 `W/"xxxxx"`

数据内容为 FotMob 公开赛事统计（球员姓名、球队、比赛事件、天气等），不含私人信息。

---

## 八、关于上传 GitHub

**本包未上传**，按要求放在本机压缩包中供你先决定。

供参考的客观数据：
- 压缩后 29.55 MB，**低于** GitHub 单文件 50MB 警告线、100MB 硬上限
- 现有 `fotmob-raw-full` 仓库（100 行版）保持不动

可选方案（需你同意后再做）：

| 方案 | 说明 |
|---|---|
| A. 新建独立仓库 | 如 `fotmob-raw-full-2844`，与 100 行版仓库并列，互不影响 |
| B. 现有仓库加子目录 | 在 `fotmob-raw-full` 下加 `full-2844/` 子目录，100 行版文件不覆盖，但会改动该仓库 |
| C. 按行分包 | 若担心单文件过大，可拆为多个 10MB 左右的 CSV + manifest |
| D. 不传 GitHub | 仅用本机压缩包或网盘传递 |

推荐 A：隔离最彻底，且 29.55MB 单文件在 GitHub 完全可接受。

---

## 九、注意

- 本包是全表快照，与数据库当前状态一致（2,844 行）
- 若数据库后续新增观测记录，需重新导出
- 包内只有 1 个 CSV + 1 个说明，无其他文件
