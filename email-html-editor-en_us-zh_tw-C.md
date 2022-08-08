---
標題：“電子郵件 HTML 編輯器”
slug: "email-html-editor"
隱藏：假
createdAt:"2021-09-28T09:27:00.287Z"
updatedAt:"2022-02-08T07:18:00.167Z"
---
AIQUA 的電子郵件 HTML 編輯器允許您創建和預覽使用自定義 HTML 製作的電子郵件模板。您可以選擇將自定義 HTML 保存為 [模板](doc:email-templates) ，以便在未來的廣告系列中重複使用。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          "屏幕截圖 2022-02-08 下午 3.17.45.png",
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用說明
為避免編輯器內預覽、AIQUA 儀表板中的縮略圖預覽和您的最終廣告系列電子郵件之間出現意外差異，請確保：
* [聲明一個字符編碼方案](#declare-a-character-encoding-scheme)。
* [檢查您的目標電子郵件客戶端是否支持模板中使用的 HTML](#check-for-supported-html-tags-and-css-properties)。

## 聲明一個字符編碼方案
如果您的 HTML 模板不包含字符編碼聲明，非英文文本可能無法在您的營銷活動電子郵件中正確顯示。為防止出現此問題，請在 HTML 的 `<head>` 標記中包含一個編碼聲明，以指定編碼方案。

例如，指定 `utf-8` 方案的編碼聲明如下所示：
```html
<頭部>
    <meta charset="utf-8">
</head>
```

使用 `utf-8` 編碼的完整 HTML 文件如下所示：
\[block:code]
{
  "codes": \[
    {
      "code": "<html>\\n <head>\\n <meta charset="utf-8"> <!-- 編碼聲明 -->\\n <title>AIQUA Email HTML Editor</title>\\n < /head>\\n <body>\\n <p>此頁面上的文本以 UTF-8 編碼。</p>\\n </body>\\n</html>",
      "language": "html"
    }
  ]
}
\[/block]
## 檢查支持的 HTML 標籤和 CSS 屬性
並非所有電子郵件客戶端都支持所有可用的 HTML 標記和 CSS 屬性，因此檢查您的自定義 HTML 是否包含特定電子郵件客戶端不支持的標記或屬性非常重要。

像這 樣的工具www.caniemail.com允許您搜索特定的 HTML 標記、CSS 屬性或電子郵件客戶端，以確定是否完全支持您的自定義 HTML，如下所示。

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
"caption":“所有 Apple Mail、Gmail 和 Outlook 電子郵件客戶端都支持 `background-color` 屬性。”
}
]
}
\[/block]
按標籤或屬性名稱搜索會返回電子郵件提供商列（例如 **Gmail** 和 **Outlook**）以及來自該提供商的電子郵件客戶端的子列（例如 **iOS** 和 **Android**）。支持您指定的標籤或屬性的電子郵件客戶端將包含一個帶有復選標記的綠色框。