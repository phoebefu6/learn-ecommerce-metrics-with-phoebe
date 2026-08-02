# learn-ecommerce-metrics-with-phoebe - source coverage map

Built 2026-08-02, directly from verified official sources (course-taking loop paused).
Course #1 of the 4-course ecom cluster (metrics -> user-journey -> traffic -> campaign),
all sharing the Mango Lane running store.

## The running case: Mango Lane (canonical numbers - NEVER drift from these)

One typical month, mobile-first fashion + lifestyle store, ~3,000 SKUs, 4 channels.

| Quantity | Value | Derivation |
|---|---|---|
| Unique visitors (UV) | 240,000 | given |
| Sessions | 360,000 | 1.5 sessions/visitor |
| PDP penetration | 40% | sessions with >=1 view_item / sessions = 144,000 |
| Add-to-cart rate (of PDP sessions) | 25% | 36,000 cart sessions |
| Cart -> checkout start | 45% | 16,200 checkout sessions |
| Checkout completion | 60% | 9,720 purchase sessions |
| Orders | 9,720 | product of the chain |
| Session CR | 2.7% | 9,720 / 360,000 (= 0.40 x 0.25 x 0.45 x 0.60) |
| Visitor CR | ~3.9% | ~9,300 buying visitors / 240,000 |
| AOV | $68 | 2 items x $34 |
| GMV | $660,960 | 9,720 x $68 |
| Channel split of GMV | Organic $264,384 (40%) / Paid $198,288 (30%) / CRM $132,192 (20%) / Social $66,096 (10%) | sums exactly |

All three simulator trees (simple GMV, funnel-deep, channel-additive) compute to
$660,960 exactly - verified in python before page authoring.

## Verified source facts (research pass 2026-08-02)

### GA4 (support.google.com/analytics - fetched/verified)
- Purchase journey report (answer/13128171): 5 steps counting USERS: session_start ->
  view_item -> add_to_cart -> begin_checkout -> purchase. Closed funnel by default.
- Checkout journey report (answer/14000977): begin_checkout -> add_shipping_info ->
  add_payment_info -> purchase; abandonment % below each step.
- Sessions (answer/9191807): session_start auto-event; ends after 30 min inactivity.
- Users (answer/12253918): Total users = triggered any event; Active users = engaged
  (the default "Users"); New users = first_open/first_visit.
- Item-scoped: cart-to-view rate, purchase-to-view rate (paraphrase - Google glossary
  page truncated on fetch; confirmed via secondary refs).

### Shopify (help.shopify.com - fetched/verified)
- Online store conversion rate = purchase sessions / total sessions (SESSION-based).
- Funnel stages (each / total sessions): sessions with cart additions -> reached
  checkout (user input during checkout) -> completed checkout.
- AOV = revenue / orders, excluding gift cards + post-order adjustments (blog-corroborated,
  help page not verbatim).
- Returning customer rate = returning / (returning + first-time) (community-corroborated).

### Benchmarks (cite source + date on pages)
- Cart abandonment 70.22% - Baymard Institute, avg of 50 studies, updated Sep 2025 (STRONGEST cite).
- IRP Commerce June 2026: overall CR 2.03%; Fashion 1.70%; range Arts&Crafts 5.53% to Baby&Child 0.51%.
- Dynamic Yield (June, rolling): CR high Beauty 5.37% annual / Pet 5.7% monthly, low Luxury 0.71%;
  add-to-cart 1.72% (luxury) to ~10% (food/beauty).
- Littledata Shopify panel 2025: avg CR 1.4% (mobile 1.2 / desktop 1.9), top 10% >4.7%;
  ATC avg 4.6%; checkout completion avg 45%, top 10% 66%. (secondary-relayed - label as such)
- Context note for pages: Mango Lane (2.7% session CR, 60% checkout completion) is a
  HEALTHY fashion store - above IRP fashion 1.70% and Littledata checkout avg 45%. Say so
  honestly where benchmarks appear (a5, b4).

### GMV vs revenue (no single standard-setter - teach as "ask what's netted out")
- GMV = price x qty of orders placed, before cancellations/returns (marketplace-style).
- Shopify ladder: Gross sales -> (- discounts - returns) -> Net sales -> (+ tax + shipping) -> Total sales.

## Per-session coverage

### Leader track (thinking mode, no computation)
| Session | Covers | Sources |
|---|---|---|
| a1 The GMV equation | GMV = traffic x CR x AOV, outcome vs lever, owners per lever | DuPont heritage ✓, sibling: learn-metric-decomposition |
| a2 The metric dictionary | UV/sessions/pageviews, CR flavors, AOV, PDP penetration, ATC rate - which metric answers which question | GA4 users/sessions ✓, Shopify CR ✓ |
| a3 Reading the funnel | 5-step trail, where money leaks, abandonment | GA4 purchase journey ✓, Baymard 70.22% ✓ |
| a4 The weekly business review | ecom WBR ritual: scorecard -> funnel -> channels; question bank | Amazon WBR pattern ◐ |
| a5 Benchmarks + seasonality | IRP/DY/Littledata numbers, fashion seasonality, BAU vs campaign baseline | IRP June 2026 ✓, DY ✓, Littledata ◐ |
| a6 Metric governance | one definition per metric, north star + guardrails, denominator honesty | Shopify vs GA4 denominator split ✓ |

### Analyst track (compute + simulate)
| Session | Covers | Sources |
|---|---|---|
| b1 Meet Mango Lane | store, 5-event trail, visitor/session/pageview counting rules, 30-min timeout, CR denominators | GA4 sessions ✓, GA4 events ✓, Shopify funnel ✓ |
| b2 Core four by hand | UV, session CR, AOV, GMV computed from raw counts; GMV vs revenue ladder | Shopify AOV ✓, GMV ladder ✓ |
| b3 Build the GMV tree (SIMULATOR) | tree-live.js: simple tree, funnel-deep tree, channel-additive tree; pass-through rule | tree math verified ✓ |
| b4 Funnel metrics | PDP penetration, ATC rate, cart->checkout, completion; GA4 checkout journey; Baymard | GA4 checkout journey ✓, Baymard ✓, Littledata ◐ |
| b5 Diagnose the drop (SIMULATOR) | simulate-drop workflow on the funnel-deep tree; % pass-through diagnosis; write the finding | tree-live ✓ |
| b6 Segment cuts | channel/device/new-vs-returning cuts; mix effects (Simpson's paradox intro); returning customer rate | Shopify returning rate ◐ |
| b7 The review pack | weekly/monthly pack: scorecard, funnel, channel table, narrative template | Amazon WBR pattern ◐ |
| b8 Metric spec sheet | definition/owner/denominator/threshold per metric; alerting basics; handoff | GA4 vs Shopify definitions ✓ |

✓ = verified against fetched source · ◐ = paraphrase/secondary (flagged in research)

## Not covered by design
- SQL computation (learn-sql-with-phoebe + learn-metric-decomposition own that)
- Attribution modelling (learn-marketing-attribution-with-phoebe)
- A/B testing of CR changes (learn-experimentation-with-phoebe)
- Forecasting/anomaly detection on these metrics (ds-bucket siblings)
- Deep driver-tree patterns beyond GMV (learn-metric-decomposition-with-phoebe)
- Campaign/mega-campaign measurement (ecom course #4, coming)

## Re-verify before delivery
- IRP CR table updates monthly; Dynamic Yield rolling - refresh numbers if delivering after 2026-Q4.
- GA4 report names/urls occasionally reshuffle.
