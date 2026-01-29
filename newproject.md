---
title: 新規プロジェクトを作成してGPIOを操作してみる - MRS2 - CH32V003
date: 2026-01-06
updated: 2026-01-07
---

[目次に戻る](index.md)

## 新規プロジェクトを作成してGPIOを操作してみる
### 新規プロジェクトの作成
[File] - [New] - [MounRiver Project] を選択。

![New Project](gpiotest_1.png)

![MCUを選ぶ](gpiotest_2.png)

ダイアログで
- `CH32V003F4P` を選択
- プロジェクト名を入力
- プロジェクトの場所を選択
- [Finish] を押して完了

MCUの型番を選択するとプロジェクト名が型番と同じものに変更されるので、MCUを選択してからプロジェクト名を入力する。

![作成完了](gpiotest_3.png)

これで新しいプロジェクトが作成されました。すでにこの状態でペリフェラルのライブラリもプロジェクトのディレクトリにコピーされて、すぐに使える状態になっています。

### クロックの設定

### この文章のライセンス
[CC0 1.0 Universal](LICENSE)

{{page.date}}作成 {{page.updated}}更新 佐藤恭一 kyoutan.jpn.org

[目次に戻る](index.md)
