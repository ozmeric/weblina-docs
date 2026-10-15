# BuildFlow customer demo script

A 15 to 20 minute demo of BuildFlow + Foreman AI for a prospect. The examples use the Ridgeline Renovations template; a build made with `buildflow-foreman` uses the prospect's own trade and sample names.

## Before the demo

### The day before

- [ ] Run `buildflow-foreman [Company Name]` so the demo shows **their** name, colors and services.
- [ ] Upload the build to GitHub Pages, for example `theircompany.usebuildflow.io`.
- [ ] Decide on demo mode or live mode:
  - **Demo mode** (`apiUrl` empty) is safe and always works, but uses simulated AI.
  - **Live mode** sends real emails and posts and makes real AI calls. Use it for the "wow" moments.
- [ ] In live mode, check that the Worker URL shows `{"ok":true,"app":"BuildFlow Demo backend"}` and that `testGemini`, `testFacebook` and `testCalendar` pass.
- [ ] Add 4 to 6 job photos to **Social posts → Settings → Your photos**, plus one **before & after pair**.

### 15 minutes before

- [ ] Open the app and enter the PIN. This wakes the Google backend, because the first request after a quiet period can be slow.
- [ ] Press **Reset** (top right) so the sample data is fresh.
- [ ] Have your phone ready to scan the quote form QR code, plus a personal email address that isn't the business Gmail.
- [ ] Turn **Autopilot** on in Leads and set Social posts to **Ask me first**.
- [ ] Open the app on its own page, not the claude.ai preview link. The microphone doesn't work inside the preview.

## The demo

### 1. Open with their problem (1 minute)

Ask what they told you on the first call: how leads come in, how fast they reply, and who does social media. Then say:

> "BuildFlow is your business in one app, with an assistant called Foreman that handles the office work: answering leads, posting to Facebook and Instagram, and keeping your calendar. You stay in control of anything that goes out."

### 2. Home: the daily brief (2 minutes)

**Show:** the navy **Daily brief** card, the four number tiles, project cards, Coming up, and AI activity.

**Say:**

> "Every morning you open this and Foreman tells you what needs you: overdue tasks, a job going over budget, new leads, bids waiting on a decision, and today's appointments. Each line takes you straight there."

**Click** one brief item (for example **See budget →**) to show it jumps to the right place.

### 3. A project (3 minutes)

**Click:** a project card, then go through the tabs.

| Tab | Point to make |
|---|---|
| **Dashboard** | Contract value, estimated cost, spent to date, profit and margin. *"On track?"* compares schedule against budget. |
| **Tasks** | Tap the check to complete a task; tap the status pill to change it. Overdue tasks show in red. |
| **Expenses** | Log an expense in seconds. *"Next we're adding Home Depot and Floor & Decor receipts automatically."* |
| **Planning** | Milestones, with progress bars from the linked tasks. |
| **Quotes** | Compare bids side by side. **Accept** one, and it rejects the others and logs the expense automatically. |

### 4. Leads: the wow moment (4 minutes)

**Say:**

> "Most contractors lose jobs because they reply the next day. Watch this."

1. Press **Live quote form**, scan the QR code with your phone, and fill it in with your personal email.
2. Within about 15 seconds the lead appears with a pop-up: **"New lead from …"**.
3. With Autopilot on, show the AI reply on the lead, then open your phone's inbox: the email is already there, offering consultation times.
4. Turn **Autopilot off** and press **Sample lead**. This time the reply waits as a draft. Edit a word and press **Approve & send**.
5. Press **Schedule consultation** on the lead. It opens a 45-minute event with the lead already invited.

> "Autopilot on means instant replies day or night. Off means Foreman writes it and you approve it. Your choice."

**Demo-mode fallback:** press **Simulate a new lead** and show the same flow with sample data.

### 5. Social posts (3 minutes)

1. **Create a post:** pick a project, choose **Before & After**, add the two photos, and press **Write it for me**. Show the combined before/after image and the caption in the preview.
2. **Autopilot posting:** open **Settings** and show **Foreman picks the best times** (for example, Tue 9:30 AM, Thu 9:30 AM, Sat 9 AM for Facebook).
3. Press **Turn on & plan 2 weeks**. The content calendar fills with posts, each with a photo, caption, day and time.
4. Click one post to preview it, then press **Approve**.

> "Two weeks of posts in one click, with your photos, at the times homeowners are most active. With Ask me first, nothing goes out until you approve it."

In live mode, publish one with **Post now** and open **View on Facebook**.

### 6. Calendar (1 minute)

**Show:** the week view, with milestones shown on their dates and each guest's reply status (Accepted, Invited, Maybe).

> "It's your real Google Calendar. Invites and changes email your customers automatically."

### 7. Ask Foreman, by voice (3 minutes)

**Tap the microphone** and say:

1. "What needs my attention today?"
2. "Are any jobs over budget?"
3. "Add a $450 tile expense to Hawthorne from Tile Gallery." When the card shows, say **"apply"**.
4. "Schedule a consultation with Ashley Romero Thursday at 2pm." Then say **"apply"**.

> "You can run your business from the truck. Ask a question or tell it what to change. It shows you first and only acts when you say yes."

### 8. Close (2 minutes)

> "Everything you saw is set up with your name and your services. Your data lives in your own Google account. We do the setup with you on a 20-minute call."

**Ask:**

- "Which part would save you the most time: leads, social, or job tracking?"
- "Who else should see this before you decide?"

**Next step:** send the **Customer Setup Guide** (`buildflow-customer-setup-guide.md` or the PDF) and book the setup call.

## Questions you'll get

| Question | Answer |
|---|---|
| "What if the AI says something wrong?" | Leave Autopilot off and approve every reply and post. Turn it on later once you trust it. |
| "Who owns my data?" | You. It's in a Google Sheet in your account, and you can download it any time. |
| "What does it cost?" | Setup, plus a monthly support plan. Running costs are usually a few dollars a month for the AI (see the setup guide). |
| "Do I need to be technical?" | No. We set it up with you on one call, and you click Allow. |
| "Does it work on my phone?" | Yes, including voice. Add it to your home screen like an app. |
| "Can it run ads?" | Ads management is planned and can be turned on per customer. |
| "Can it track Home Depot spending?" | Receipt emails can be turned into job expenses automatically. This is being added now. |

## If something goes wrong

| What you see | Quick fix |
|---|---|
| "Wrong PIN" | Run `testPin` in Apps Script and check the `APP_PIN` script property. |
| "Unauthorized" | The Worker points to an older BuildFlow script. Point it at this demo's `/exec` URL. |
| First action fails, second works | The backend was asleep. Open the app 15 minutes early. |
| Microphone blocked | You're in the preview link. Open the app on its own page, and use Chrome, Edge or Safari. |
| Lead email didn't arrive | Check spam. Sample leads and `@example.com` addresses are never emailed. Use a real personal address. |
| Gmail lead not showing | Press **Check Gmail** on the Leads page. |
| Data looks messy | Press **Reset** at the top right. |
