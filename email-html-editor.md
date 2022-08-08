---
title: "Email HTML Editor"
slug: "email-html-editor"
hidden: false
createdAt: "2021-09-28T09:27:00.287Z"
updatedAt: "2022-02-08T07:18:00.167Z"
---
AIQUA's Email HTML Editor allows you to create and preview email templates made with custom HTML. You can choose to save your custom HTML as a [template](doc:email-templates) which can be reused in future campaigns.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/56325f7-Screen_Shot_2022-02-08_at_3.17.45_PM.png",
        "Screen Shot 2022-02-08 at 3.17.45 PM.png",
        1213,
        633,
        "#38413f"
      ]
    }
  ]
}
[/block]
# Usage Notes
To avoid unexpected differences between the in-editor preview, the thumbnail preview in the AIQUA Dashboard, and your final campaign email, make sure to:
* [Declare a character encoding scheme](#declare-a-character-encoding-scheme).
* [Check that the HTML used in your template is supported](#check-for-supported-html-tags-and-css-properties) by the email client(s) you are targeting.

## Declare a Character Encoding Scheme
If your HTML template doesn't include a character encoding declaration, non-English text may not display properly in your campaign email. To prevent this issue, include an encoding declaration in your HTML's `<head>` tag specifying an encoding scheme.

For example, an encoding declaration specifying the `utf-8` scheme looks like this:
```html
<head>
    <meta charset="utf-8">
</head>
```

and a full HTML file using the `utf-8` encoding looks like this:
[block:code]
{
  "codes": [
    {
      "code": "<html>\n  <head>\n    <meta charset=\"utf-8\"> <!-- Encoding declaration -->\n    <title>AIQUA Email HTML Editor</title>\n  </head>\n  <body>\n    <p>The text on this page is encoded in UTF-8.</p>\n  </body>\n</html>",
      "language": "html"
    }
  ]
}
[/block]
## Check for Supported HTML Tags and CSS Properties
Not all email clients support all available HTML tags and CSS properties, so it's important to check if your custom HTML contains tags or properties unsupported by a particular email client.

Tools like <a href="https://www.caniemail.com" target="_blank">www.caniemail.com</a> allow you to search for specific HTML tags, CSS properties, or email clients to determine if your custom HTML will be fully supported, as seen below.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/451cb94-can-i-email.png",
        "can-i-email.png",
        2298,
        1362,
        "#304251"
      ],
      "caption": "The `background-color` property is supported by all Apple Mail, Gmail, and Outlook email clients."
    }
  ]
}
[/block]
Searching by tag or property name returns columns of email providers (such as **Gmail** and **Outlook**) and sub-columns of email clients from that provider (such as **iOS** and **Android**). Email clients that support the tag or property you specified will contain a green box with a checkmark.