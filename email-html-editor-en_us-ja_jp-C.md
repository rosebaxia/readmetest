---
title: "電子メール HTML エディタ"
slug: "email-html-editor"
hidden: false
createdAt: "2021-09-28T09:27:00.287Z"
updatedAt: "2022-02-08T07:18:00.167Z"
---
AIQUA の Email HTML Editor を使用すると、カスタム HTML で作成された電子メール テンプレートを作成およびプレビューできます。カスタム HTML を [テンプレート](doc:email-templates) として保存して、将来のキャンペーンで再利用することができます。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          "スクリーンショット 2022-02-08 3.17.45 PM.png",
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用上の注意
エディター内のプレビュー、AIQUA ダッシュボードのサムネイル プレビュー、および最終的なキャンペーン メールの予期しない違いを回避するには、次のことを確認してください。
* [文字コード体系を宣言します](#declare-a-character-encoding-scheme)。
* [テンプレートで使用されている HTML が、対象の電子メール クライアントでサポートされていることを確認してください](#check-for-supported-html-tags-and-css-properties) 。

## 文字エンコーディング スキームを宣言する
HTML テンプレートに文字エンコード宣言が含まれていない場合、英語以外のテキストがキャンペーン メールに正しく表示されない場合があります。この問題を回避するには、HTML の `<head>` タグにエンコーディング スキーマを指定するエンコーディング宣言を含めます。

たとえば、`utf-8` スキームを指定するエンコーディング宣言は次のようになります。
```html
<頭>
    <meta charset="utf-8">
</head>
```

`utf-8` エンコーディングを使用した完全な HTML ファイルは次のようになります。
\[block:code]
{
  "codes": \[
    {
      "code": "<html>\\n <head>\\n <meta charset="utf-8"> <!-- エンコーディング宣言 -->\\n <title>AIQUA Email HTML Editor</title>\\n < /head>\\n <body>\\n <p>このページのテキストは UTF-8 でエンコードされています.</p>\\n </body>\\n</html>",
      "language": "html"
    }
  ]
}
\[/block]
## サポートされている HTML タグと CSS プロパティを確認する
すべてのメール クライアントが利用可能な HTML タグと CSS プロパティをすべてサポートしているわけではないため、カスタム HTML に、特定のメール クライアントでサポートされていないタグやプロパティが含まれていないかどうかを確認することが重要です。

のよ うなツールwww.caniemail.com以下に示すように、特定の HTML タグ、CSS プロパティ、または電子メール クライアントを検索して、カスタム HTML が完全にサポートされるかどうかを判断できます。

\[block:image]
{
  "images": \[
    {
      "image": \[
              "https://files.readme.io/451cb94-can-i-email.png",
            "can-i-email.png",
          2298、
        1362,
        "#304251"
      ],
"caption":「`background-color` プロパティは、すべての Apple Mail、Gmail、および Outlook メール クライアントでサポートされています。」
}
]
}
\[/block]
タグまたはプロパティ名で検索すると、メール プロバイダー (**Gmail** や **Outlook**など) の列と、そのプロバイダーのメール クライアント (**iOS** や **Android**など) のサブ列が返されます。指定したタグまたはプロパティをサポートする電子メール クライアントには、チェックマークの付いた緑色のボックスが含まれます。