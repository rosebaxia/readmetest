---
のタイトルが表示されます。"メールHTMLエディタ"
slug: "email-html-editor"
非表示: false
createdAt:"2021-09-28T09:27:00.287Z"
updatedAt:"2022-02-08T07:18:00.167Z"
---
AIQUAのメールHTMLエディターは、カスタムHTMLで作られたメールテンプレートを作成し、プレビューすることができます。カスタムHTMLを[テンプレートとして](doc:email-templates)保存し、今後のキャンペーンで再利用することも可能です。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          "スクリーンショット 2022-02-08 at 3.17.45 PM.png",
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用上の注意
編集中のプレビューとAIQUA Dashboardのサムネイルプレビュー、そして最終的なキャンペーンメールとの間に予期せぬ差異が発生しないように、以下のことを確認してください。
* [文字符号化方式を宣言](#declare-a-character-encoding-scheme)する。
* [テンプレートで使用されているHTMLが](#check-for-supported-html-tags-and-css-properties)、対象となるメールクライアントで[サポートされて](#check-for-supported-html-tags-and-css-properties)いることを確認してください。

## 文字符号化方式を宣言する
HTMLテンプレートに文字コード宣言が含まれていない場合、英語以外のテキストがキャンペーンメールに正しく表示されないことがあります。この問題を防ぐには、HTMLの`<head>` タグに、エンコード方式を指定する宣言を記述してください。

例えば、`utf-8` スキームを指定するエンコーディング宣言は次のようになります。
```html
<head>
    <meta charset="utf-8">
</head>
```

で、`utf-8` エンコードを使用した完全な HTML ファイルは次のようになります。
\[block:code]
{
  "codes": \[
    {
      "code": "<html></html></html></html> <head></html> <meta charset="utf-8"> <!-- Encoding declaration --> <title>AIQUA Email HTML Editor</title></head></body></html> <p>The text on this page is encoded in UTF-8.</p>n </body></html> "このページは、UTF-8でエンコードされています。
      "language": "html"
    }
  ]
}
\[/block]
## 対応するHTMLタグとCSSプロパティを確認する
すべてのメールクライアントが、利用可能なすべてのHTMLタグとCSSプロパティをサポートしているわけではないので、カスタムHTMLに特定のメールクライアントがサポートしていないタグやプロパティが含まれていないかどうか確認することが重要です。

のようなツールは、特定のHTMLタグやCSSプロパティ、メールクライアントを検索することができます。 <a href="https://www.caniemail.com" target="_blank">www.caniemail.com</a>などのツールを使って、特定のHTMLタグ、CSSプロパティ、メールクライアントを検索し、カスタムHTMLが完全にサポートされるかどうかを判断することができます。

\[block:image]
{
  "images": \[
    {
      "image": \[
              "https://files.readme.io/451cb94-can-i-email.png",
            "can-i-email.png",
          2298,
        1362,
        "#304251"
      ],
"caption":"`background-color` プロパティは、Apple Mail、Gmail、Outlookのすべてのメールクライアントでサポートされています。"
}
]
}
\[/block]
タグやプロパティ名で検索すると、メールプロバイダ（**Gmailや** **Outlookなど**）のカラムと、そのプロバイダのメールクライアント（**iOSや** **Androidなど**）のサブカラムが返されます。指定したタグやプロパティに対応しているメールクライアントは、緑色のボックスにチェックマークが表示されます。