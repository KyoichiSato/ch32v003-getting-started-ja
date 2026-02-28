---
title: USART - CH32V003
date: 2026-02-28
updated: 2026-02-28
---

[目次に戻る](index.md)

## USART ユニバーサル同期/非同期受送信機

- 全二重 / 半二重 の 同期 / 非同期 通信に対応
- NRZ (Non-Return-to-Zero) データフォーマット
- 分数型ボーレートジェネレータ（最大3Mbps）
- データ長がプログラム可能
- ストップビットの構成可能
- LIN, IrDAエンコーダ, スマートカード対応
- DMA (Direct Memory Access) サポート
- 複数の割り込みソースに対応

### 概要
### USARTレジスタ
```c
USART1->STATR
USART1->DATAR
USART1->BRR
USART1->CTLR1
USART1->CTLR2
USART1->CTLR3
USART1->GPR
```
```
STATR ステータスレジスタ 初期値 0x000000c0
[31:10] RO Reserved
[    9] CTS  RW0 CTS状態変化フラグ        1:nCTS端子変化あり
[    8] LBD  RW0 LINブレイクフラグ        1:ブレイク検出
[    7] TXE  RO  送信データレジスタエンプティフラグ 1:送信データなし
[    6] TC   RW0 送信完了フラグ           1:送信完了
[    5] RXNE RW0 受信データありフラグ      1:受信データあり
[    4] IDLE RO  バスアイドルフラグ        1:アイドル状態
[    3] ORE  RO  オーバーロードエラーフラグ 1:受信データを読み出す前に次のデータを受信した
                 （後から受信したデータは捨てられる）
[    2] NE   RO  ノイズエラーフラグ         1:エラー
[    1] FE   RO  フレーミングエラーフラグ   1:エラー
[    0] PE   RO  パリティエラーフラグ       1:エラー

DATAR データレジスタ 初期値 0x000000xx
[31: 9]         RO Reserved
[ 8: 0] DR[8:0] RW データレジスタ
読み出しは受信データレジスタからの読み出し、書き込みは送信データレジスタへの書き込みになる。

BRR ボーレートレジスタ 初期値 0x00000000
[31:16]                    RO Reserved
[15: 4] DIV_Mantissa[11:0] RW 分周器の整数部
[ 3: 0] DIV_Fraction[3:0] RW 分周器の小数部

CTLR1 コントロールレジスタ1 初期値 0x00000000
[31:14] RO Reserved
[   13] UE     RW USARTイネーブル
                  0を書いた場合、現在の通信が完了してから動作が停止する
[   12] M      RW データ長設定 0:8bit 1:9bit
[   11] WAKE   RW ウェイクアップ選択 0:バスアイドル 1:アドレスマーカー
[   10] PCE    RW パリティイネーブル
[    9] PS     RW パリティ選択 0:偶数パリティ 1:奇数パリティ
[    8] PEIE   RW パリティエラー割り込みイネーブル
[    7] TXEIE  RW 送信データレジスタ空割り込みイネーブル
[    6] TCIE   RW 送信完了割り込みイネーブル
[    5] RXNEIE RW 受信データあり割り込みイネーブル
[    4] IDLEIE RW バスアイドル割り込みイネーブル
[    3] TE     RW 送信イネーブル
[    2] RE     RW 受信イネーブル
[    1] RWU    RW 受信ウェイクアップ
                  0:レシーバは通常動作モード
                  1:レシーバはサイレントモード
[    0] SBK    RW ブレイク送信ビット

CTLR2 コントロールレジスタ2 初期値 0x00000000
[31:15]           RO Reserved
[   14] LINEN     RW LINモードイネーブル
[13:12] STOP[1:0] RW ストップビット設定
                     00:1    10: 2
                     01:0.5  11: 1.5
[   11] CLKEN     RW クロックイネーブル
[   10] CPOL      RW クロック極性 0:アイドル時 CK=0 1:アイドル時 CK=1
[    9] CPHA      RW クロック位相 0:最初のCK偏移でデータキャプチャ開始 1:2番目のCK偏移でキャプチャ
[    8] LBCL      RW 最終ビットクロックパルス出力 0:しない 1:する
[    7]           RO Reserved
[    6] LBDIE     RW LINブレイク割り込みイネーブル
[    5] LBDL      RW LINブレイク検出長 0:10bit 1:11bit
[    4]           RO Reserved
[ 3: 0] ADD[3:0]  RW USARTノードアドレス
                     マルチプロセッサ通信で使用する本機のアドレスで、アドレスマークによるウェイクアップに使用される。

CPOL=0, CPHA=0 _____+--+__+--+__
CPOL=0, CPHA=1 __+--+__+--+__+--
CPOL=1, CPHA=0 -----+__+--+__+--
CPOL=1, CPHA=1 --+__+--+__+--+__
                    ^     ^
                    +-----+-- データキャプチャ

CTLR3 コントロールレジスタ3 初期値 0x00000000
[31:11]       RO Reserved
[   10] CTSIE RW CTS割り込みイネーブル
[    9] CTSE  RW CTSイネーブル
[    8] RTSE  RW RTSイネーブル
[    7] DMAT  RW 送信DMAイネーブル
[    6] DMAR  RW 受信DMAイネーブル
[    5] SCEN  RW スマートカードモードイネーブル
[    4] NACK  RW スマートカードNACKイネーブル
[    3] HDSEL RW 半二重モードイネーブル
[    2] IRLP  RW IrDA低電力モードイネーブル（IREN=1の時のみ有効）
[    1] IREN  RW IrDAモードイネーブル
[    0] EIE   RW エラー割り込みイネーブル
                 受信DMA有効 (DMAR=1) のときに通信エラーが発生すると割り込みが発生

GPR プロテクションタイム/プリスケーラレジスタ 初期値 0x00000000
[31:16] RO Reserved
[15: 8] GT[7:0] RW ガードタイム値（スマートカードモード用）
[ 7: 0] PSC[7:0] RW プリスケーラ値
                    IrDA省電力モードでは、ソースクロックがこの値で除算される。
                    通常のIrDAモードでは、1のみ設定可能。
                    スマートカードモードでは、ソースクロックがこの値の 2倍で除算されて
                    スマートカードのクロックとして使われる。（下位 5bitが有効）
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 [kyoutan.jpn.org](https://kyoutan.jpn.org)

[目次に戻る](index.md)
