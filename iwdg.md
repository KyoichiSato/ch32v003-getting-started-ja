---
title: 独立型ウォッチドッグ (IWDG) - CH32V003
date: 2026-02-19
updated: 2026-02-19
---

[目次に戻る](index.md)

## 独立型ウォッチドッグ (IWDG)
* 12bit減算カウンタ
* クロックソースは LSI
* カウンタが 0になるとリセット
  
### レジスタ
`CH32V00x.h`では、IWDGレジスタ群は構造体として定義されていて、各レジスタには以下のような C言語の識別子が割り当てられている。
```c
IWDG->CTLR
IWDG->PSCR
IWDG->RLDR
IWDG->STATR
```

```
CTLR コントロールレジスタ 初期値 0x0000
[15: 0] KEY  WO 0xaaaa:ウォッチドッグリフレッシュ（リロードレジスタの値をカウンタへロード）
                0x5555:PSCRおよび RLDRレジスタの変更を許可する
                0xcccc:ウォッチドッグを起動する
ユーザーオプションバイトでリセット時の自動起動が可能

PSCR プリスケーラレジスタ 初期値 0x0000
[15: 3]      RO Reserved
[ 2: 0] PR   RW IWDGクロック分周比
                000:LSI/4   100:LSI/64
                001:LSI/8   101:LSI/128
                010:LSI/16  110:LSI/256
                011:LSI/32  111:LSI/256
変更前にCTLRに 0x5555を書き込む必要あり
STATRの PVUビットが 1の場合の読み出し値は無効

RLDR リロードレジスタ 初期値 0x0fff
[15:12]      RO Reserved
[11: 0] RL   RW リロード値
変更前にCTLRに 0x5555を書き込む必要あり
STATRの RVUビットが 1の場合の読み書きは無効

STATR ステータスレジスタ 初期値 0x0000
[15: 2]      RO Reserved
[    1] RVU  RO リロード値更新フラグ 0:更新完了
[    0] PVU  RO クロック分周比更新フラグ 0:更新完了
```

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 [kyoutan.jpn.org](https://kyoutan.jpn.org)


[目次に戻る](index.md)
