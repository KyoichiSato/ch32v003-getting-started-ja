---
title: DMA - CH32V003
date: 2026-02-20
updated: 2026-02-21
---

[目次に戻る](index.md)

## DMA
* ペリフェラル -> メモリ、メモリ -> ペリフェラル、メモリ -> メモリ
* データ幅 8bit/16bit/32bitから選択
* 転送サイズ最大 65535カウント
* チャンネル優先度を設定可能
* ループ対応のバッファ管理
* 転送ハーフフラグ
* レジスタ数が多いけれど 7チャンネル分のレジスタがあるからで、1チャンネル分だけを見るとレジスタは 4個だけ

### DMA要求マッピング
各ペリフェラルのレジスタで DMA制御ビットを設定することで、個別に DMA機能を 有効/無効に設定できる。

#### ペリフェラルマッピング一覧

|CH1     |CH2     |CH3     |CH4      |CH5      |CH6     |CH7     |
|--------|--------|--------|---------|---------|--------|--------|
|ADC1    |        |        |         |         |        |        |
|        |SPI1_RX |SPI1_TX |         |         |        |        |
|        |        |        |USART1_RX|USART1_TX|        |        |
|        |        |        |         |         |I2C_TX  |I2C_RX  |
|        |TIM1_CH1|TIM1_CH2|TIM1_CH4<br>TIM1_TRIG<br>TIM1_COM|TIM1_UP  |TIM1_CH3|        |
|TIM2_CH3|TIM2_UP |        |         |TIM2_CH1 |        |TIM2_CH2<br>TIM2_CH4|

### DMAレジスタ
```
DMA1->INTFR
DMA1->INTFCR

DMA1_Channel1->CFGR
DMA1_Channel1->CNTR
DMA1_Channel1->PADDR
DMA1_Channel1->MADDR
DMA1_Channel2->CFGR
DMA1_Channel2->CNTR
DMA1_Channel2->PADDR
DMA1_Channel2->MADDR
    :
DMA1_Channel7 まである

INTFR DMA割り込みステータスレジスタ 初期値 0x00000000
[31:28]        RO Reserved
[   27] TEIF7  RO CH7 転送エラーフラグ  1:エラー
[   26] HTIF7  RO CH7 転送ハーフフラグ  1:設定した転送サイズの半分を転送し終えた
[   25] TCIF7  RO CH7 転送完了フラグ    1:転送完了
[   24] GIF7   RO CH7 グローバル割り込みフラグ 1:TEIF/HTIF/TCIFいずれかが発生した
[   23] TEIF6  RO CH6 転送エラーフラグ
[   22] HTIF6  RO CH6 転送ハーフフラグ
[   21] TCIF6  RO CH6 転送完了フラグ
[   20] GIF6   RO CH6 グローバル割り込みフラグ
[   19] TEIF5  RO CH5 転送エラーフラグ
[   18] HTIF5  RO CH5 転送ハーフフラグ
[   17] TCIF5  RO CH5 転送完了フラグ
[   16] GIF5   RO CH5 グローバル割り込みフラグ
[   15] TEIF4  RO CH4 転送エラーフラグ
[   14] HTIF4  RO CH4 転送ハーフフラグ
[   13] TCIF4  RO CH4 転送完了フラグ
[   12] GIF4   RO CH4 グローバル割り込みフラグ
[   11] TEIF3  RO CH3 転送エラーフラグ
[   10] HTIF3  RO CH3 転送ハーフフラグ
[    9] TCIF3  RO CH3 転送完了フラグ
[    8] GIF3   RO CH3 グローバル割り込みフラグ
[    7] TEIF2  RO CH2 転送エラーフラグ
[    6] HTIF2  RO CH2 転送ハーフフラグ
[    5] TCIF2  RO CH2 転送完了フラグ
[    4] GIF2   RO CH2 グローバル割り込みフラグ
[    3] TEIF1  RO CH1 転送エラーフラグ
[    2] HTIF1  RO CH1 転送ハーフフラグ
[    1] TCIF1  RO CH1 転送完了フラグ
[    0] GIF1   RO CH1 グローバル割り込みフラグ TEIF/HTIF/TCIFいずれかが発生した

INTFCR DMA割り込みフラグクリアレジスタ 初期値 0x00000000
[31:28] RO Reserved
[   27] CTEIF7 WO CH7 転送エラーフラグクリアビット        1:TEIFクリア
[   26] CHTIF7 WO CH7 転送ハーフフラグクリアビット        1:HTIFクリア
[   25] CTCIF7 WO CH7 転送完了フラグクリア                1:TCIFクリア
[   24] CGIF7  WO CH7 グローバル割り込みフラグクリアビット 1:TEIF/HTIF/TCIF/GIFクリア
[   23] CTEIF6 WO CH6 転送エラーフラグクリアビット
[   22] CHTIF6 WO CH6 転送ハーフフラグクリアビット
[   21] CTCIF6 WO CH6 転送完了フラグクリア
[   20] CGIF6  WO CH6 グローバル割り込みフラグクリアビット
[   19] CTEIF5 WO CH5 転送エラーフラグクリアビット
[   18] CHTIF5 WO CH5 転送ハーフフラグクリアビット
[   17] CTCIF5 WO CH5 転送完了フラグクリア
[   16] CGIF5  WO CH5 グローバル割り込みフラグクリアビット
[   15] CTEIF4 WO CH4 転送エラーフラグクリアビット
[   14] CHTIF4 WO CH4 転送ハーフフラグクリアビット
[   13] CTCIF4 WO CH4 転送完了フラグクリア
[   12] CGIF4  WO CH4 グローバル割り込みフラグクリアビット
[   11] CTEIF3 WO CH3 転送エラーフラグクリアビット
[   10] CHTIF3 WO CH3 転送ハーフフラグクリアビット
[    9] CTCIF3 WO CH3 転送完了フラグクリア
[    8] CGIF3  WO CH3 グローバル割り込みフラグクリアビット
[    7] CTEIF2 WO CH2 転送エラーフラグクリアビット
[    6] CHTIF2 WO CH2 転送ハーフフラグクリアビット
[    5] CTCIF2 WO CH2 転送完了フラグクリア
[    4] CGIF2  WO CH2 グローバル割り込みフラグクリアビット
[    3] CTEIF1 WO CH1 転送エラーフラグクリアビット
[    2] CHTIF1 WO CH1 転送ハーフフラグクリアビット
[    1] CTCIF1 WO CH1 転送完了フラグクリア
[    0] CGIF1  WO CH1 グローバル割り込みフラグクリアビット

CFGR DMAxコンフィギュレーションレジスタ 初期値 0x00000000
[31:15]         RO Reserved
[   14] MEM2MEM RW メモリ -> メモリ モード有効化ビット 1:有効
[13:12] PL      RW チャンネル優先度 0~3 3が優先度最高
[11:10] MSIZE   RW メモリアドレスのデータ幅       00:8bit 01:16bit 10:32bit 11:設定不可
[ 9: 8] PSIZE   RW ペリフェラルアドレスのデータ幅 00:8bit 01:16bit 10:32bit 11:設定不可
[    7] MINC    RW メモリアドレスのインクリメント有効化       1:インクリメントする 0:固定
[    6] PINC    RW ペリフェラルアドレスのインクリメント有効化 1:インクリメントする 0:固定
[    5] CIRC    RW サイクリックモード有効化（リングバッファ） 1:有効
[    4] DIR     RW 転送方向 1:メモリ -> ペリフェラル 0:ペリフェラル -> メモリ
[    3] TEIE    RW 転送エラー割り込み有効 1:有効
[    2] HTIE    RW 転送ハーフ割り込み有効 1:有効
[    1] TCIE    RW 転送完了割り込み有効   1:有効
[    0] EN      RW チャンネル有効化       1:有効

CNTR DMAx転送カウンタ 初期値 0x00000000
[31:16]           RO Reserved
[15: 0] NDT[15:0] RW 転送データ数 (0~65535)
                     読み出すと残りの転送数
                     サイクリックモードの場合、0になると自動的に設定値にリロードされる
CFGRのEN=0のときのみ書き込み可能

PADDR DMAxペリフェラルアドレスレジスタ 初期値 0x00000000
[31: 0] PA[31:0] RW ペリフェラルベースアドレス
                    16bit幅モードでは bit0を無視して 2バイト境界にアラインされる
                    32bit幅モードでは bit0, bit1を無視して 4バイト境界にアラインされる
                    メモリ -> メモリ モードでは転送元アドレス
CFGRのEN=0のときのみ書き込み可能

MADDR DMAxメモリアドレスレジスタ 初期値 0x00000000
[31: 0] MA[31:0] RW メモリベースアドレス
                    16bit幅モードでは bit0を無視して 2バイト境界にアラインされる
                    32bit幅モードでは bit0, bit1を無視して 4バイト境界にアラインされる
                    メモリ -> メモリ モードでは転送先アドレス
CFGRのEN=0のときのみ書き込み可能
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
