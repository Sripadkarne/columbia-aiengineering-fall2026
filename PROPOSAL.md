# Per Plate

Team name: Back of House

## 1. Problem
An independent restaurant owner does not know which dishes make money. Ingredient prices change every week, and a dish's own margin misses what it brings with it: the steak may barely break even but sell the wine. Working this out means joining invoices, recipes and sales by hand, so most owners guess.

## 2. User
The user is the chef or kitchen manager. The buyer is the owner, who pays because food is one of the restaurant's largest costs.
Back-office suites can cost dishes, but they run several hundred dollars a month per location and do far more than a single restaurant needs. We will test with three owners before Week 5 whether they would upload invoices and sales every week.

## 3. MVP
The owner enters recipes once, then uploads the week's supplier invoices and the sales export from the till. Per Plate computes each dish's cost, margin and weekly profit, and the profit of the whole order when that dish is in it. It flags dishes that lose money, and dishes that look weak alone but come with profitable orders.
Not in the MVP: labor and rent, inventory counts, ordering, bill pay or price suggestions.

## 4. Data
Real invoices and sales records are private, so we cannot use them. We will generate 200 synthetic invoices and 10 weeks of orders for three restaurants, built from a public recipe dataset and public USDA price reports, with known pairings planted in the orders.

## 5. Models and APIs
- Generation: a small, cheap model reads invoices into line items, matches them to recipe ingredients, and writes a one-line note for each flagged dish.
- Rules: plain code converts units and computes every cost, margin and order profit. 
- Cost: about $0.50 per restaurant per week.

## 6. Evaluation
A correct week reads every price correctly and flags the same dishes and pairings as an answer key computed by hand. We will test 10 weekly scenarios across the three restaurants, with planted price jumps, renamed items and planted pairings such as steak with wine.

| Measure | Target |
| --- | --- |
| Invoice lines read correctly | 90% |
| Loss-making dishes caught | 80% |
| Planted pairings found | 70% |
| Flagged dishes that are actually fine | Under 10% |

## 7. Risks

| Risk | Fallback |
| --- | --- |
| The model misreads a price or pack size | Ask the owner to confirm any price change over 30% |
| A line item is matched to the wrong ingredient | The owner confirms each new match once, and it is stored |
| A pairing is a coincidence, not a cause | Require a minimum number of orders and say "ordered together", never "causes" |
| Synthetic data is too clean to be realistic | Write the messiest invoices by hand and ask a restaurant for real samples |
