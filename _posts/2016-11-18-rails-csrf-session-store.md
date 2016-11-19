---
layout: post
title: "Rails CSRF仕組みとセションストアの関係"
date: 2016-11-19 23:15

---

## 問題

ELB＋複数EC２インスタンスにすると、CSRFエラーでフォーム送信できない

## 調査

- セッションストアがデフォルトCookieStoreである
- Rails４から、クッキー内容を暗号化している
- 暗号化する際は、SECRET_KEY_BASEの値を使っている

## 対応

- セッションストアをActiveRecordにすることで複数インスタンスの間で共通できるようになった

## 課題

- SECRET_KEY_BASEで暗号化されるようでしたら、複数インスタンで同じSECRET_KEY_BASEを付けば共有できるはずだが、うまくできなかった
