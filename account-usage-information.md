---
title: "Usage Information"
slug: "account-usage-information"
hidden: false
createdAt: "2022-03-10T01:56:10.508Z"
updatedAt: "2022-04-23T02:04:02.759Z"
---
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "This feature is only available if you're subscribed to AIQUA's email or SMS services. Contact your customer success manager for more information."
}
[/block]
## Overview
Your account's usage information page details the total volume of messages your campaigns deliver, which can be used to estimate the potential delivery expenses incurred. For details on which usage metrics incur costs, see [Usage Definitions](#usage-definitions). Total and daily usage within a specified date range are visible for each supported channel.

Currently, usage information is supported for the following channels:
* Email
* SMS

To view your account's usage information, click on your account name in the lower left corner of the AIQUA Dashboard, then click **Usage Information**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e57a983-usage-info-navbar.png",
        "usage-info-navbar.png",
        1302,
        689,
        "#eff0f4"
      ]
    }
  ]
}
[/block]
Select the channel and date range you want to view usage data for ([date range limitations apply](#date-range-notes-and-limitations)). You can also [export the usage report](#exporting-the-usage-report) to a CSV file.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/582c0d5-usage-info-channel-selection.png",
        "usage-info-channel-selection.png",
        1083,
        518,
        "#eff0f4"
      ]
    }
  ]
}
[/block]

---

## Usage Definitions
### Usage Definition by Channel
Refer to the following definitions of *usage* to understand which metric is used to calculate costs for each channel. For detailed definitions of each metric, see [Usage Metrics](#usage-metrics).

* *Email usage* is defined by the number of **Email Sent**.
* *SMS usage* is defined by the number of **SMS Credits Used**.

### Usage Metrics
[block:parameters]
{
  "data": {
    "0-0": "**Email Sent** ",
    "1-0": "**SMS Delivered**",
    "2-0": "**SMS Credits Used**",
    "1-1": "The total number of SMS delivered by all campaigns associated with this account.",
    "0-1": "The total number of email sent by all campaigns associated with this account. Determines email channel costs.",
    "h-0": "Usage Metric",
    "h-1": "Definition",
    "2-1": "The total number of SMS credits used by all campaigns associated with this account. Determines SMS channel costs.\n\nNote that **SMS Credits Used** may be greater than **SMS Delivered**. SMS credit usage is based on message length, language, and character encoding scheme."
  },
  "cols": 2,
  "rows": 3
}
[/block]
## Exporting the Usage Report
Export a CSV file containing a report with your account's usage data by selecting the **Email** or **SMS** tab, then clicking the **Export Report** button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d3e51d5-usage-info-export-report.png",
        "usage-info-export-report.png",
        1083,
        520,
        "#eff0f5"
      ]
    }
  ]
}
[/block]
In the **Export Usage Report** modal that appears, complete the following:
1. Specify a date range. Usage data is unavailable for dates before April 1, 2022.
2. Enter up to 10 emails that you want the report download link to be sent to.
3. Click **Export**. When the report is ready, an email containing the download link for the report will be sent to the email(s) you specified. It takes one to two days for the download link to be sent.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/14f611c-export-usage-report.png",
        "export-usage-report.png",
        630,
        511,
        "#f1f2f4"
      ],
      "sizing": "80"
    }
  ]
}
[/block]
The email containing your usage report's download link also includes your business name, the channel type, and date range of the data.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/11b0dd7-usage-info-email-content.png",
        "usage-info-email-content.png",
        702,
        541,
        "#f9fafa"
      ],
      "sizing": "80"
    }
  ]
}
[/block]

---

## Date Range Notes and Limitations
* **Usage data is only available starting from April 1, 2022**.
* Only data from the past 12 months can be viewed on the AIQUA Dashboard. This means that the start date cannot be more than 12 months before the current date. This limitation doesn't apply to usage report exports.
* Usage data for each date is based on the time zone configured in your account settings.