---
title: "電子郵件 HTML 編輯器"
slug: "email-html-editor"
hidden: false
createdAt: "2021-09-28T09:27:00.287Z"
updatedAt: "2022-02-08T07:18:00.167Z"
---
AIQUA的電子郵件HTML編輯器允許您創建和預覽使用自訂HTML製作的電子郵件範本。您可以選擇將自定義 HTML [另存為範本](doc:email-templates) ，以便在將來的廣告系列中重複使用。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          “螢幕截圖 2022-02-08 在 3.17.45 下午.png”，
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用說明
為避免編輯器內預覽、AIQUA 控制面板中的縮圖預覽和最終活動電子郵件之間存在意外差異，請確保：
* [聲明字元編碼方案](#declare-a-character-encoding-scheme)。
* [檢查範本中使用的 HTML 是否受](#check-for-supported-html-tags-and-css-properties) 目標電子郵件客戶端的支援。

## 聲明字元編碼方案
如果您的 HTML 範本不包含字元編碼聲明，則非英語文本可能無法在您的廣告系列電子郵件中正確顯示。若要防止出現此問題，請在 HTML 的 `<head>` 標記中包含指定編碼方案的編碼聲明。

例如，指定方案的 `utf-8` 編碼聲明如下所示：
```html
<頭>
    <meta charset="utf-8">
</head>
```

使用編碼 `utf-8` 的完整HTML檔如下所示：
\[block:code]
{
  "codes": \[
    {
      “code”： “<html>\\n <head>\\n <meta charset=”utf-8“> <!-- Encoding declaration -->\\n <title>AIQUA Email HTML Editor</title>\\n </head>\\n <body>\\n <p>此頁面上的文本以 UTF-8.</p>\\n </body>\\n</html>”，
      "language": "html"
    }
  ]
}
\[/block]
## 檢查支援的 HTML 標記和 CSS 屬性
並非所有電子郵件客戶端都支援所有可用的 HTML 標記和 CSS 屬性，因此檢查自定義 HTML 是否包含特定電子郵件用戶端不支援的標記或屬性非常重要。

諸如此類 <a href="https://www.caniemail.com" target="_blank">www.caniemail.com</a> 的工具允許您搜索特定的HTML標籤，CSS屬性或電子郵件用戶端，以確定您的自定義HTML是否完全受支援，如下所示。

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
"caption":“該 `background-color` 屬性受所有Apple Mail，Gmail和Outlook電子郵件用戶端的支援。
}
]
}
\[/block]
按標記或媒體資源名稱搜索會返回電子郵件供應商（如 **Gmail** 和 **Outlook**）的列以及來自該提供者（如 **iOS** 和 **Android**）的電子郵件用戶端的子列。支援您指定的標記或屬性的電子郵件用戶端將包含一個帶有複選標記的綠色框。