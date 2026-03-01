---
title: I2C - CH32V003
date: 2026-03-01
updated: 2026-03-01
---

[目次に戻る](index.md)

## I2Cインターフェース
Internal Integrated Circuit Bus (I2C) は、マイクロコントローラとセンサおよびその他のオフチップモジュール間の通信に広く使用される。

マルチマスタおよびマルチスレーブモードをサポートし、SDAおよびSCLの 2本のラインのみで 100kHz（標準）および 400kHz（高速）で通信可能である。タイミング制御、DMA、CRCチェックサム機能を備える。

### I2Cレジスタ
```c
USART1->STATR
USART1->DATAR
USART1->BRR
USART1->CTLR1
USART1->CTLR2
USART1->CTLR3
USART1->GPR
```
### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 [kyoutan.jpn.org](https://kyoutan.jpn.org)

[目次に戻る](index.md)
