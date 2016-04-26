---
layout: post
title: "bootstrap_form vs simple_form"
date: "2016-04-24 10:03:27 +0900"
---
Formをシンプルに書きたいので、両者を比較するメモ

## 結論
- `bootstrap`を使っている場合は、`bootstrap_form`のほうが使いやすい
- そうではない場合は、`simple_form`を使うとシンプルにかけるかも知れません、それほど役に立てないかも知れません

## 前提
- rails 4.2.6
- bootstrap 3 を使う
- `rails g model post title body:text draft:boolean published_at:datetime`

## 比較ポイント
- 縦レウアウトのフォーム
  - 日付フィールドのほうは`bootstrap_form`が楽勝
  - simple_form

    ```
      = simple_form_for @post, url: simple_forms_path do |f|
        = f.input :title
        = f.input :body
        = f.input :draft
        = f.input :published_at
        = f.button :submit
    ```

  - bootstrap_form

    ```
      = bootstrap_form_for @post, url: bootstrap_forms_path do |f|
        = f.text_field :title
        = f.text_area :body
        = f.check_box :draft
        = f.datetime_select :published_at
        = f.submit
    ```

- 横レイアウトのフォーム
  - simple_form

    ```
      = simple_form_for @post, url: simple_forms_path, wrapper: :horizontal_form, html: { class: 'form-horizontal' } do |f|
        = f.input :title
        = f.input :body
        .form-group
          .col-md-9.col-md-offset-3
            = f.input :draft
        = f.input :published_at
        .form-group
          .col-md-9.col-md-offset-3
            = f.button :submit
    ```
  - bootstrap_form

    ```
      = bootstrap_form_for @post, layout: :horizontal, label_col: 'col-md-2', control_col: 'col-md-10', url: bootstrap_forms_path do |f|
        = f.text_field :title
        = f.text_area :body
        = f.form_group do
          = f.check_box :draft
        = f.datetime_select :published_at
        = f.form_group do
          = f.submit
    ```

- `require`、`maxlength`などの自動設定
  - `require`両方とも`validates :title, presence: true`によって自動的に`required`のCSSクラスが付与される
    - simple_form
      - CSSの表現もデフォルトで付いている
    - bootstrap_form
      - デフォルトはCSSのクラス付与だけ、どう表現するかは自分でCSSを書く必要がある

        ```
        label.required:before
          content: " *"
        ```

  - `maxlength`, `size`は両方とも生成されない

- エラー情報の表示
  - 両方ともフィールドの下に表示してくれる
- I18n
  - 両方ともデフォルトでモデルのI18n設定を読み取ってくれる
- 入力フィールドの種類
  - [simple_form Available input types and defaults for each column type](https://github.com/plataformatec/simple_form#available-input-types-and-defaults-for-each-column-type)
  - [bootstrap_form form-helpers](https://github.com/bootstrap-ruby/rails-bootstrap-forms#form-helpers)
