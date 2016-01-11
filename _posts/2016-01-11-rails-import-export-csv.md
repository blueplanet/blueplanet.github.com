---
layout: post
title: 'RailsでCSVのインポート・エクスポートメモ'
date: "2016-01-11"
comments: true
tags: Rails
---

## ゴール
- MacOSとWindows両方で文字化けしないCSVファイルをエクスポート・インポートする
- CSVファイルはカンマ区切りで、ヘッダ行は日本語表記

## 制限
- MacOSとWindowsでCSVを編集するのはExcelアプリケーションで行うこと
- 他のエディターで編集するのはできないわけではないですが、日本語のヘッダ部は文字化けで読めないことがある
- 環境
  - Rails 4.2.5
  - MacOS 10.11.2 + Excel 2016日本語版
  - Window 7（英語版＋日本語言語Pack） + Excel 2016日本語版

## 実装ポイント
- 実装ポイントと言っても、当たり前で`CSV.generate`と`CSV.parse`のパラメータに`Encoding`を設定すること
- エクスポート
  - `{ headers: ['氏名', '年齢'], write_headers: true, encoding: Encoding::SHIFT_JIS }`
- インポート
  - `{ headers: :first_row, encoding: Encoding::SHIFT_JIS }`

## ハマったこと
- MacOSの言語設定を英語にすると、Excel自体が日本語であっても、日本語ヘッダが文字化けになる
  - 対策：MacOSの言語を英語に設定する
- Modern.IEの仮想マシンを使ってWindows7+Excel2016で確認したんですが、Windows自体を日本語化できても日本語ヘッダが文字化け
  - 対策：Excel 2016のオプション設定で「言語」カテゴリで日本語を既定に設定する（日本語を一番上に移動すればOK）

## 参考記事
- [Rails4で実装するCSVのインポート / エクスポート](https://www.imd-net.com/column/3534/)
- [Ruby(Rails)からExcelで文字化けしないCSVファイル作成](http://tech-mr-myself.hatenablog.com/entry/2013/11/14/195346)
