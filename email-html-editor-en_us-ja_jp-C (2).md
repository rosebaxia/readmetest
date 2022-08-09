---
title: "電子メールHTMLエディタ"
slug: "email-html-editor"
hidden: false
createdAt: "2021-09-28T09:27:00.287Z"
updatedAt: "2022-02-08T07:18:00.167Z"
---
AIQUAの電子メールHTMLエディタを使用すると、カスタムHTMLで作成された電子メールテンプレートを作成およびプレビューできます。カスタム HTML をテンプレート](doc:email-templates) として保存し、 [今後のキャンペーンで再利用することもできます。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          "スクリーンショット 2022-02-08 に 3.17.45 PM.png",
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用上の注意
エディター内プレビュー、AIQUA ダッシュボードのサムネイル プレビュー、および最終的なキャンペーン メールの間に予期しない違いが生じないようにするには、次の点を確認してください。
* [文字エンコード方式を宣言します](#declare-a-character-encoding-scheme)。
* [テンプレートで使用されている HTML が、ターゲットとする電子メール クライアントでサポートされている](#check-for-supported-html-tags-and-css-properties) ことを確認してください。

## 文字コード化スキームの宣言
HTML テンプレートに文字エンコーディング宣言が含まれていない場合、英語以外のテキストがキャンペーンメールに正しく表示されないことがあります。この問題を回避するには、HTML のタグに `<head>` エンコード スキームを指定するエンコード宣言を含めます。

たとえば、スキームを指定するエンコード宣言は `utf-8` 次のようになります。
```html
<頭>
    <meta charset="utf-8">
</head>
```

エンコーディングを使用した `utf-8` 完全なHTMLファイルは次のようになります。
\[block:code]
{
  "codes": \[
    {
      "code": "<html>\\n <head>\\n <meta charset="utf-8"> <!-- エンコーディング宣言 -->\\n <title>AIQUA Email HTML Editor</title>\\n </head>\\n <body>\\n <p>このページのテキストは UTF-8.<>/p<\\n >/body\\n</html>" でエンコードされています。
      "language": "html"
    }
  ]
}
\[/block]
## サポートされている HTML タグと CSS プロパティを確認する
すべての電子メールクライアントが使用可能なすべてのHTMLタグとCSSプロパティをサポートしているわけではないため、カスタムHTMLに特定の電子メールクライアントでサポートされていないタグまたはプロパティが含まれているかどうかを確認することが重要です。

次のような <a href="https://www.caniemail.com" target="_blank">www.caniemail.com</a> ツールを使用すると、以下に示すように、特定のHTMLタグ、CSSプロパティ、または電子メールクライアントを検索して、カスタムHTMLが完全にサポートされるかどうかを判断できます。

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
"caption":「このプロパティは `background-color` 、すべての Apple Mail、Gmail、Outlook の電子メール クライアントでサポートされています。
}
]
}
\[/block]
タグ名またはプロパティ名で検索すると、メールプロバイダ(Gmail** やOutlookなど)の列と、そのプロバイダ(iOSやAndroidなど ** **)のメールクライアントのサブ列が返され** ます** **。 ****指定したタグまたはプロパティをサポートする電子メール クライアントには、チェックマークの付いた緑色のボックスが表示されます。