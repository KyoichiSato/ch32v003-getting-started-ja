---
title: GPIOとオルタネート機能 (GPIO/AFIO) - CH32V003
date: 2026-02-19
updated: 2026-02-20
---

[目次に戻る](index.md)

## GPIOとオルタネート機能 (GPIO / AFIO)
* 癖のない GPIOである
* ペリフェラルピンのリマップが可能で、 3種類くらいから選べる
* リマップは AFIOで行う

### GPIOレジスタ
```
GPIOx->CFGLR
GPIOx->INDR
GPIOx->OUTDR
GPIOx->BSHR
GPIOx->BCR
GPIOx->LCKR

xには A/C/Dのポート名が入る GPIOA->CFGLR とか GPIOC->CFGLR とか GPIOD->CFGLR。
レジスタ数が多めだけど、1つのレジスタに複数の機能が割当らてていなくて、
機能ごとにレジスタが別れていてわかりやすい。

CFGLR PORT構成レジスタ 初期値 0x44444444
[31:30] CNF7  RW PORT7のピン動作モード
                 MODEが入力                     MODEが出力
                 00:アナログ入力                 00:汎用プッシュプル出力
                 01:フローティング入力            01:汎用オープンドレイン出力
                 10:プルアップ/プルダウン付き入力  10:ペリフェラル機能プッシュプル出力
                                                 11:ペリフェラル機能オープンドレイン出力
[29:28] MODE7 RW PORT7のモード選択
                 00:入力 01:出力10MHz 10:出力20MHz 11:出力30MHz
[27:26] CNF6  RW PORT6のピン動作モード
[25:24] MODE6 RW PORT6のモード選択
[23:22] CNF5  RW PORT5のピン動作モード
[21:20] MODE5 RW PORT5のモード選択
[19:18] CNF4  RW PORT4のピン動作モード
[17:16] MODE4 RW PORT4のモード選択
[15:14] CNF3  RW PORT3のピン動作モード
[13:12] MODE3 RW PORT3のモード選択
[11:10] CNF2  RW PORT2のピン動作モード
[ 9: 8] MODE2 RW PORT2のモード選択
[ 7: 6] CNF1  RW PORT1のピン動作モード
[ 5: 4] MODE1 RW PORT1のモード選択
[ 3: 2] CNF0  RW PORT0のピン動作モード
[ 1: 0] MODE0 RW PORT0のモード選択
リセット後初期値はフローティング入力
出力モードの周波数は、ピンの駆動能力の選択。駆動能力が高いと高い周波数で出力できるという意味。

INDR 入力データレジスタ 初期値 0x000000xx
[31: 8]          RO Reserved
[ 7: 0] IDR[7:0] RO ポートの入力データ

OUTDR 出力データレジスタ 初期値 0x00000000
[31: 8]          RO Reserved
[ 7: 0] ODR[7:0] RW ポートの出力データ
読み出すと書いた値を読み出すことができる

BSHR PORTセット／リセットレジスタ 初期値 0x00000000
[31:24]          RO Reserved
[23:16] BR[7:0]  WO リセットビット 1を書くとOUTDRの対応するビットが0になる
[15: 8]          RO Reserved
[ 7: 0] BS[7:0]  WO セットビット   1を書くとOUTDRの対応するビットが1になる
BRと BS両方に 1を書いは場合は BSが優先される

BCR PORTリセットレジスタ 初期値 0x00000000
[31: 8]          RO Reserved
[ 7: 0] BR[7:0]  RW リセットビット 1を書くとOUTDRの対応するビットが0になる

LCKR PORT構成ロックレジスタ 初期値 0x00000000
[31:17]          RO Reserved
[   16] LCKK  RW lock key 次の手順で操作するとポートの構成をロックできる。
                 write 1 - write 0 - write 1 - read 0 - read 1
                 ※最後の read 1 は省略可能だが、ロックが有効になったことを確認するために使用できる。
                 読み出しはいつでも可能で、0なら未ロック、1ならロック済。
[15: 8]          RO Reserved
[ 7: 0] LCK[7:0] RW 1を書くと対応するポートの CFGLR構成をロックする。
                    LCKKでロックする前のみ変更可能。
LOCKシーケンスが実行されると、次回リセットまでポートの構成を変更できない。
```
CFGLRで入出力の設定、INDRが入力レジスタ、OUTDRが出力レジスタ、BSHRがビットセット／リセットレジスタ。これだけで使えるので簡単。

### AFIOレジスタ

* GPIOとはクロックが独立しているので、レジスタアクセス前にAFIOにクロックを供給すること

```
AFIO->PCFR1
AFIO->EXTICR

PCFR1 リマップレジスタ 初期値 0x00000000
[31:27]                RO Reserved
[26:24] SWCFG[2:0]     RW SWD (SDI) 無効化 0xx:SWD有効 100:SWD無効（GPIOとして使用）
[   23] TIM1_IREMAP    RW TIM1選択 1:LSIクロック 0:外部ピン
[   22] I2C1REMAP1     RW I2Cリマップ上位ビット（下位ビットは このレジスタの bit1）
                          00:SCL/PC2, SDA/PC1 デフォルト
                          01:SCL/PD1, SDA/PD0
                          1X:SCL/PC5, SDA/PC6
[   21] USART1_RM1     RW USART1リマップ上位ビット（下位ビットはこのレジスタの bit2）
                          00:CK/PD4, TX/PD5, RX/PD6, CTS/PD3, RTS/PC2
                          01:CK/PD7, TX/PD0, RX/PD1, CTS/PC3, RTS/PC2, SW_RX/PD0
                          10:CK/PD7, TX/PD6, RX/PD5. CTS/PC6, RTS/PC7, SW_RX/PD6
                          11:CK/PC5, TX/PC0, RX/PC1, CTS/PC6, RTS/PC7、SW_RX/PC0
[20:19]                RO Reserved
[   18] ADC_ETRGREG_RM RW ADC外部トリガピン選択 1:PC2 0:PD3
[   17] ADC_ETRGINJ_RM RW ADC外部インジェクションピン選択 1:PC2 0:PD3
[   16]                RO Reserved
[   15] PA1PA2_RM      RW PA1, PA2機能選択 1:無効（水晶を接続） 0:GPIO
[14:10]                RO Reserved
[ 9: 8] TIM2_RM[1:0]   RW TIM2リマップ
                          00:CH1/ETR/PD4, CH2/PD3, CH3/PC0, CH4/PD7 デフォルト
                          01:CH1/ETR/PC5, CH2/PC2, CH3/PD2, CH4/PC1
                          10:CH1/ETR/PC1, CH2/PD3, CH3/PC0, CH4/PD7
                          11:CH1/ETR/PC1, CH2/PC7, CH3/PD6, CH4/PD5
[ 7: 6] TIM1_RM[1:0]   RW TIM1リマップ
00:ETR/PC5, CH1/PD2, CH2/PA1, CH3/PC3, CH4/PC4, BKIN/PC2, CH1N/PD0, CH2N/PA2, CH3N/PD1 デフォルト
01:ETR/PC5, CH1/PC6, CH2/PC7, CH3/PC0, CH4/PD3, BKIN/PC1, CH1N/PC3, CH2N/PC4, CH3N/PD1
10:ETR/PD4, CH1/PD2, CH2/PA1, CH3/PC3, CH4/PC4, BKIN/PC2, CH1N/PD0, CH2N/PA2, CH3N/PD1
11:ETR/PC2, CH1/PC4, CH2/PC7, CH3/PC5, CH4/PD4, BKIN/PC1, CH1N/PC3, CH2N/PD2, CH3N/PC6
[ 5: 3]                RO Reserved
[    2] USART1_RM      RW USART1リマップ下位ビット
[    1] I2C1_RM        RW I2Cリマップ下位ビット
[    0] SPI1_RM        RW SPI1リマップ
                          0:NSS/PC1, CK/PC5, MISO/PC7, MOSI/PC6 デフォルト
                          1:NSS/PC0, CK/PC5, MISO/PC7, MOSI/PC6

EXTICR 外部割り込み構成レジスタ 初期値 0x00000000
[31:16] RO Reserved
[15:14] EXTI7[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[13:12] EXTI6[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[11:10] EXTI5[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[ 9: 8] EXTI4[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[ 7: 6] EXTI3[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[ 5: 4] EXTI2[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[ 3: 2] EXTI1[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
[ 1: 0] EXTI0[1:0] RW 外部割り込み入力ピン構成 00:PA 10:PC 11:PD
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
