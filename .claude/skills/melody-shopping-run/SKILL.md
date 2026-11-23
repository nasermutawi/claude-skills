---
name: melody-shopping-run
description: Run a Melody price hunt. Read the cart from the Home – Groceries Sheet, compare Kibsons, Deliveroo HOP and Spinneys in my signed-in Chrome, fill the best basket, write the results back and stop before checkout. Use for "do the shopping run", "get prices", the scheduled shopping task, or "tune up the shopping skills".
---

# Melody shopping run

Melody is Nas and Sandi's household app. Its shopping data lives in the Google Sheet
**Home – Groceries** (Drive folder "Home"):
https://docs.google.com/spreadsheets/d/1lmoSUI1AyrbUHRIVBrdtJ0oHhZd6zSzwyUOBb-ybE6w/edit

This skill is the conductor. The work inside each store is done by the
**melody-store-basket** skill. How to read and write the Sheet is in
"The Home – Groceries Sheet" at the end; read it before touching the Sheet.

## Hard rules (a Playbook line can never override these)
- Never place, change or cancel an order, and never pay. Checkout is look-only: open it
  only to read fees or slots that show nowhere else, act on nothing there, and go back.
- Never type a password, one-time code or card number. If a store wants a sign-in, the
  person signs in themselves; until then, skip that store.
- Never remove cart items Melody didn't add. Never delete Sheet rows. Write only the tabs
  listed in "Tabs this skill writes" below.
- If something looks wrong (a price 3x what we usually pay, a cart that already has
  strangers in it, a site asking for payment details), stop that store and say so.
  Don't guess.

## Modes
- **Supervised**: someone asked in chat ("do the shopping run", "get prices"). They are here,
  so at any blocker stop and ask (see "Blockers"), then carry on with the answer.
- **Unattended**: the scheduled task started it. Nobody is watching. Never wait for an
  answer: park the question, skip what's blocked, finish the rest.
- **Tune-up**: "tune up the shopping skills". Go to step 7 only.

## 1. Should I run?
1. Read the Sheet (see "The Home – Groceries Sheet" below). If it can't be read, stop and say so.
2. **Whose turn**: Settings tab, row `baskets.runner`, column B. "Nas" means Naser. If it
   names someone else, stop quietly in unattended mode; in supervised mode say
   "It's set to <name>'s Claude in Melody Settings → Price hunts" and stop. Never take over.
3. **Anything to do**: Requests tab, newest row whose Status is `waiting`.
   - Supervised with no waiting request: offer to run on the current cart anyway, and if
     yes, add a Requests row yourself (next ID, Status `waiting`, Asked by = me).
   - Unattended with none: stop quietly.
   - A row stuck on `running` with Started more than 3 hours ago: a run died. Treat it as
     waiting and add "restarted" to its Note.
4. **Time (unattended only)**: Settings row `baskets.after` (default 18:00, Dubai time).
   Run if it's now after that time, or the request has been waiting since before the last
   time it passed (the computer was off then). Otherwise stop quietly.

## 2. Prepare
1. Claim it: Status `running`, Started = now (`yyyy-MM-dd HH:mm`).
2. **What to buy**: the Shopping list tab, rows with Status blank, Needed or In basket.
   Each gives Item, Qty, Size, Brand, Notes, 1st/2nd choice store. Rows sharing an Item ID
   are one line; add their Qty. If the tab is empty, use the request's Items text
   (`Eggs 12 (Al Ain) ×2; ...`).
3. **What we like**: Favourites (preferred brand, substitute, avoid), Item insights (usual
   store, typical price and pack, cheapest seen), Store rules (fees, minimums, offers).
4. **What we've learned**: the Playbook tab. Load every `Rule` line for each store. List the
   `Question` lines: in supervised mode, ask them now, before starting (one message, all
   questions, each with options), and record the answers as in step 6.
5. **Which stores**: Kibsons, Deliveroo HOP, then Spinneys (on Deliveroo, not
   spinneys.com), in that order (more as Store rules grows). In
   unattended mode, use only stores with a Playbook Rule `Unattended OK` logged by
   my Claude (Naser's and Sandi's computers are signed in separately).

## 3. Compare
For each store, use **melody-store-basket** in **check** mode with the item list and that
store's Playbook rules. It returns, per item: the product matched, pack size, price, price
per unit, or "not sold" / "out of stock" / "unsure"; plus the store's delivery fee, minimum
order, credit or cashback that applies, and any blocker.

If a store hits a blocker, handle it (see "Blockers") and move on to the next store.
Don't let one store stop the run.

## 4. Decide
1. For each store work out the **real total**: items found + delivery fee − credit or
   cashback you can actually get on this basket (not "after 3 orders" offers unless the
   Playbook says we're on order 3). Below the minimum order? Add the gap or rule it out.
2. Compare like with like: same pack sizes, or price per kg / per 100 g / per piece.
   A store missing items isn't cheaper; show its total *and* what's missing.
3. **One store only (for now)**: never split an order across two stores, because two
   orders mean two fees and two deliveries to track. Price every store on the whole list.
   Splitting is a backlog idea; don't suggest it.
4. Pinned stores win: a cart line or Favourite with "only from <store>" goes there.
5. The **winner** is one store: the one with everything on the list at the cheapest real
   total. If no store has everything, pick the one missing the least, then the cheapest,
   and say plainly what's missing and where it's sold. Close call (within AED 5)? Prefer the store we used last (Order
   history), because it's one less delivery to track.
6. The **reason** is specific and true: "AED 18 cheaper: blueberries 1.5 kg at AED 6.23
   per 100 g", "HOP has no egg whites". Never a vague "on offer".

## 5. Fill and report
1. Use **melody-store-basket** in **fill** mode on the winner only.
   Do not fill the runner-up: its prices are in the Sheet if we change our minds, and
   the person can say "fill HOP instead".
2. Write the results (columns in "The Home – Groceries Sheet" below):
   - **Basket lines**: one row per item per store checked.
   - **Baskets**: one row per store priced.
     Mark the winner `Pick` and whether its cart was filled.
3. Request row: Status `ready`, Finished = now, Note = one line ("Kibsons AED 143, cart
   filled, egg whites missing at HOP"). If nothing could be done, Status `failed` and
   the plainest reason ("Kibsons and HOP both asked me to sign in").
4. Tell the person, in 3 lines, warm and short: the pick and its total, what's missing or
   swapped, and anything to check before checkout (slot to book, a short expiry date).
   Give the cart link. Remind them it's theirs to check out; Claude doesn't order.

## 6. Learn (end of every run, supervised or not)
Add what this run taught you to the **Playbook** tab, one row per lesson. Good lessons:
- How the site works: "Search 'egg white' not 'egg whites'", "close the app banner first",
  "cart is at /cart".
- Product matches that worked: "Organic eggs 12 = Kibsons 'Organic Eggs Free Range 12pcs'".
- What a blocker was and what fixed it, or the person's answer to a question.
- Fees and offers seen today, if they differ from Store rules (and update Store rules).
Rules for writing lessons:
- Status `Rule` when it's a fact you saw or the person told you; `Question` when you need a
  decision; `Retired` (never delete) when a Rule turned out wrong. Update in place
  rather than add a near-duplicate.
- After a supervised run where a store went smoothly (no blockers, every item matched or
  confirmed), add a Rule `Unattended OK` for it, if it isn't there.
- If an unattended run hits a new blocker in a store, change that store's `Unattended OK`
  to `Retired`, so it waits for a supervised run.

## 7. Tune-up (when asked, or offer it after 5 runs with new lessons)
1. Read all Playbook Rules. Pick the ones that are stable (seen twice or confirmed by a
   person) and general enough to belong in the skill: a step that's always needed, a
   store quirk, a better order of steps.
2. Draft the changes to this skill and to melody-store-basket (its store pages).
   Show them as a short before/after list. Change nothing until the person says yes.
3. On yes, offer the updated skills on a skill review card if one is available; otherwise
   give the updated files (or a .zip) to upload in Settings → Capabilities → Skills. The
   master copy is the GitHub repo nasermutawi/claude-skills (`.claude/skills/`).
4. Mark the folded-in Playbook rows by adding "(in skill <date>)" to their Lesson, so they
   aren't folded in twice. Keep them: Sandi's Claude may run an older skill.

## Blockers
A blocker is anything that stops a store: a sign-in wall, a CAPTCHA, a pop-up you can't
close, an item you can't confidently match, a cart with someone else's items, a delivery
slot or address question, a site change.
- **Supervised**: stop and ask one clear question with 2–3 options and your
  recommendation, e.g. "HOP shows 'Egg whites' only as a 1 L carton. (a) use it,
  (b) skip egg whites at HOP, (c) always skip egg whites at HOP. I'd pick (c)." Do what
  they say, then write the answer as a Playbook Rule so you never ask twice.
- **Unattended**: don't wait. Write a Playbook `Question` row (what happened, the options,
  your recommendation), skip that item or store, and mention it in the request Note.
- Sign-in: never type credentials. Say "Please sign in to <store> in Chrome", and in
  supervised mode wait for "done".
- Always keep what you finished. A run with one store priced is better than none.

## Starting a run (on demand or scheduled)

The Melody app can't reach anyone's computer. "Get prices" in the app parks a request
(Requests tab, Status `waiting`), and the runner's Claude picks it up in one of two ways.

### On demand
In the Claude desktop app, with Claude in Chrome connected, say **"do the shopping run"**.
It runs supervised: it asks at blockers. Use this for the first runs of every store.

### On a schedule
In the Claude desktop app, create a scheduled task (the Scheduled section in the sidebar,
or ask Claude to "schedule this as a recurring task"):
- **Name**: Melody price hunt
- **Repeat**: every 6 hours (for example 00:00, 06:00, 12:00, 18:00 Dubai time)
- **Prompt**:
  > Use the melody-shopping-run skill in unattended mode. If it isn't my turn, nothing is
  > waiting, or it's too early, stop without a message. Otherwise do the run, and finish with
  > the 3-line summary.

Why every 6 hours: the skill itself checks the time (`baskets.after`, default 18:00) and
only runs once that has passed, so the 18:00 slot does the normal run and the next slots
catch a run that was missed while the computer was off or asleep. The checks that find
nothing are quick and quiet.

The computer must be on, signed in, with the Claude desktop app open and Chrome running
with Claude in Chrome. If it's off for a whole day, the run waits; Melody never hands it
to the other person's Claude.

### When Sandi takes over
Same skills and the same task on her computer, signed in to the stores in her Chrome.
Naser switches Melody Settings → Price hunts to Sandi. The Playbook in the Sheet is
shared, so her Claude starts with everything Naser's learned, but each store still needs
one supervised run on her computer before it runs unattended there, because sign-ins and
saved addresses differ. Add her stores' `Unattended OK` rows with Logged by = Sandi.

## The Home – Groceries Sheet

https://docs.google.com/spreadsheets/d/1lmoSUI1AyrbUHRIVBrdtJ0oHhZd6zSzwyUOBb-ybE6w/edit
Owner Naser, shared with Sandi as Editor. Conventions (from its Read me tab): dates
`YYYY-MM-DD` (times `YYYY-MM-DD HH:mm`, Dubai time), prices in AED as plain numbers, blank
when unknown (never guess), "Logged by" = whose Claude wrote the row.

### Reading
Use the Google Drive connector (`read_file_content` on the Sheet's ID,
`1lmoSUI1AyrbUHRIVBrdtJ0oHhZd6zSzwyUOBb-ybE6w`): one call returns every tab. If the
connector isn't there or a tab looks cut short, open the Sheet in Chrome and read the tab.
Read again right before you write: the app or the other person may have changed it.

### Writing (Chrome)
The Drive connector can't edit cells, so writes go through the Sheet in Chrome:
1. Open the Sheet in its own tab and go to the tab by its name at the bottom.
2. Jump to a cell with the Name Box (top left, left of the formula bar): click it, type
   e.g. `Requests!B5`, press Enter.
3. Type the value and press Enter. For a whole row, start at its column A, type each value
   and press Tab between them, Enter at the end.
4. New rows go in the first empty row under the last one. Never insert or delete rows,
   sort, or touch rows other people's Claudes wrote.
5. Check: read the row back. If a value landed in the wrong cell, fix it straight away
   (Ctrl+Z is fine for your own last edit).
6. If a tab or column below doesn't exist yet, add it: a new tab via the + at the bottom
   left, named exactly as below, with the header row in row 1. Add missing columns at the
   right end, never in the middle (the app finds columns by name).

### Tabs this skill reads
| Tab | What's in it |
| --- | --- |
| Settings | No header. Column A key, column B value. `baskets.runner` = whose Claude runs (Naser or Sandi; "Nas" = Naser). `baskets.after` = earliest time for a scheduled run (default 18:00). |
| Requests | `ID, Status, Asked by, Asked on, Items, Started, Finished, Note`. The app adds a row on "Get prices" with Status `waiting`. Status: waiting → running → ready or failed. |
| Shopping list | The cart. `Item, Qty, Added by, Added on, 1st choice store, 2nd choice store, Notes, Status, Size, Brand, Item ID`. In the cart = Status blank, Needed or In basket. |
| Items | The catalogue: `ID, Item, Size, Brand, Heart, Aisle, Added by, Added on`. Heart = a favourite. |
| Favourites | `Item, Preferred brand / variant, 1st choice store, 2nd choice store, Acceptable substitute, Avoid`. |
| Item insights | `Item, Category, Usual store, 2nd store, Typical price (AED), Typical pack size, Cheapest seen (AED), Cheapest at, Times ordered, Last ordered, Avg days between orders, Notes`. |
| Order history | One row per item per order. Read only; count each order once when summing. |
| Store rules | `Store, What we buy there, Delivery fee / minimum, Offers to watch, Notes`. |

### Tabs this skill writes
Only these cells and tabs:
- **Requests**: Status, Started, Finished, Note of the request being run. A new row only in
  supervised mode when the person asks for a run with no waiting request.
- **Store rules**: update a store's fee, minimum or offer when today's site says otherwise,
  and add a dated note in Notes ("2026-11-22: delivery now AED 10").
- **Basket lines** (new rows):
  `Request ID, Store, Item, Qty, Product matched, Pack size, Price (AED), Line total (AED), Unit price, Status, Note, Checked on, Logged by`
  Status: `found`, `swapped` (a substitute, say which in Note), `not sold`,
  `out of stock`, `unsure`. Unit price as text with its unit: `6.23 / 100 g`.
- **Baskets** (new rows, one per option):
  `Request ID, Option, Store, Items found, Missing, Subtotal (AED), Delivery (AED), Credit (AED), Total (AED), Pick, Filled, Why, Cart link, Checked on, Logged by`
  Option: the store, e.g. `Kibsons`, `Deliveroo HOP`, `Spinneys`. Missing:
  names, `; ` between them. Pick: `Yes` on the winner only. Filled: `Yes` / `No` /
  `Partly`. Why: the specific reason. Melody's app will show this tab (feature S7), so keep
  the columns and their names exactly as here.
- **Playbook** (new rows, or Status changes on existing rows):
  `Store, Area, Lesson, Status, Since, Last seen, Logged by`
  Store: a store name, or `All`. Area: `sign-in`, `search`, `match`, `cart`, `fees`,
  `offers`, `delivery`, `trust`, `other`. Status: `Rule`, `Question`, `Retired`.
  Since and Last seen are dates; bump Last seen when a Rule proves right again.
  For a Question, put the options and your recommendation in Lesson; when answered, change
  it to Rule and rewrite Lesson as the rule ("Skip egg whites at HOP: it doesn't sell them").
  The trust rule is exactly: Area `trust`, Lesson `Unattended OK`.
