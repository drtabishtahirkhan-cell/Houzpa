# HOUZPA backend (Supabase)

Live project: `zgevzsifxvjizyfsljlx` (Mumbai, ap-south-1)

## Edge functions (deployed)
| Function | Login needed | What it does |
| --- | --- | --- |
| `create-order` | yes | Checks the guest (verified, age), the party and passes left, creates a Razorpay order and a `pending_payment` booking. Free passes are confirmed directly. |
| `verify-payment` | yes | Checks Razorpay's signature (HMAC with the secret key) and confirms the booking. |
| `razorpay-webhook` | no (checks Razorpay webhook signature) | Confirms the booking even if the guest closed the app after paying. |

The source code of each function can be read in Supabase → Edge Functions → (function) → Code.

## Secrets to set (Supabase → Edge Functions → Secrets)
- `RAZORPAY_KEY_ID`: from Razorpay → Account & Settings → API Keys (`rzp_test_...` while testing)
- `RAZORPAY_KEY_SECRET`: shown once when you generate the key. Never put it in `index.html`.
- `RAZORPAY_WEBHOOK_SECRET`: any strong password you type when adding the webhook in Razorpay

Webhook URL for Razorpay: `https://zgevzsifxvjizyfsljlx.supabase.co/functions/v1/razorpay-webhook`
Events: `payment.captured`, `order.paid`

## Database
- `confirm_booking(order_id, payment_id)`: locks the pass row so the last pass can never be sold twice. Only server code can call it.
- Admins can mark users as verified (policy `admin updates profiles`).
