---
title: MounRiver Studioで使う CH32V003 はじめの一歩
date: 2025-11-27
updated: 2026-02-27
---

# MounRiver Studioで使う CH32V003 はじめの一歩
秋月電子で 100円の R8C/M12の販売が終了したので、かわりに秋月電子から 50円で買える32bitマイコンの CH32V003を使うことにしました。メーカーが用意している開発環境の MounRiver Studioで使ってみます。簡単にデバッガを使えるし、サンプルコードも用意されているしで便利に使うことができました。

この記事は他の MCUを C言語で使ったことがある人向けに書きました。全くの初心者の場合 Raspberry Pi Picoや Arduinoがおすすめです。

## 目次
1. [CH32V003の概要](./CH32V003.md)
    * 概要
    * 開発に必要なもの
    * 資料の探し方
2. [MounRiver Studioを使ってみる](./MRS2getstart.md)
    * MounRiver Studioのインストール
    * サンプルのビルド
    * 書き込み
    * 実行
3. [新規プロジェクトを作成してGPIOを操作してみる](newproject.md)
    * 新規プロジェクトの作成
    * クロックの設定
    * GPIOを操作するプログラムを書く
    * PD1 (SWD) をユーザーが使用した場合のプログラム再書き込み方法
    * GPIOの入力も使ってみる
4. [PD1/SWD (SDI)ピンをユーザーが使う方法と注意点](pd1_swd.md)
    * PD1/SWDピンをユーザーが使う方法
    * ユーザーの操作で都度コードフラッシュを消去する方法
    * 書き込み時に自動でコードフラッシュを消去する方法
5. 資料
    * ペリフェラルの概要
      * [RCC クロック制御](rcc.md)
      * [IWDG 独立型ウォッチドッグ](iwdg.md)
      * [WWDG ウィンドウウォッチドッグ](wwdg.md)
      * [PFIC/EXTI 割り込みコントローラ](pfic_exti.md)
      * [SysTick システムタイマ](systick.md)
      * [GPIOとピンリマップ](gpio.md)
      * [DMA](dma.md)
      * [ADC](adc.md)
      * [TIM1 アドバンスドコントロールタイマ](tim1.md)
      * [TIM2 汎用タイマ](tim2.md)
    * [V1772開発基板の回路図](./V1772.md)
    * [https://www.wch-ic.com/products/CH32V003.html](https://www.wch-ic.com/products/CH32V003.html) メーカーの製品ページ
    * [CH32V003 日本語リファレンスマニュアル](https://archive.org/details/ch-32-v-003-v-1.9/CH32V003%20%E3%83%87%E3%83%BC%E3%82%BF%E3%82%B7%E3%83%BC%E3%83%88%20V1.7/) (Internet Archive) 

### この文章のライセンス
[CC0 1.0 Universal](https://github.com/KyoichiSato/ch32v003-getting-started-ja/blob/main/LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 [kyoutan.jpn.org](https://kyoutan.jpn.org)
