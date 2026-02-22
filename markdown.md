---
title: GitHub Docsの Markdown
date: 2026-02-22
updated: 2026-02-22
---
# GitHub Docsの Markdown
* [GitHub 上での執筆とフォーマットについて](https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github)
* [GitHub Docs での Markdown と Liquid の使用](https://docs.github.com/ja/contributing/writing-for-github-docs/using-markdown-and-liquid-in-github-docs)
* [Liquid](https://shopify.github.io/liquid/basics/introduction/)

# 見出し1
## 見出し2

段落
続きの文章 `code` 色のサンプル`#FF8844` **強調** <sub>下付</sub> <sup>上付き</sup> <ins>下線</ins>

次の段落 [link](https://kyoutan.jpn.org/)

![画像](WCH-LinkE_V1772.png)

* list
- list
  - list

1. list
   1. list
   - list

> 引用

```c copy
// コードブロック

    // PC0をフローティング入力に設定
    RCC_APB2PeriphClockCmd (RCC_APB2Periph_GPIOC, ENABLE);  // GPIOCにクロックを供給
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;               // 設定するピンの選択
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;   // 入出力モードの選択
    GPIO_Init (GPIOC, &GPIO_InitStructure);                 // 上で値をセットした構造体をレジスタに書き込んで設定を行う

    // ボタンを押すたびに LEDを点灯させたり消灯させたりする
    while (true) {
        if (1 == GPIO_ReadInputDataBit (GPIOC, GPIO_Pin_0)) {
            // ボタンが押されたとき出力を反転する
            GPIO_WriteBit (GPIOD, GPIO_Pin_1, !GPIO_ReadOutputDataBit (GPIOD, GPIO_Pin_1));

            //  ボタンが離されるまで待つ
            while (1 == GPIO_ReadInputDataBit (GPIOC, GPIO_Pin_0));  // 押されている間ループ
        }
        // チャタリング除去のディレイ（極端に早い連打を無視する）
        Delay_Ms (10);
```

|表  |左寄せ|中寄せ|右寄せ|
|----|:-----|:----:|-----:|
|1234|  5678|  9012|  3456|
| 789|   012|   345|   678|

<!--コメント-->


### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org
