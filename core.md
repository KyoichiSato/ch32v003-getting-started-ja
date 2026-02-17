---
title: プロセッサコア - CH32V003
date: 2026-02-17
updated: 2026-02-17
---

[目次に戻る](index.md)

## プロセッサコアの機能
詳細は QingKeV2 マイクロプロセッサマニュアルに書いてある。

### システムタイマー (SysTick)
- 32ビット加算カウンタ（SysTick）
- システムクロック (HCLK) またはシステムクロックの8分周を選択可能
- リセット後は HCLK / 8
- コンペアレジスタあり
- コンペアマッチでオートリロードあり

* HCLKが 24MHzなら (1 / 24000000) * 0xFFFFFFFF = 約178秒でラップアラウンド
* 24MHz /8 なら 約1431秒 = 約23分51秒 

#### レジスタ
`core_riscv.h` では、SysTickレジスタ群は構造体として定義されていて、各レジスタには以下のような C言語の識別子が割り当てられている

```c
SysTick->CTLR
SysTick->SR
SysTick->CNT
SysTick->CMP
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
