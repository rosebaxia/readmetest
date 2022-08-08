---
title: "電子郵件編輯器"
slug: "email-html-editor"
hidden: false
createdAt: "2021-09-28T09:27:00.287Z"
updatedAt: "2022-02-08T07:18:00.167Z"
---
AIQUA 的電子郵件 HTML 編輯器允許您創建和預覽使用自定義 HTML 製作的電子郵件模板。您可以選擇將自訂 HTML 儲存為 [ 範本](doc:email-templates)，以便在未來的廣告活動中重複使用。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
          「螢幕擷取畫面」PM.png
        1213,
        633,
        "#38413f"
      ]
}
]
}
\[/block]
# 使用注意事項
為了避免編輯內預覽、AIQUA 儀表板中的縮圖預覽和最終行銷活動電子郵件之間出現意外差異，請務必：
* [宣告字元編碼方案](#declare-a-character-encoding-scheme)。
* [檢查模板中使用的 HTML 是否受到您](#check-for-supported-html-tags-and-css-properties)定位的電子郵件客戶端的支持。

## 宣告字元編碼配置
如果您的 HTML 範本不包含字元編碼宣告，則非英文文字可能無法在行銷活動電子郵件中正確顯示。若要避免此問題，請在 HTML 的 `<head>` 標籤中加入編碼宣告，指定編碼配置。

例如，指定 `utf-8` 配置的編碼聲明如下所示：
```html
<head>
    <meta charset="utf-8">
</head>
```

並且使用 `utf-8` 編碼的完整 HTML 文件如下所示：
\[block:code]
{
  "codes": \[
    {
      「代碼」：「<html>\\ n<head>\\ n {<meta charset=} <!— 編碼宣告 —>\\ n</head> <title>艾奎電子郵件 HTML 編輯器</title>\\ n<body>\\ n\\ n <p>此頁面上的文字以 UTF-8 編碼。</p>\\n</body>\\ n</html>「,
      "language": "html"
    }
  ]
}
\[/block]
## 檢查是否有支援的 HTML 標籤和 CSS 屬性
並非所有電子郵件用戶端都支援所有可用的 HTML 標籤和 CSS 屬性，因此請務必檢查您的自訂 HTML 包含特定電子郵件用戶端不支援的標籤或屬性。

這類工具可 <a href="https://www.caniemail.com" target="_blank"> www.caniemail.com</a> 讓您搜尋特定的 HTML 標籤、CSS 屬性或電子郵件用戶端，以判斷是否完全支援您的自訂 HTML，如下所示。

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
"caption":「該 `background-color` 屬性是由所有蘋果郵件，Gmail 和 Outlook 電子郵件客戶端的支持。「
}
]
}
\[/block]
依標籤或屬性名稱搜尋會傳回電子郵件供應商的欄位 (例如 ** Gmail** 和 ** Outlook**)，以及來自該提供者的電子郵件用戶端子欄 (例如 ** iOS** 和 ** Android**)。支援您指定之標籤或屬性的電子郵件用戶端會包含一個帶有核取記號的綠色方塊。