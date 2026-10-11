---
name: gmail-send
description: 代使用者透過 Gmail 寄信或回信時的固定規則。凡是要寄出（send_message、reply、forward）任何郵件前都必須先讀此 skill。
---

# Gmail 寄信規則

## 必做：副本寄回使用者本人

每次代使用者寄出郵件（新信、回信、轉寄）時，一律在 `cc` 加入使用者本人的信箱
（即 userEmail context 提供的地址），讓使用者能在收件匣確認寄信成功。

- 使用 `reply` 時要明確帶入 `cc`，並以 `to` 指定原寄件人，避免預設收件人被覆寫後漏掉對方。
- 若使用者本人已在 `to` 中，則不需重複加入 `cc`。

## 寄出後驗證

寄出後用 `get_thread`（`METADATA_ONLY`）確認：

1. 新訊息帶有 `SENT` 標籤。
2. `ccRecipients` 含使用者本人信箱，且該訊息同時出現在 `INBOX`（副本已送達）。

確認後再回報使用者「已寄出並收到副本」；若驗證失敗，照實回報。

## 其他

- 內文用純文字 `body`，不要使用 Markdown 格式。
- 只有在使用者明確說「寄出」時才寄；否則只在對話中提供草稿或建立 Gmail 草稿。
