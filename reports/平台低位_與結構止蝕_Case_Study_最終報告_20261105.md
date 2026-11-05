# 平台低位與結構止蝕 Case Study：最終報告

**日期：2026-11-05｜性質：RECORD-ONLY（重用樣本研究）｜正式 SOP、燈號、網站、總賬及正式統計不變。**

## 0. 一句結論

**今輪唔支持將 BASE-HOLD（低位守穩）或 PLATFORM（平台整理）加入做利好過濾，亦唔支持即時改用結構低位止蝕。** 兩個主形態喺三層樣本嘅 IN 每單 R 全部為負，而且全部跑輸各自 OUT；結構 SL 只有小幅、唔穩定嘅相對改善，改善後絕對 R 仍為負，並未通過右尾、成本、年份及股票依賴檢查。

呢個係「假設未通過」嘅研究結果，唔係失敗交付；反面證據已完整保留。由於呢輪冇做動態 SKIP-IN 政策，**亦唔可以將 IN 負 R 直接變成跳過訊號或反手造淡規則**。

## 1. 研究範圍與誠信

1. 固定唯讀庫《掃描器/data_4y/》，257 個 CSV 逐檔 SHA-256 與封存快照完全一致；分析硬截 2026-06-30。
2. DISCOVERY＝2022–2023；REUSED VALIDATION＝2024–2025；REUSED QUASI-OOS＝2026-01-01～2026-06-30。全部係重用材料，唔叫全新 unseen OOS。
3. 主成本 0.05ATR，另跑 0／0.02ATR。原 Limit、9→21EMA 開市切換、TP1 3R 賣半、B1、runner、Daily OHLC 保守觸價次序全部沿用。
4. 主成本 baseline 精確重現 6,519 張 CLOSED：1,950／3,569／1,000；最大 R 誤差只有浮點誤差，退出日完全相符。
5. 標籤 5 項原單元測試＋4 項邊界 regression tests 全過；另核對 confirmation≤T<RESET、最後 bar RESET、RESET 日成新 N、15 份動態模擬嘅逐股 overlap 時間線，以及 19 份模擬嘅 R／退出／實際 SL 一致性，全部通過。
6. 協議 SHA-256：`dd83b1fbeb2b55c5cd6fee6c5c937b1bedb738b745f27b518968406cf1ae4409`。結果前判定補充 SHA-256：`4ccd73a81db0f3f28f1bf8f545f0fa4dc72acde64f26e8f0cd6c0c0051e48852`。

## 2. SETUP（形態過濾）主結果

下表「IN／OUT 每單 R」只計 CLOSED；「IN−OUT 每候選 R」保留 MISS、CANCEL、OVERLAP 喺分母。
| 版本 | 時間層 | IN候選 | IN CLOSED | IN每單R | OUT每單R | IN−OUT每單R | IN−OUT每候選R |
|---|---|---|---|---|---|---|---|
| BASE_STRICT2 | DISCOVERY | 4,367 | 1,147 | -0.1356 | -0.0358 | -0.0998 | -0.0266 |
| BASE_STRICT2 | REUSED VALIDATION | 7,725 | 2,005 | -0.0700 | -0.0171 | -0.0529 | -0.0138 |
| BASE_STRICT2 | REUSED QUASI-OOS | 1,997 | 539 | -0.1000 | -0.0020 | -0.0980 | -0.0264 |
| BASE_STRICT2 | OVERALL | 14,089 | 3,691 | -0.0948 | -0.0200 | -0.0748 | -0.0197 |
| PLATFORM_STRICT2 | DISCOVERY | 3,509 | 898 | -0.1622 | -0.0368 | -0.1254 | -0.0319 |
| PLATFORM_STRICT2 | REUSED VALIDATION | 6,296 | 1,616 | -0.1200 | 0.0137 | -0.1336 | -0.0343 |
| PLATFORM_STRICT2 | REUSED QUASI-OOS | 1,518 | 395 | -0.1002 | -0.0252 | -0.0750 | -0.0191 |
| PLATFORM_STRICT2 | OVERALL | 11,323 | 2,909 | -0.1303 | -0.0076 | -0.1228 | -0.0315 |

**判讀：**

- BASE-HOLD 三層 IN 每單 R 為 **−0.1356／−0.0700／−0.1000**；PLATFORM 為 **−0.1622／−0.1200／−0.1002**。
- 兩者唔單止絕對負，亦每層跑輸 OUT；所以唔係「大盤或原方法本來就差，IN 無辜」咁簡單。
- OVERALL BASE IN 總 R＝−349.88；PLATFORM IN 總 R＝−379.09。兩個主版本均唔通過全部事前驗收。

### 2.1 成交率、TP1、B1 與 MFE（OVERALL，主成本）
| 版本 | 組別 | 成交率 | TP1率 | B1率 | 平均MFE(R) | 總R |
|---|---|---|---|---|---|---|
| BASE_STRICT2 | IN | 26.20% | 19.05% | 32.89% | 2.339 | -349.88 |
| BASE_STRICT2 | OUT | 25.81% | 20.58% | 32.39% | 2.425 | -56.46 |
| PLATFORM_STRICT2 | IN | 25.69% | 18.18% | 33.17% | 2.289 | -379.09 |
| PLATFORM_STRICT2 | OUT | 26.30% | 20.94% | 32.27% | 2.447 | -27.26 |

IN／OUT 成交率相近，因此負差異唔係單純由「IN 比較難成交」造成。IN 嘅 TP1 率及 MFE 亦較低，方向同每單 R 一致。

### 2.2 成本檢查（OVERALL）
| 版本 | 成本ATR | IN每單R | OUT每單R | IN−OUT每候選R |
|---|---|---|---|---|
| BASE_STRICT2 | 0.00 | 0.0182 | 0.0915 | -0.0188 |
| BASE_STRICT2 | 0.02 | -0.0270 | 0.0469 | -0.0192 |
| BASE_STRICT2 | 0.05 | -0.0948 | -0.0200 | -0.0197 |
| PLATFORM_STRICT2 | 0.00 | -0.0168 | 0.1038 | -0.0316 |
| PLATFORM_STRICT2 | 0.02 | -0.0622 | 0.0592 | -0.0316 |
| PLATFORM_STRICT2 | 0.05 | -0.1303 | -0.0076 | -0.0315 |

BASE 喺零成本下 IN 每單 R 雖為 +0.0182，但 OUT 為 +0.0915，仍明顯跑輸；加入成本後 IN 轉負。PLATFORM 即使零成本都係負。因此主結論唔係成本造成嘅幻覺。

### 2.3 全部十個形態版本（OVERALL，主成本）
| 版本 | IN候選 | IN CLOSED | IN每單R | OUT每單R | IN−OUT每候選R | 剔IN最佳1%後每單R |
|---|---|---|---|---|---|---|
| PLATFORM_CAP15 | 7,554 | 1,873 | -0.0751 | -0.0572 | -0.0034 | -0.1857 |
| BASE_STRICT2 | 14,089 | 3,691 | -0.0948 | -0.0200 | -0.0197 | -0.1958 |
| PLATFORM_CAP25 | 13,011 | 3,382 | -0.1082 | -0.0128 | -0.0248 | -0.2085 |
| BASE_WASHOUT2 | 15,069 | 3,971 | -0.0997 | -0.0040 | -0.0253 | -0.1978 |
| BASE_STRICT15 | 11,810 | 3,091 | -0.1173 | -0.0128 | -0.0274 | -0.2216 |
| PLATFORM_WASHOUT2 | 11,718 | 3,034 | -0.1270 | -0.0061 | -0.0313 | -0.2248 |
| PLATFORM_STRICT2 | 11,323 | 2,909 | -0.1303 | -0.0076 | -0.0315 | -0.2203 |
| PLATFORM_NONE | 12,135 | 3,146 | -0.1284 | -0.0007 | -0.0331 | -0.2247 |
| BASE_NONE | 16,593 | 4,382 | -0.1063 | 0.0278 | -0.0351 | -0.2027 |
| PLATFORM_STRICT15 | 10,017 | 2,585 | -0.1501 | -0.0046 | -0.0375 | -0.2450 |

冇任何版本達到事前驗收。剔 IN 最大 1% CLOSED 贏單後，全部 IN 每單 R 更負；呢個係保守右尾檢查，IN 原候選分母不變。

### 2.4 必須保留嘅局部正向反例：PLATFORM_CAP15（1.5ATR 寬度）
| 時間層 | IN每單R | OUT每單R | IN−OUT每候選R | 剔右尾IN每單R | 剔右尾差異 |
|---|---|---|---|---|---|
| DISCOVERY | -0.1707 | -0.0597 | -0.0275 | -0.2476 | -0.0462 |
| REUSED VALIDATION | -0.0386 | -0.0502 | 0.0038 | -0.1730 | -0.0287 |
| REUSED QUASI-OOS | 0.0155 | -0.0761 | 0.0250 | -0.1117 | -0.0062 |
| OVERALL | -0.0751 | -0.0572 | -0.0034 | -0.1857 | -0.0304 |

QUASI 的 IN 每單 R＝+0.0155、IN−OUT 每候選 R＝+0.0250，VALIDATION 差異亦略正；但 DISCOVERY IN＝−0.1707，OVERALL IN＝−0.0751，剔右尾後三層 IN 全負。**呢個係局部反例，唔係畀人事後揀 1.5ATR 做最佳參數嘅許可證。**

### 2.5 逐年與入場週
| 版本 | 入場年 | IN每單R | OUT每單R | IN−OUT每候選R |
|---|---|---|---|---|
| BASE_STRICT2 | 2022 | -0.2220 | -0.0628 | -0.0450 |
| BASE_STRICT2 | 2023 | -0.0978 | -0.0244 | -0.0191 |
| BASE_STRICT2 | 2024 | -0.0904 | -0.0030 | -0.0222 |
| BASE_STRICT2 | 2025 | -0.0510 | -0.0295 | -0.0055 |
| BASE_STRICT2 | 2026 | -0.1000 | -0.0020 | -0.0264 |
| PLATFORM_STRICT2 | 2022 | -0.2089 | -0.1106 | -0.0265 |
| PLATFORM_STRICT2 | 2023 | -0.1411 | -0.0059 | -0.0337 |
| PLATFORM_STRICT2 | 2024 | -0.1931 | 0.0680 | -0.0653 |
| PLATFORM_STRICT2 | 2025 | -0.0505 | -0.0341 | -0.0040 |
| PLATFORM_STRICT2 | 2026 | -0.1002 | -0.0252 | -0.0191 |

兩個主版本每一年 IN 每單 R 都係負，每一年 IN−OUT 每候選 R 都係負；呢個係最一致嘅反證。

| 版本 | OVERALL差異 | 入場週bootstrap 95%區間 | 單邊p(>0) | Holm p |
|---|---|---|---|---|
| BASE_STRICT2 | -0.0197 | -0.0448 ～ 0.0048 | 0.9375 | 1.0000 |
| PLATFORM_STRICT2 | -0.0315 | -0.0556 ～ -0.0071 | 0.9930 | 1.0000 |

BASE 整體區間仍跨零；PLATFORM 整體區間完全低於零。呢度只按事前定義描述區間，唔將非顯著當成證明無效，亦唔將 PLATFORM 負向直接變成造淡訊號。

## 3. STOP-ONLY（結構止蝕）動態主結果

主政策 `N_LOW_04`＝Low_N−0.4ATR_N；敏感性 `MIN3_LOW_04`＝三日最低 Low−0.4ATR_N。兩者均向下取整至美仙，同原 SL 報價習慣一致。所有原候選留喺分母，並按新退出日動態重算同股 overlap。
| 形態 | SL政策 | 時間層 | baseline每候選R | 政策每候選R | 改善 | 剔最大1%正改善後 |
|---|---|---|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | DISCOVERY | -0.0244 | -0.0193 | 0.0051 | -0.0014 |
| BASE_STRICT2 | N_LOW_04 | REUSED VALIDATION | -0.0121 | -0.0113 | 0.0008 | -0.0072 |
| BASE_STRICT2 | N_LOW_04 | REUSED QUASI-OOS | -0.0148 | 0.0018 | 0.0166 | 0.0091 |
| BASE_STRICT2 | N_LOW_04 | OVERALL | -0.0162 | -0.0118 | 0.0044 | -0.0028 |
| BASE_STRICT2 | MIN3_LOW_04 | DISCOVERY | -0.0244 | -0.0193 | 0.0051 | -0.0014 |
| BASE_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | -0.0121 | -0.0081 | 0.0041 | -0.0039 |
| BASE_STRICT2 | MIN3_LOW_04 | REUSED QUASI-OOS | -0.0148 | 0.0027 | 0.0175 | 0.0105 |
| BASE_STRICT2 | MIN3_LOW_04 | OVERALL | -0.0162 | -0.0099 | 0.0064 | -0.0008 |
| PLATFORM_STRICT2 | N_LOW_04 | DISCOVERY | -0.0244 | -0.0237 | 0.0008 | -0.0046 |
| PLATFORM_STRICT2 | N_LOW_04 | REUSED VALIDATION | -0.0121 | -0.0066 | 0.0056 | -0.0013 |
| PLATFORM_STRICT2 | N_LOW_04 | REUSED QUASI-OOS | -0.0148 | -0.0047 | 0.0101 | 0.0029 |
| PLATFORM_STRICT2 | N_LOW_04 | OVERALL | -0.0162 | -0.0114 | 0.0048 | -0.0015 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | DISCOVERY | -0.0244 | -0.0261 | -0.0017 | -0.0071 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | -0.0121 | -0.0024 | 0.0097 | 0.0034 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | REUSED QUASI-OOS | -0.0148 | -0.0136 | 0.0012 | -0.0058 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | OVERALL | -0.0162 | -0.0112 | 0.0050 | -0.0008 |

**核心判讀：**

- 四個政策 OVERALL 名義改善只有 **+0.0044～+0.0064R／原候選**，但 baseline OVERALL＝−0.0162R；改善後四個政策仍全部負 R。
- BASE 兩個政策喺 DISCOVERY／VALIDATION 主成本方向為正；PLATFORM_N_LOW_04 三層方向亦為正，但幅度小，且未過其他驗收。
- PLATFORM_MIN3_LOW_04 喺 DISCOVERY 已經係 −0.0017R，核心跨期一致性直接唔通過。
- 剔最大 1% 正改善貢獻後，OVERALL 四個政策全部轉負，顯示總改善依賴少數大幅改善個案。

### 3.1 絕對績效、尾部依賴與拒絕成交（OVERALL，主成本）
| 形態 | SL政策 | 政策每單R | TP1率 | INVALID數 | 原改善總R | 最大1%正改善貢獻 | 剔除後改善總R |
|---|---|---|---|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | -0.0436 | 13.10% | 618 | 110.93 | 180.14 | -69.22 |
| BASE_STRICT2 | MIN3_LOW_04 | -0.0365 | 13.02% | 558 | 159.39 | 178.29 | -18.90 |
| PLATFORM_STRICT2 | N_LOW_04 | -0.0430 | 16.00% | 695 | 119.87 | 158.19 | -38.32 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | -0.0421 | 15.82% | 625 | 126.02 | 146.72 | -20.70 |

原 baseline 每單 R＝−0.0623、TP1＝19.71%。政策每單 R 雖較唔負，但 TP1 率反而跌至 13.02%～16.00%；B1 比例亦明顯上升，例如 BASE_N_LOW 由 32.67% 升至 55.96%，PLATFORM_N_LOW 升至 47.04%。呢個表示結果混合咗新 R 分母、TP1 位置、B1 與退出日改變，唔係單純「同一批交易少蝕咗」。

`INVALID_STRUCTURE_SL` 係協議要求嘅正確處理：結構 SL 高過或等於適用 Limit 時唔硬塞成交。四個政策有 558～695 個呢類候選，全部保留喺原候選分母。

### 3.2 成本敏感性：D／V／Q 每原候選改善
| 形態 | SL政策 | 成本ATR | DISCOVERY / VALIDATION / QUASI |
|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | 0.00 | 0.0015 / -0.0031 / 0.0106 |
| BASE_STRICT2 | N_LOW_04 | 0.02 | 0.0029 / -0.0015 / 0.0130 |
| BASE_STRICT2 | N_LOW_04 | 0.05 | 0.0051 / 0.0008 / 0.0166 |
| BASE_STRICT2 | MIN3_LOW_04 | 0.00 | 0.0003 / -0.0013 / 0.0112 |
| BASE_STRICT2 | MIN3_LOW_04 | 0.02 | 0.0022 / 0.0009 / 0.0137 |
| BASE_STRICT2 | MIN3_LOW_04 | 0.05 | 0.0051 / 0.0041 / 0.0175 |
| PLATFORM_STRICT2 | N_LOW_04 | 0.00 | 0.0023 / 0.0069 / 0.0085 |
| PLATFORM_STRICT2 | N_LOW_04 | 0.02 | 0.0017 / 0.0063 / 0.0091 |
| PLATFORM_STRICT2 | N_LOW_04 | 0.05 | 0.0008 / 0.0056 / 0.0101 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | 0.00 | -0.0011 / 0.0079 / 0.0075 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | 0.02 | -0.0014 / 0.0086 / 0.0050 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | 0.05 | -0.0017 / 0.0097 / 0.0012 |

只有 PLATFORM_N_LOW_04 喺三檔成本、三層都保持正方向，但佢仍未通過右尾及年份／股票依賴，而且 OVERALL bootstrap 區間跨零，所以最多叫**弱改善線索**。BASE 兩個政策喺較低成本嘅 VALIDATION 出現反向；PLATFORM_MIN3 喺 DISCOVERY 三檔成本都反向。

### 3.3 最佳年份／股票依賴
| 形態 | SL政策 | 時間層 | 剔除項 | 剔除對象 | 剔除後每候選改善 |
|---|---|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | DISCOVERY | 最佳年份 | 2022 | -0.0011 |
| BASE_STRICT2 | N_LOW_04 | DISCOVERY | 最佳股票 | NUE | 0.0035 |
| BASE_STRICT2 | N_LOW_04 | REUSED VALIDATION | 最佳年份 | 2024 | -0.0039 |
| BASE_STRICT2 | N_LOW_04 | REUSED VALIDATION | 最佳股票 | GOOG | -0.0006 |
| BASE_STRICT2 | MIN3_LOW_04 | DISCOVERY | 最佳年份 | 2022 | -0.0006 |
| BASE_STRICT2 | MIN3_LOW_04 | DISCOVERY | 最佳股票 | NUE | 0.0036 |
| BASE_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | 最佳年份 | 2024 | 0.0017 |
| BASE_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | 最佳股票 | FIX | 0.0028 |
| PLATFORM_STRICT2 | N_LOW_04 | DISCOVERY | 最佳年份 | 2022 | -0.0003 |
| PLATFORM_STRICT2 | N_LOW_04 | DISCOVERY | 最佳股票 | FERG | -0.0006 |
| PLATFORM_STRICT2 | N_LOW_04 | REUSED VALIDATION | 最佳年份 | 2024 | 0.0027 |
| PLATFORM_STRICT2 | N_LOW_04 | REUSED VALIDATION | 最佳股票 | GOOG | 0.0042 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | DISCOVERY | 最佳年份 | 2022 | -0.0030 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | DISCOVERY | 最佳股票 | FERG | -0.0029 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | 最佳年份 | 2024 | 0.0066 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | REUSED VALIDATION | 最佳股票 | CLS | 0.0086 |

BASE_N_LOW 剔除 2022 後 DISCOVERY 轉負，剔除 2024 後 VALIDATION 轉負；PLATFORM_N_LOW 剔除 2022 或最佳股票 FERG 後，DISCOVERY 亦轉負。呢啲係事前否決條件，唔可以只用 OVERALL 正數掩蓋。

### 3.4 入場週 bootstrap（OVERALL）
| 形態 | SL政策 | 每候選改善 | 95%區間 | 單邊p(>0) |
|---|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | 0.0044 | -0.0060 ～ 0.0140 | 0.1814 |
| BASE_STRICT2 | MIN3_LOW_04 | 0.0064 | -0.0039 ～ 0.0163 | 0.1084 |
| PLATFORM_STRICT2 | N_LOW_04 | 0.0048 | -0.0027 ～ 0.0124 | 0.1089 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | 0.0050 | -0.0031 ～ 0.0129 | 0.1119 |

四個 OVERALL 區間全部跨零，冇穩健統計確認。VALIDATION 個別政策嘅局部細 p 值唔可以取代跨期、右尾與成本驗收。

### 3.5 靜態 matched-trade（只作機制拆解）
| 形態 | SL政策 | 靜態每原成交R改善 | 靜態總改善R | 動態每原候選R改善 | 動態總改善R |
|---|---|---|---|---|---|
| BASE_STRICT2 | N_LOW_04 | 0.0200 | 130.08 | 0.0044 | 110.93 |
| BASE_STRICT2 | MIN3_LOW_04 | 0.0275 | 179.07 | 0.0064 | 159.39 |
| PLATFORM_STRICT2 | N_LOW_04 | 0.0192 | 125.35 | 0.0048 | 119.87 |
| PLATFORM_STRICT2 | MIN3_LOW_04 | 0.0202 | 131.70 | 0.0050 | 126.02 |

靜態分母係原 baseline 6,519 張 CLOSED；動態分母係 25,048 個原候選，兩者唔可以直接當同一口徑比大小。靜態正數只說明結構 SL 確有改變退出機制，唔能推翻動態右尾與穩健性否決。

## 4. 最終驗收與實際行動

| 研究項 | 判定 | 主要原因 |
|---|---|---|
| BASE-HOLD SETUP | **不通過** | 三層 IN 負、跑輸 OUT、逐年負、右尾負 |
| PLATFORM SETUP | **不通過** | 同上；OVERALL 負差異更明顯 |
| BASE＋N_LOW_04 | **不通過；弱相對改善** | 主成本 D／V 正，但低成本反向、右尾及最佳年份依賴 |
| BASE＋MIN3_LOW_04 | **不通過；弱相對改善** | 主成本 D／V 正，但低成本、右尾及年份依賴未過 |
| PLATFORM＋N_LOW_04 | **不通過；四項中較一致嘅弱線索** | 三成本三層正，但右尾、最佳年份／股票及 bootstrap 未過 |
| PLATFORM＋MIN3_LOW_04 | **不通過** | DISCOVERY 核心改善已負，右尾及成本未過 |

**而家應做：**

1. 正式 SOP、燈號、網站、總賬及倉位規則全部唔改；新諗法只留停車場。
2. 可將 BASE／PLATFORM 當前瞻「風險標記」繼續記錄，但唔係已驗證 skip 條件，更唔係反手造淡依據。
3. 唔因 CAP15 局部正數而掃參數；如要再研究，應另起協議解釋點解平台內候選較弱，例如結構年齡、距離 structure high、前序趨勢與訊號位置，而唔係事後揀最佳寬度。
4. EARLY-ENTRY（提早進場）與突破後 9EMA Buffer 仍未測，維持獨立協議要求；今輪結果唔畀佢哋背書。
5. 已建立空白前瞻觀察表，由 2026-11-05 之後真實見到嘅訊號開始記，唔倒填 BT 樣本。

## 5. 限制

- 固定股票池、日線機械代理及原成交模型只係研究近似，唔等於實盤可複製報價；重用樣本亦增加資料探勘風險。
- Daily OHLC 同時觸價沿用原保守次序；新 SL 改變 R 分母、TP1、B1、退出日及後續 overlap，所以應以動態每原候選 R 為主。
- RVOL 只係相對過去 20 日成交量中位數，唔代表真正吸籌／派發；平台定義亦唔等於主觀圖形辨識。
- bootstrap 以入場週處理橫截面群聚，但唔能消除固定池、重用數據及模型設定風險。
- 負向 SETUP 觀察未經動態 SKIP-IN 或 SHORT 政策驗證，唔應直接變交易動作。

## 6. 可重用檔案

- `主結果總表.csv`：SETUP 與 STOP 主口徑合併表。
- `analysis_tables/`：全部十版本、三成本、逐年、逐股、逐週、右尾、bootstrap、Holm、動態／靜態及驗收表。
- `candidate_labels.csv`、`structure_episodes.csv`：全部候選標籤與結構 episodes。
- `simulations/`：baseline、四個動態政策、三檔成本及主成本靜態 matched-trade。
- `label_audit.json`、`label_regression_audit.json`、`backtest_audit.json`、`integrity_audit.json`：測試與獨立審計。
- `前瞻觀察表.csv` 及使用說明：只供本研究後前瞻記錄。

**結論再講一次：呢輪價值係否定咗一個睇落合理嘅圖形直覺，避免將成功圖表嘅事後幾何當成正期望。保留觀察，唔改實盤規則。**
