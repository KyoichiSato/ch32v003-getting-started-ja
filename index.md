---
title: MounRiver Studioで使う CH32V003 はじめの一歩
date: 2025-11-27
updated: 2026-02-15
---

# MounRiver Studioで使う CH32V003 はじめの一歩
秋月電子で 100円の R8C/M12の販売が終了したので、かわりに秋月電子から 50円で買える32bitマイコンの CH32V003を使うことにしました。メーカーが用意している開発環境の MounRiver Studioで使ってみます。簡単にデバッガを使えるし、サンプルコードも用意されているしで便利に使うことができました。

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
4. [V1772開発基板の回路図](./V1772.md)

### この文章のライセンス
CC0 1.0 Universal

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org