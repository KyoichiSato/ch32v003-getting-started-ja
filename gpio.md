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
```
AFIO->PCFR1
AFIO->EXTICR
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
