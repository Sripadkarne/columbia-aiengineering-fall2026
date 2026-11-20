# Per Plate

Team name: Back of House

## 1. Problem
An independent restaurant owner does not know which dishes have slipped below their target food margin. Ingredient prices change every week, and a dish's own margin misses what it brings with it: the steak may barely cover its ingredients but sell the wine. Working this out means joining invoices, recipes and sales by hand, so most owners guess.

## 2. User
One profile: an independent, single-location restaurant with about $1M in yearly revenue. The head chef is the user and the owner is the buyer.

| Step | Who | When |
| --- | --- | --- |
| Keep recipes up to date | Chef | When the menu changes |
| Photograph supplier invoices | Chef | At each delivery |
| Confirm new matches and large price changes | Chef | Weekly |
| Upload the sales export | Owner | Weekly |
| Reprice, re-portion or drop a dish | Owner | Weekly |

At 30% food cost this restaurant spends about $300k a year on food, so cutting one point is worth about $10k a year. We would charge $100 a month, under a third of a back-office suite. We will test these numbers and the weekly steps with three owners before Week 5.

## 3. MVP
The chef enters recipes once and photographs invoices; the owner uploads weekly sales. Per Plate computes each dish's food cost and food margin, and the food margin of the whole order it appears in. It raises two flags: negative food margin, and below target food margin.
Not in the MVP: labor, overhead, inventory usage, ordering or bill pay. Every number is food cost or food margin, never profit.

## 4. Data
Real invoices and sales records are private, so we cannot use them. We will generate 200 synthetic invoices and 10 weeks of orders for three restaurants of this profile, from a public recipe dataset and USDA price reports, with price jumps and pairings planted.

## 5. Models and APIs
- Generation: a small, cheap model reads invoices into line items, matches them to recipe ingredients, and writes a one-line note for each flagged dish.
- Rules: plain code converts units and computes every food cost and food margin.
- Cost: about $0.50 per restaurant per week.

## 6. Evaluation
A correct week gives every dish a food cost close to a hand-computed answer key and raises the same flags. Line accuracy is not enough: eight ingredients each right 90% of the time are all right only 43% of the time. So we measure dish cost in dollars and the flags, before and after the chef's corrections, on 10 weekly scenarios.

| Measure | Before corrections | After corrections |
| --- | --- | --- |
| Invoice lines fully correct | 90% | n/a |
| Dish food cost within $0.25 | 50% | 90% |
| Dish food cost off by over $1.00 | Under 20% | Under 5% |
| Flag precision | 60% | 90% |
| Flag recall | 60% | 90% |

Business KPI: share of planted food-margin loss, in dollars, that the flags surface. Target 80%.

## 7. Risks

| Risk | Fallback |
| --- | --- |
| The model misreads a price or pack size | The chef confirms any price change over 30% |
| One wrong ingredient price distorts many dishes | Confirm the ingredients that affect the most dishes and dollars first |
| A pairing is a coincidence, not a cause | Require a minimum number of orders and say "ordered together" |
| The chef skips the weekly confirmations | Mark dishes with unconfirmed prices as provisional |
