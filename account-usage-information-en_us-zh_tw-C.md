---
title: "使用資訊"
slug: "account-usage-information"
hidden: false
createdAt: "2022-03-10T01:56:10.508Z"
updatedAt: "2022-04-23T02:04:02.759Z"
---
\[block:callout]
{
  "type": "info",
  "title":“注意”，
  "body":“此功能僅在您訂閱AIQUA的電子郵件或短信服務時才可用。請聯繫您的客戶成功經理以獲取更多資訊。
}
\[/block]
## 概述
您帳戶的使用方式資訊頁面詳細介紹了您的廣告系列發送的消息總量，可用於估算潛在的投放費用。有關哪些使用方式指標產生成本的詳細資訊，請參閱 [使用方式定義](#usage-definitions)。對於每個受支援的頻道，指定日期範圍內的總使用量和每日使用量都可見。

目前，以下通道支援使用資訊：
\* 電子郵件
\* SMS

要查看您帳戶的使用資訊，請按兩下 AIQUA 儀錶板左下角的帳戶名稱，然後按兩下「 **使用資訊**」。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/e57a983-usage-info-navbar.png",
          "usage-info-navbar.png",
        1302,
        689,
        “#eff0f4”
      ]
}
  ]
    }
      \[/block]
    選擇要查看其使用情況數據的頻道和日期範圍（[存在日期範圍限制](#date-range-notes-and-limitations)）。您還可以 [將使用方式報告](#exporting-the-usage-report) 匯出到 CSV 檔。
  \[block:image]
{
"images": \[
{
"image": \[
        "https://files.readme.io/582c0d5-usage-info-channel-selection.png",
        "usage-info-channel-selection.png",
        1083,
        518,
        “#eff0f4”
      ]
}
]
}
\[/block]

---

## 用法定義
### 按渠道劃分的使用定義
請參閱以下用法* 定義 *，瞭解用於計算每個管道成本的指標。有關每個指標的詳細定義，請參閱 [使用方式指標](#usage-metrics)。

* *電子郵件使用方式* 由電子郵件發送**的數量 **定義。
* *簡訊使用量* 由使用的簡訊積分**數量 **定義。

### 使用情況指標
\[block:parameters]
{
  "data": {
    "0-0":「**電子郵件已發送** 」，
    "1-0":“**短信已**發送”，
    "2-0":“**使用的**短信積分”，
    "1-1":“與此帳戶關聯的所有廣告系列發送的短信總數。
    "0-1":“與此帳戶關聯的所有廣告系列發送的電子郵件總數。確定電子郵件管道成本。
    "h-0":“使用指標”，
    "h-1":定義“，
    "2-1":“與此帳戶關聯的所有廣告系列使用的短信積分總數。確定SMS通道成本。\\n\\n請注意， **使用的** SMS信用額度可能大於 **SMS傳遞。**SMS 信用額度的使用基於消息長度、語言和字元編碼方案。
  },
  "cols":2,
  "rows":3
}
\[/block]
## 匯出使用方式報告
匯出包含包含帳戶使用情況數據的報告的 CSV 檔，方法是選擇「 **電子郵件** 」或 **「簡訊** 」選項卡，然後按兩下「 **匯出報告** 」 按鈕。
\[block:image]
{
  "images": \[
    {
      "image": \[
            "https://files.readme.io/d3e51d5-usage-info-export-report.png",
          "usage-info-export-report.png",
        1083,
        520,
        “#eff0f5”
      ]
}
]
}
\[/block]
  在顯示的 **「匯出使用方式報告** 」模式中，完成以下操作：
    1\.指定日期範圍。2022 年 4 月 1 日之前的日期的使用方式數據不可用。
      2\.輸入最多 10 封您希望將報告下載連結發送到的電子郵件。
      3\.按兩下「 **導出**」。報告準備就緒后，將向您指定的電子郵件發送一封包含報告下載鏈接的電子郵件。發送下載連結需要一到兩天的時間。
    \[block:image]
  {
"images": \[
{
"image": \[
        "https://files.readme.io/14f611c-export-usage-report.png",
        "export-usage-report.png",
          630,
            511,
              "#f1f2f4"
            ],
    "sizing":"80"
  }
]
}
\[/block]
包含使用方式報告下載鏈接的電子郵件還包括您的商家名稱、管道類型和數據的日期範圍。
\[block:image]
{
"images": \[
{
"image": \[
        "https://files.readme.io/11b0dd7-usage-info-email-content.png",
        "usage-info-email-content.png",
        702,
        541,
        "#f9fafa"
      ],
"sizing":"80"
}
]
}
\[/block]

---

## 日期範圍說明和限制
* **使用數據僅從 2022 年 4 月 1 日**開始提供。
* 只能在 AIQUA 儀錶板上查看過去 12 個月的數據。這意味著開始日期不能比當前日期早 12 個月以上。此限制不適用於使用方式報告匯出。
* 每個日期的使用方式數據基於在帳戶設置中配置的時區。