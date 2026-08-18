# CHIP Payment Gateway for Arastta eCommerce

CHIP payment gateway module for [Arastta eCommerce](https://arastta.org) 1.6.x.

Accept payments via FPX, FPX B2B1, Mastercard, Maestro, Visa, Atome, GrabPay, Maybank QR, ShopeePay, Touch 'n Go, DuitNow QR and Crypto Coin through [CHIP Collect](https://www.chip-in.asia).

## Features

- Hosted payment page (CHIP Collect checkout) — no card data touches your store
- Webhook callback (`success_callback`) with X-Signature verification (RSA, `sha256WithRSAEncryption`)
- Success / cancel / failure redirect handling with order status updates
- Payment method whitelist with clean labels (FPX, FPX B2B1, Mastercard, Maestro, Visa, Atome, GrabPay, Maybank QR, ShopeePay, Touch 'n Go, DuitNow QR, Crypto Coin)
- DuitNow QR / ShopeePay group resolution against `/payment_methods/` (legacy `duitnow_qr`/`razer_shopeepay` keys auto-migrated to `dnqr`/`shopee_pay`)
- `chip_report` table for purchase tracking (order, purchase ID, status, amount, environment, date)
- Admin report tab with pagination
- Per-language payment name and checkout instruction
- MYR currency conversion support (`convert_to_processing`)
- Debug logging

## Installation

1. Copy the `catalog/` and `admin/` folders into your Arastta root (merge with existing folders).
2. In the admin, go to **Extensions → Payments** and click **Install** on the CHIP module.
3. Click **Edit** and configure:

| Setting | Description |
|---------|-------------|
| Secret Key | From CHIP Collect Dashboard (Settings → API Keys) |
| Brand ID | From CHIP Collect Dashboard (Settings → Brand) |
| Payment Method Whitelist | Leave empty to allow all methods |
| Paid Order Status | Default: Complete |
| Canceled Order Status | Default: Canceled |
| Failed Order Status | Default: Failed |
| Refunded Order Status | Default: Refunded |
| Pending Order Status | Default: Pending |
| Due Strict | Recommended: Yes |
| Due Strict Timing | Minutes before payment expires (default 60) |
| Time Zone | Default: Asia/Kuala_Lumpur |
| Convert To Processing | Yes if store currency is not MYR |

4. Set **Status** to Enabled and **Save**.

## Webhook

The module uses the success callback URL shown in the admin (API details tab):

```
https://your-store.com/index.php?route=payment/chip/success_callback
```

The callback verifies the `X-Signature` header against the general public key fetched automatically from the CHIP API using your secret key. No manual public key configuration is required.

## Compatibility

- Arastta 1.6.2 (PHP 5.4+ / 7.x)
- Requires `curl` and `openssl` PHP extensions
- Requires MYR currency to be enabled in the store (or `Convert To Processing` enabled)

## Files

```
catalog/controller/payment/chip.php
catalog/model/payment/chip.php
catalog/language/en-GB/payment/chip.php
catalog/view/theme/default/template/payment/chip.tpl
admin/controller/payment/chip.php
admin/model/payment/chip.php
admin/language/en-GB/payment/chip.php
admin/view/template/payment/chip.tpl
admin/view/template/payment/chip_report.tpl
```

## License

GNU GPL version 3 — see LICENSE.txt.
