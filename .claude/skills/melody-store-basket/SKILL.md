---
name: melody-store-basket
description: Work one UAE grocery store (Kibsons, Deliveroo HOP, Spinneys) in my signed-in Chrome for Melody - check prices and pack sizes for a list of items, or fill that store's cart, then stop before checkout. Used by melody-shopping-run, or alone, e.g. "what's Kibsons charging for blueberries" or "fill my HOP cart with eggs and oats".
---

# One store, one list

Used by **melody-shopping-run** for each store, or on its own when someone asks about one
store. Uses Claude in Chrome on the person's own signed-in Chrome. Read the store's page under
"Store pages" first, then its Playbook Rules in the Home – Groceries Sheet (Playbook tab, that
store and `All`); a Playbook Rule wins over the store page when they disagree, because it's
newer. When used alone, read the Playbook too (see melody-shopping-run's
"The Home – Groceries Sheet"), and still write what you learn (step 5).

## Hard rules
- The checkout page is look-only. You may open it to read what only shows there (delivery
  fee, service fee, credit, slots), but don't press, tick, type or choose anything on it:
  no Place order, Pay, slot booking, address, payment, tip, coupon or substitution
  choices. The only actions allowed there are going back: the browser's Back, the site's
  back or "edit cart" link, or the store's logo or product pages. Never go past checkout.
- Never type a password, one-time code or card number. Never save an address or card.
- Never remove a cart item Melody didn't add. Only remove items you added in this run.
- Never accept a subscription, membership or trial pop-up. Close it.

## Modes
- **check**: find each item and read prices. Don't add anything to the cart, unless the
  store only shows fees and credit in the cart or at checkout (its page says so); then
  add, read (at checkout too, look-only), go back, and take out exactly what you added.
- **fill**: add the matched items in the right quantities and read the cart's totals.

## 1. Open and check the sign-in
1. Open the store in a new Chrome tab (URL on its page or in the Playbook).
2. Signed in? Look for the account name or "My account" rather than "Sign in". If not
   signed in: it's a blocker (give it back to the run skill, or ask the person to sign in
   themselves, and wait for "done" if they're here). Some stores can show prices signed
   out: check mode may carry on if the store page says so, but fill mode needs a sign-in.
3. Delivery area: the home is in JVC, Dubai. If the site asks for a location, choose the
   saved home address. If there's none, it's a blocker: never type a new address unasked.
4. Look at the cart. Items already there that Melody didn't add: leave them, note them,
   and count them out of the totals. In fill mode, tell the person, because checkout
   will include them.

## 2. Find each item
For each item (name, size, brand, qty, notes, Favourites preferences):
1. Check the Playbook for a known match ("Organic eggs 12 = 'Organic Eggs Free Range
   12pcs'"). Search for that exact product first.
2. Otherwise search by plain name, then a shorter or singular form ("egg white" for "egg
   whites"). Use the site search box, not Google.
3. Pick the match:
   - Brand: if one is set, that brand only, unless Favourites lists an acceptable
     substitute. Brand blank means any; prefer what we bought before (Order history,
     Item insights), then the best price per unit.
   - Size: match the pack. If only other sizes exist, take the one closest to the same
     total amount (2 × 500 g for 1 kg) and note the unit price.
   - Avoid anything in Favourites → Avoid.
   - Fresh items (chicken, fish, berries): read the expiry or "best before" if shown.
     Same day or next day: mark `unsure` and say so.
4. Read: product name as the site shows it, pack size, price now (the offer price if
   there is one, and the offer's condition, e.g. "2 for AED 30"), and unit price.
5. Can't find it after 3 searches: `not sold`. Found but unavailable: `out of stock`.
   Two plausible matches and nothing to choose by: `unsure` (a blocker in supervised mode:
   ask with both options and a recommendation).

## 3. Fees, minimum, credit
Read them from the site, not from memory: delivery fee for this basket size, minimum
order, service fee, membership perks (HOP Plus free delivery over AED 30), and any credit
or cashback you'd get on *this* basket. If something only shows at checkout (Kibsons'
delivery fee), open checkout, read it, and go straight back (see Hard rules). If even
checkout doesn't show it, use the Store rules figure and say "from Store rules".

## 4. Fill (fill mode only)
1. Add each matched item with the right quantity. Read the cart back: every line, its
   quantity and price, the subtotal and fees.
2. If a line differs from what you checked (price changed, substitute offered), fix it if
   you can, or report it.
3. Delivery slot (Kibsons needs one): look at the next free slots (at checkout if that's
   the only place, look-only) and report the first two. Never book one.
4. Copy the cart's URL. Go back to the cart if you're on checkout. Stop. Leave the tab
   open on the cart.

## 5. Report and learn
Return, per item: item, qty, product matched, pack size, price, line total, unit price,
status (`found`, `swapped`, `not sold`, `out of stock`, `unsure`) and a note; plus
subtotal, delivery, credit, total, minimum met or not, cart link, and any blocker.
Then hand back lessons for the Playbook (or write them yourself when used alone):
search terms that worked, exact product matches, pop-ups and how you closed them, where
things are on the site, anything that changed since the store page was written.

## Adding a store
Add a page under "Store pages" from the template below (during a tune-up), fill what's known, add a row to the
Sheet's Store rules tab, and do the first run supervised.

## Store pages
What was known when this skill was last tuned up. The Playbook tab is newer.

### Deliveroo shops (HOP and Spinneys)
- One Deliveroo account, one sign-in for both. Always check **HOP first**, then Spinneys:
  HOP is where we order more often.
- Not known yet: does the Deliveroo website keep a separate basket per shop (the phone app
  may), or only one basket? Find out on the first supervised run, without clearing
  anything: add one item at HOP, open Spinneys, add one item, and see whether Deliveroo
  asks to clear the HOP basket. If it asks, cancel. Write the answer to the Playbook
  (Store `Deliveroo`, Area `cart`).
- **Separate baskets**: check both shops and leave each basket as it is.
- **One basket**: check HOP, note exactly what you added, clear only that, check Spinneys.
  Once the run picks the winner, stay on Spinneys if it won, or clear what you added at
  Spinneys and refill HOP if HOP won. Never clear a basket that holds items Melody
  didn't add: stop and ask (supervised) or skip Spinneys (unattended).
- In fill mode with one basket, only one Deliveroo shop can be filled, so a split of HOP
  + Spinneys can't be filled at once; a split of Kibsons + one Deliveroo shop can.

### Kibsons

- **Start at**: kibsons.com (confirm the exact address on the first supervised run and
  add it to the Playbook).
- **Sign-in**: the household's own account, signed in by the person. No saved address
  yet: the first fill will hit a "where to deliver" question. That's a blocker: the person
  adds the address themselves.
- **Delivery**: AED 8, minimum order AED 30. Needs a booked slot and someone home, so
  report the next two free slots.
- **Fees seen where**: the delivery fee only appears at checkout. Open checkout to read
  it and the free slots, then go back (look-only, see Hard rules).
- **Offers**: none known yet.
- **Known gaps**: none known. It sells egg whites.
- **Usually good for**: toilet paper (Kleenex 12 rolls), blueberries (the 1.5 kg pack was
  about AED 6.23 per 100 g on 2026-11-18), Al Ain chicken breast, organic eggs.
- **Reference**: 2026-11-18, all 5 staples (organic eggs, egg whites, blueberries,
  chicken breast, oats) came to AED 143.15.
- **Last checked**: 2026-11-18 (by hand, before this skill).

### Deliveroo HOP

Deliveroo's own grocery store, inside Deliveroo. Store name in the Sheet: `Deliveroo Hop`.

- **Start at**: deliveroo.ae, signed in, delivering to the JVC home, then the HOP store
  (confirm the exact page on the first supervised run and add it to the Playbook).
- **Sign-in**: the household's Deliveroo account, with a Plus membership.
- **Delivery**: 20–30 minutes to JVC. Plus gives free delivery over AED 30; check for a
  service or small-order fee in the cart.
- **Fees seen where**: the cart shows delivery, fees and credit before checkout.
- **Offers**: spend-and-get credit, which changes often, e.g. 10% back over AED 80, or
  AED 30 after 3 orders of AED 40+. Count only credit this basket earns or can use now.
  Note the offer's exact wording in the Playbook.
- **Known gaps**: no egg whites. Stock of oat balls varies.
- **Watch**: chicken that expires the same day. Mark it `unsure` and say so.
- **Reference**: 2026-11-18, 4 staples (no egg whites) came to AED 117.12, about
  AED 105.80 after credit.
- **Last checked**: 2026-11-18 (by hand, before this skill).

### Spinneys (on Deliveroo)

We order Spinneys through Deliveroo, not spinneys.com: it's a shop inside Deliveroo, like
HOP. Same Deliveroo account and sign-in. See "Deliveroo shops" below.

- **Start at**: deliveroo.ae, signed in, delivering to the JVC home, then the Spinneys
  shop (confirm the exact page on the first supervised run and add it to the Playbook).
- **Delivery**: its own fee and minimum on Deliveroo, which may differ from HOP. Read them
  in the cart on the first run and add them to the Store rules tab.
- **Offers**: none known yet.
- **Known gaps**: oat balls (stock varies).
- **Avoid for**: eggs, roughly double the price elsewhere. Still price them, so the
  comparison is honest, but don't expect it to win.
- **Last checked**: never by this skill.

## Template for a new store
#### <Store name>

- **Start at**: <URL, or "find on first supervised run and add to the Playbook">
- **Sign-in**: who signs in, whether prices show signed out
- **Delivery**: fee, minimum, slots, how fast
- **Fees seen where**: product page, cart, or checkout only
- **Offers**: memberships, credit, cashback and their conditions
- **Known gaps**: things it doesn't sell, or sells badly
- **Site quirks**: pop-ups, search tips, where the cart is
- **Last checked**: <date>
