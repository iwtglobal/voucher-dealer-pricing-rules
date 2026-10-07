# Pricing Precedence: A Worked Example

This page shows how a **voucher pricing engine** can resolve one effective price from several rules. The numbers are **illustrative only** and are not recommended margins.

## Example setup

Product: a 10.00 face-value airtime voucher.

| Rule | Applies to | Buy price |
|------|-----------|-----------|
| Tier default | All sub-dealers | 9.70 |
| Regional price | Sub-dealers in Region North | 9.65 |
| Dealer override | Sub-dealer "D-204" | 9.60 |
| Promotion | All sub-dealers, one weekend | 9.55 |

## Precedence order (one example)

1. Active promotion for this dealer's tier  
2. Dealer-specific override  
3. Regional price  
4. Tier default  

The first rule that matches wins.

## Outcomes

| Dealer | Date | Effective price | Why |
|--------|------|-----------------|-----|
| D-204 (Region North) | Promo weekend | 9.55 | Promotion matches first |
| D-204 (Region North) | Normal day | 9.60 | Dealer override |
| D-310 (Region North) | Normal day | 9.65 | Regional price |
| D-415 (Region South) | Normal day | 9.70 | Tier default |

## Guardrail check

Before the sale completes, the engine checks that the effective price is:

- not below the price the dealer's parent paid (no selling below cost), and  
- not above face value.

If either check fails, the sale is blocked and the rule is flagged for review.

## Some programmes choose a different order

Some operators let a dealer override beat a promotion, so a large partner never gets a worse price during a campaign. Either is valid. The important part is that the order is documented and applied consistently in every channel.

## Related

See the main [README](../README.md). Product context: [multi-tier reseller voucher distribution](https://evdsystem.com/evd-software/multi-tier-reseller-voucher-distribution/).
