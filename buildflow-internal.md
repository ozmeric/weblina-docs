# BuildFlow: internal notes

> **Internal. Do not share with customers.** Covers the receipt import checklist, setup access, and the hosting risks for Weblina.

## 1. Home Depot and Floor & Decor receipts into Expenses

Neither store offers an API for contractor purchases. Instead, the app reads **receipt emails** through the existing Gmail connection. Gemini pulls out the store, date, items, tax and total, matches the purchase to a job, and adds an expense for review.

### What I need to provide

| # | Item | How to get it |
|---|---|---|
| 1 | 2 or 3 real receipt emails from each store, ideally including a return or refund | In Gmail, open the email, use the ⋮ menu, and choose **Download message** (.eml). A PDF or screenshot also works. |
| 2 | The sender address of each store's receipts | Usually visible in the files; just confirm it. |
| 3 | Which Gmail inbox receives them | It must be the Google account that runs the Apps Script. If it isn't, set up auto-forwarding to that account. |
| 4 | Whether the crew enters a job name or PO at checkout, and what they type | Home Depot Pro Xtra allows a job name or PO at the register. Matching works best when it matches the project name in the app. |

### Decisions (defaults)

| Decision | Default | My choice |
|---|---|---|
| Review or automatic | Each receipt shows as **Needs review** with **Approve**, like AI lead replies. Optionally, clear job matches are added automatically. | |
| Amount | The total including tax becomes one expense, with the item list in the notes. Returns become negative expenses on the same project. | |
| Category | Home Depot under Materials. Floor & Decor under Materials, or a new Flooring & Tile category. | |
| Old receipts | Import the last 30 days on the first run, or only new receipts. | |
| Project data in the Sheet | Required. In live mode, projects and expenses move from the browser to the customer's Google Sheet. Demo mode keeps the sample data. | OK / not OK |

Also possible later: a Pro Xtra CSV purchase-history import, which is good for catching up on past jobs ([FlipperForce guide](https://flipperforce.com/house-flipping-blog/how-to-bulk-import-your-home-depot-purchase-history)), and photo receipts for any store.

## 2. Setup access per customer

- **The customer's own Google account must run `setup()` and Deploy.** Apps Script sends email and creates triggers as the person who authorized it, so emails only come from the customer's Gmail if they click Allow and Deploy themselves. Do this on a screen share. Afterwards we only need Editor access on the Sheet.
- **Use the paid Gemini tier for customer data.** Google's pricing page says free-tier content is "used to improve our products" and paid-tier content isn't ([Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)).
  - Gemini 3.8 Flash costs $0.75 per million input tokens and $3.75 per million output tokens through Dec 31, 2026, then $1.50 and $7.50.
  - Gemini 3.5 Flash-Lite costs $0.30 per million input tokens and $2.50 per million output tokens.
- **Free Gmail has lower Apps Script limits** ([Apps Script quotas](https://developers.google.com/apps-script/quotas)).

  | Limit | Free Gmail | Workspace |
  |---|---|---|
  | Email recipients a day | 100 | 1,500 |
  | Trigger runtime a day | 90 min | 6 hours |
  | URL fetches a day | 20,000 | 100,000 |

  A busy customer on free Gmail can hit the trigger limit with 1-minute Gmail checks. Recommend Workspace, or keep the checks at 5 minutes.
- **Posting to customers' Facebook Pages from Weblina's Meta app needs Advanced Access**, which means App Review plus Business Verification. Until that's approved, Standard Access only works on assets your own account owns ([overview](https://bundle.social/blog/facebook-api-permissions)). Workaround: create the Meta app inside each customer's own Business portfolio.
- **Hosting option C (Google only)** serves the app from Apps Script using HtmlService. That needs adaptation work, and it loses the custom domain, installing as a phone app, and reliable microphone access.
- **Option B can be simpler.** Cloudflare Pages supports drag-and-drop upload (up to 1,000 files, 25 MiB each), and a `_worker.js` file can act as the connector. That makes it one project instead of Pages plus a separate Worker ([Pages direct upload](https://developers.cloudflare.com/pages/get-started/direct-upload/)). Test this before offering it.
- **It may be possible to drop the Worker entirely.** The app might be able to call the Apps Script `/exec` URL directly, because requests are sent as `text/plain`. This isn't tested yet. The trade-off is that the script URL becomes visible, though the PIN still protects it.

## 3. Risks if Weblina keeps GitHub and Cloudflare (option A)

Hosting for customers is a good recurring service, but every customer then depends on two Weblina accounts.

| Risk | What could happen | How to reduce it |
|---|---|---|
| **Account lockout or takeover** | A suspended, unpaid or hacked GitHub or Cloudflare account takes every customer's app down. An attacker could also change the app code, for example to capture PINs. | Use business-owned accounts, not personal ones. Add two-factor with a security key and a backup admin. Use least-privilege tokens and GitHub branch protection, and review the audit logs. |
| **Shared free limits** | Workers Free allows 100,000 requests a day per account, shared by all customers ([limits](https://3b21a3e4.previews.developers.cloudflare.com/workers/platform/limits/index.md)). One open app makes about 330 requests an hour (15-second refresh, 1-minute Gmail check, 2-minute calendar), or about 2,600 per 8-hour day. That's roughly 35 to 40 all-day devices across all customers. | Move to Workers Paid ($5 a month minimum, 10 million requests a month included, per [pricing](https://developers.cloudflare.com/workers/platform/pricing/)) once you have about 10 live customers, or slow the refresh rates. Price it into support. |
| **Your code is public** | Free GitHub Pages requires a public repo, so anyone can copy the BuildFlow app ([discussion](https://github.com/orgs/community/discussions/22817)). | Use private repos on a paid GitHub plan (GitHub Team is $4 per user per month for the first 12 months, per [pricing](https://github.com/pricing)), or host on Cloudflare Pages, which supports private repos and direct upload. |
| **You handle customer data** | Leads, photos and PINs pass through your Worker, which makes you a data processor. | Turn off logging in the Worker. Add a privacy policy and a data clause to the contract. Keep keys in the customer's Script properties. |
| **Lock-in disputes** | Customers' links and QR codes depend on your domain (usebuildflow.io), so leaving gets messy without a plan. | Put license, ownership and a handover process in the contract (30 days' notice, file handover). Offer customers their own domain from day one. |
| **Costs you carry** | Cloudflare Paid, the GitHub plan and domain renewals add up as you grow. An expired domain takes every customer down. | Auto-renew usebuildflow.io. Include hosting in the monthly fee and review costs each quarter. |
| **Platform changes** | Google retires models (2.5 was restricted for new users), Meta changes Graph API versions every year, and quotas change. Customers will call you. | Include maintenance in support plans. Run the test functions monthly and watch the Google and Meta changelogs. |
| **Content liability** | Foreman sends emails and posts in the customer's name. A wrong reply or claim reflects on them, and on you. | Default to **Ask me first**. The contract states that the customer approves content. Sample addresses are never emailed (already built). |
| **Meta app dependency** | If your shared Meta app is restricted, every customer's posting stops. | Complete Business Verification and keep the app compliant, or use one Meta app per customer. |

> **Recommendation:** offer A as the default, but run it like a real hosting business:
> - Use a business GitHub organization and Cloudflare account, protected with security keys.
> - Keep the repos private.
> - Move to Workers Paid once you have about 10 live customers.
> - Put a hosting and data clause in every contract.
> - Keep options B and D ready for customers who want to own everything.

## Sources

Opened October 15, 2026.

- [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- [Apps Script quotas](https://developers.google.com/apps-script/quotas)
- [Cloudflare Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/) and [limits](https://3b21a3e4.previews.developers.cloudflare.com/workers/platform/limits/index.md)
- [Cloudflare Pages direct upload](https://developers.cloudflare.com/pages/get-started/direct-upload/)
- [GitHub pricing](https://github.com/pricing) and [GitHub Pages on private repos](https://github.com/orgs/community/discussions/22817)
- [Google Workspace pricing](https://www.emailvendorselection.com/google-workspace-pricing/)
- [Meta API permissions overview](https://bundle.social/blog/facebook-api-permissions)
- [Home Depot purchase history export](https://flipperforce.com/house-flipping-blog/how-to-bulk-import-your-home-depot-purchase-history)

*Prices and limits change, so confirm them before quoting.*
