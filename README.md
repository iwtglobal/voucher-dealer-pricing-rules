# Voucher Dealer Pricing Rules

An educational guide to **pricing rules in gift card and voucher distribution**: how operators set wholesale prices, discounts, and margins for each level of a reseller hierarchy, and how an **electronic voucher distribution system** applies those rules on every sale. Written for telecoms, gift card distributors, and VAS partners evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Are Voucher Dealer Pricing Rules?

In voucher distribution, the face value printed on an airtime PIN or gift card is rarely what each partner pays. A master distributor buys stock at one price, sells to sub-distributors at another, who sell to retailers at another, and the retailer sells to the customer at face value. **Dealer pricing rules** are the configuration that decides each of those prices.

There are two broad ways to pay the channel:

- **Discount (buy-price) model** — the dealer pays less than face value upfront, and their margin is the difference  
- **Commission model** — the dealer pays face value, and earns a commission later (see the related commission repo)  

Many programmes mix both: a discount at the top of the hierarchy and commissions further down. This repo focuses on the discount and price-list side.

---

## Architecture Overview

```
Product catalogue (operator / brand, denomination, face value)
        │
        ▼
Price lists ──► per tier (master, distributor, sub-dealer, retailer)
        │       per product, per denomination, per region or channel
        ▼
Overrides ──► dealer-specific deals, promotions with start/end dates
        │
        ▼
Sale request from dealer / POS / API
        │
        ▼
Pricing engine ──► resolves effective price, checks floor and ceiling
        │
        ▼
Debit dealer wallet at buy price ──► issue voucher ──► margin recorded per level
```

### Core components

| Component | Responsibility |
|-----------|----------------|
| **Product catalogue** | Every sellable item: brand, product, denomination, face value |
| **Price list service** | Default buy prices or discount percentages per tier |
| **Override layer** | Dealer-specific or time-limited prices, with approval |
| **Pricing engine** | Picks the effective price for each sale using a clear precedence |
| **Guardrails** | Floor and ceiling checks so no one sells below cost by mistake |
| **Margin ledger** | Records the margin earned at each hierarchy level for every sale |
| **Price change history** | Who changed what, when, and why |

See [docs/pricing-precedence.md](./docs/pricing-precedence.md) for a worked example of rule precedence.

---

## How Pricing Works, Step by Step

### 1. Start from the catalogue

Every price is attached to a product and denomination. Keep the catalogue clean first; messy product codes make pricing impossible to audit.

### 2. Set tier defaults

Give each hierarchy level a default discount or buy price per product group. Most dealers will never need anything else.

### 3. Add overrides sparingly

Large dealers negotiate special rates. Store these as overrides with start and end dates and an approver, rather than editing the defaults.

### 4. Resolve one effective price

For each sale, the engine applies a fixed precedence, for example: active dealer promotion, then dealer override, then regional price, then tier default. Whatever the order is, it must be written down and identical everywhere.

### 5. Enforce guardrails

Reject or flag any resolved price below the level's own cost or above face value. This catches typos before they cost money.

### 6. Record margin at every level

When a voucher is sold, record what each level in the chain earned. That is what lets finance reconcile the channel and lets partners trust their statements.

---

## Design Considerations

- **Percent vs fixed price** — percentages are simpler across many denominations; fixed prices suit products with irregular face values  
- **Rounding** — define rounding rules per currency, and apply them in one place  
- **Price change timing** — schedule changes for a specific moment and never change prices mid-transaction  
- **Wallet pre-funding** — in the discount model, dealers usually pre-fund a wallet; price changes affect how far that balance goes  
- **Multi-currency and cross-border** — catalogue face value and dealer buy price may be in different currencies  
- **Transparency** — let each dealer see their own current price list in the portal or app  

Platforms such as EVD System / MoboGage handle multi-tier reseller distribution, dealer wallets, and voucher inventory, which is where pricing rules are configured and enforced on every sale.

---

## FAQ

**What is a dealer discount in voucher distribution?**  
The difference between a voucher's face value and the price the dealer pays for it. It is the dealer's margin when they sell at face value.

**What is the difference between discount and commission models?**  
In a discount model the dealer pays less upfront. In a commission model the dealer pays full value and is paid a commission afterwards.

**How do distributors set different prices for different resellers?**  
With tier default price lists plus dealer-specific overrides, resolved by a fixed precedence order on each sale.

**What stops a reseller selling vouchers below cost?**  
Floor checks in the pricing engine, plus approval on any override that reduces a price.

**How does this relate to MoboGage / EVD System?**  
MoboGage's EVD platform supports multi-tier reseller hierarchies and voucher distribution. See [multi-tier reseller voucher distribution](https://evdsystem.com/evd-software/multi-tier-reseller-voucher-distribution/).

---

## Further Reading / Related Industry Resources

- [Multi-tier reseller voucher distribution](https://evdsystem.com/evd-software/multi-tier-reseller-voucher-distribution/) — how reseller hierarchies work in EVD  
- [Electronic voucher distribution software](https://evdsystem.com/evd-software/electronic-voucher-distribution-software/) — software overview for voucher distribution  
- Related repo: [reseller-commission-settlement](https://github.com/iwtglobal/reseller-commission-settlement) — the commission side of channel payouts  

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not commercial, legal, or tax advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
