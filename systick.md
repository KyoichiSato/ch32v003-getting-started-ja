---
title: SysTick - CH32V003
date: 2026-02-17
updated: 2026-02-18
---

[目次に戻る](index.md)


## システムタイマー (SysTick)
- 32ビット加算カウンタ（SysTick）
- システムクロック (HCLK) またはシステムクロックの8分周を選択可能
- リセット後は HCLK / 8
- コンペアレジスタあり
- コンペアマッチでオートリロードあり

* クロックが 24MHzなら (1 / 24000000) * 0xFFFFFFFF = 約178秒でラップアラウンド  
  1tick 約41.7ns
* クロックが 24MHz /8 なら 約1431秒 = 約23分51秒  
  1tick 約333.3ns

SysTickは CPUコアの機能なので、詳細はリファレンスマニュアルではなく QingKeV2 マイクロプロセッサマニュアルに書いてある。

### レジスタ
`core_riscv.h` では、SysTickレジスタ群は構造体として定義されていて、各レジスタには以下のような C言語の識別子が割り当てられている。

```c
SysTick->CTLR
SysTick->SR
SysTick->CNT
SysTick->CMP
```

```
CTLR コントロールレジスタ 初期値 0x00000000
[   31] SWIE  RW ソフトウェア割り込み制御ビット 1:SWI発生 0:無効
[30: 4]       RO Reserved
[    3] STRE  RW オートリロード有効化ビット 1:コンペアマッチでカウンタクリアする 0:しない
[    2] STCLK RW クロックソース選択ビット 1:HCLK 0:HCLK/8
[    1] STIE  RW カウンタ割り込み有効化ビット 1:有効 0:無効
[    0] STE   RW SysTick制御ビット 1:SysTick動作 0:SysTick停止
ソフトウェア割り込み : SWIEに1を書き込むと SysTick割り込みが発生
                     （任意のタイミングでタスクスイッチするなどの用途）
カウンタ割り込み     : カウンタが 0になった時点で割り込み

SR ステータスレジスタ 初期値 0x00000000
[31: 1]           Reserved
[    0] CNTIF RW0 コンペアマッチフラグ 1:CNTとCMPが一致した 0:一致していない
フラグはソフトウェアでクリアする
コンペアマッチ割り込みは無し
STREが 1ならコンペアマッチでカウンタが 0になるので割り込み発生

CNTR カウンタレジスタ 初期値 0x00000000
[31: 0] CNTR  RW 32bitのカウンタレジスタ

CMPR コンペアレジスタ 初期値 0x00000000
[31: 0] CMPR  RW 32bitのコンペアレジスタ
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
