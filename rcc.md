---
title: クロック制御 (RCC) - CH32V003
date: 2026-02-17
updated: 2026-02-18
---

[目次に戻る](index.md)

## クロック制御 (RCC)
### システムクロック構造
![クロックツリー](rcc_1.png)
図 クロックツリー CH32V003リファレンスマニュアル3章より

#### システムクロック (SYSCLK)
- HSIで動作する場合に選択できる SYSCLKは 24MHzと 48MHzのみ
- SYSCLKのソースは HSI、HSE、PLLのみ。リセット後は HSIが選択される
- CPUのクロックは SYSCLKでは無くHCLK

#### HBパス・ペリフェラルクロック (HCLK)
- CPUコアはこのクロックで動作する
- レジスタ操作で周辺モジュールを初期状態にリセット可能
- 周辺モジュールへのクロック供給を個別にオン／オフすることが可能。モジュールを使用するためには、まずクロックを供給してレジスタアクセスを可能にする必要がある

#### マイクロコントローラ・クロック出力 (MCO)
- SYSCLK、HSI、HSE、PLLCLKのいずれかを外部に出力することができる

#### クロックセキュリティシステム
- HSEクロック障害時に HSIクロックへ自動切り替えを行い、割り込みを生成してアプリケーションソフトウェアによる救済処理を可能にする

## レジスタ
`CH32V00x.h`では、RCCレジスタ群は構造体として定義されていて、各レジスタには以下のような C言語の識別子が割り当てられている。

```c
RCC->CTLR
RCC->CFGR0
RCC->INTR
RCC->APB2PRSTR
RCC->APB1PRSTR
RCC->AHBPCENR
RCC->APB2PCENR
RCC->APB1PCENR
RCC->RSTSCKR
```
これらの識別子は、RCC ペリフェラルの各ハードウェアレジスタにメモリマップド I/O として対応付けられている。

```c
    RCC->CFGR0 &= (uint32_t)0xFFFEFFFF;
    RCC->INTR = 0x009F0000;
    // C言語からはこのようにレジスタを扱うことができる
```

### レジスタ一覧
```
CTLR クロックコントロールレジスタ 初期値 0x0000xx83
[31:26]         RO Reserved
[   25] PLLRDY  RO PLL安定フラグ 1:安定した 0:していない
[   24] PLLON   RW PLL制御ビット 1:PLL有効 0:PLL無効
[23:20]         RO Reserved
[   19] CSSON   RW クロックセキュリティシステム有効化ビット 1:有効 0:無効
[   18] HSEBYP  RW HSEバイパス制御ビット 1:バイパスする（クロックを入力） 0:しない
[   17] HSERDY  RO HSE安定フラグ
[   16] HSEON   RW HSE制御ビット
[15: 8] HSICAL  RO HSIキャリブレーション値
[ 7: 3] HSITRIM RW HSI微調整
[    2]         RO Reserved
[    1] HSIRDY  RO HSI安定フラグ
[    0] HSION   RW HSI制御ビット

CFGR0 クロック構成レジスタ0 初期値 0x00000020
[31:27]        RO Reserved
[26:24] MCO    RW MCOピンスロック出力制御
                  0xx:出力なし
                  100:SYSCLK 101:HSI 110:HSE 111:PLL
[23:17]        RO Reserved
[   16] PLLSRC RW PLLソース 0:HSI 1:HSE
[15:11] ADCPRE RW ADCクロックソース
                  000xx:HBCLK/2   100xx:HBCLK/6
                  00100:HBCLK/4   10100:HBCLK/12
                  00101:HBCLK/8   10101:HBCLK/24
                  00110:HBCLK/16  10110:HBCLK/48
                  00111:HBCLK/32  10111:HBCLK/96
                  010xx:HBCLK/4   110xx:HBCLK/8
                  01100:HBCLK/8   11100:HBCLK/16
                  01101:HBCLK/16  11101:HBCLK/32
                  01110:HBCLK/32  11110:HBCLK/64
                  01111:HBCLK/64  11111:HBCLK/128
[10: 8]        RO Reserved
[ 7: 4] HPER   RW HBCLKプリスケーラ
                  0000:SYSCLK    1000:SYSCLK/2
                  0001:SYSCLK/2  1001:SYSCLK/4
                  0010:SYSCLK/3  1010:SYSCLK/8
                  0011:SYSCLK/4  1011:SYSCLK/16
                  0100:SYSCLK/5  1100:SYSCLK/32
                  0101:SYSCLK/6  1101:SYSCLK/64
                  0110:SYSCLK/7  1110:SYSCLK/128
                  0111:SYSCLK/8  1111:SYSCLK/256
[ 3: 2] SWS    RO SYSCLKステータス
                  00:HSI 01:HSE 10:PLL 11:Not available
[ 1: 0] SW     RW システムクロックソース
                  00:HSI 01:HSE 10:PLL 11:無効

INTR クロックインタラプトレジスタ 初期値 0x00000000
[31:24]          RO Reserved
[   23] CSSC     WO クロックセキュリティシステム割り込みフラグ (CSSF) クリア制御 1:クリアする
[22:21]          RO Reserved
[   20] PLLRDYC  WO PLL-Ready割り込みフラグ (PLLRDYF) クリア制御 1:クリアする
[   19] HSERDYC  WO HSE-Ready割り込みフラグ (HSERDYF) クリア制御
[   18] HSIRDYC  WO HSI-Ready割り込みフラグ (HSIRDYF) クリア制御
[   17]          RO Reserved
[   16] LSIRDYC  WO LSI-Ready割り込みフラグ (LSIRDYF) クリア制御
[15:13]          RO Reserved
[   12] PLLRDYIE RW PLL-Ready割り込み有効ビット 1:割り込み有効
[   11] HSERDYIE RW HSE-Ready割り込み有効ビット
[   10] HSIRDYIE RW HSI-Ready割り込み有効ビット
[    9]          RO Reserved
[    8] LSIRDYIE RW LSI-Ready割り込み有効ビット
[    7] CSSF     RO クロックセキュリティシステム割り込みフラグ
[ 6: 5]          RO Reserved
[    4] PLLRDYF  RO PLL-Ready割り込みフラグ (PLLRDYCでクリア)
[    3] HSERDYF  RO HSE-Ready割り込みフラグ
[    2] HSIRDYF  RO HSI-Ready割り込みフラグ
[    1]          RO Reserved
[    0] LSIRDYF  RO LSI-Ready割り込みフラグ

APB2PRSTR ペリフェラルリセットレジスタ 初期値 0x00000000
[31:15]           RO Reserved
[   14] USART1RST RW USART1リセットコントロール 1:リセット
[   13]           RO Reserved
[   12] SPI1RST   RW SPI1リセットコントロール
[   11] TIM1RST   RW TIM1リセットコントロール
[   10]           RO Reserved
[    9] ADC1RST   RW ADC1リセットコントロール
[ 8: 6]           RO Reserved
[    5] IOPDRST   RW GPIO PDリセットコントロール
[    4] IOPCRST   RW GPIO PCリセットコントロール
[    3]           RO Reserved
[    2] IOPARST   RW GPIO PAリセットコントロール
[    1]           RO Reserved
[    0] AFIORST   RW AFIO (I/O auxiliary function) リセットコントロール

APB1PRSTR ペリフェラルリセットレジスタ 初期値 0x00000000
[31:29]           RO Reserved
[   28] PWRRST    RW パワーインターフェースモジュールリセットコントロール
[27:22]           RO Reserved
[   21] I2C1RST   RW I2C1リセットコントロール
[20:12]           RO Reserved
[   11] WWDGRST   RW WWDG (Window watchdog) リセットコントロール
[10: 1]           RO Reserved
[    0] TIM2RST   RW TIM2リセットコントロール

AHBPCENR ペリフェラルクロック Enable レジスタ 初期値 0x00000004
[31: 3]           RO Reserved
[    2] SRAMEN    RW SRAM interface module clock enable bit.
[    1]           RO Reserved
[    0] DMA1EN    RW DMA1 module clock enable bit.

APB2PCENR ペリフェラルクロック Enable レジスタ 初期値 0x00000000
[31:15]           RO Reserved
[   14] USART1EN  RW USART1 clock enable bit.
[   13]           RO Reserved
[   12] SPI1EN    RW SPI1 clock enable bit.
[   11] TIM1EN    RW TIM1 clock enable bit.
[   10]           RO Reserved
[    9] ADC1EN    RW ADC1 clock enable bit.
[ 8: 6]           RO Reserved
[    5] IOPDEN    RW PD port clock enable bit.
[    4] IOPCEN    RW PC port clock enable bit.
[    3]           RO Reserved
[    2] IOPAEN    RW PA port clock enable bit.
[    1]           RO Reserved
[    0] AFIOEN    RW AFIO (I/O auxiliary function) clock enable bit.

APB1PCENR ペリフェラルクロック Enable レジスタ 初期値 0x00000000
[31:29]           RO Reserved
[   28] PWREN     RW Power interface module clock enable bit.
[27:22]           RO Reserved
[   21] I2C1EN    RW I2C1 clock enable bit.
[20:12]           RO Reserved
[   11] WWDGEN    RW WWDG (Window watchdog) clock enable bit.
[10: 1]           RO Reserved
[    0] TIM2EN    RW TIM2 clock enable bit.

RSTSCKR Control/Status レジスタ 初期値 0x0c000000
[   31] LPWRRSTF  RO Low-power リセットフラグ
[   30] WWDGRSTF  RO WWDGリセットフラグ
[   29] LWDGRSTF  RO IWDGリセットフラグ
[   28] SFTRSTF   RO ソフトウェアリセットフラグ
[   27] PORRSTF   RO Power-up/power-down リセットフラグ
[   26] PINRSTF   RO NRSTピンリセットフラグ
[   25]           RO Reserved
[   24] RMVF      RW リセットフラグコントロール 1:すべてのリセットフラグをクリアする
[23: 2]           RO Reserved
[    1] LSIRDY    RO Low Speed Clock (LSI) 安定フラグ
[    0] LSION     RW LSI有効化ビット
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
