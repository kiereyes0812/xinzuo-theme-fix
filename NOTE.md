## What I picked

Keyboard accessibility bug in the Bundle Builder product cards
(`sections/bundle-builder.liquid` + `assets/bundle-builder.js`).

## Why it's the highest-impact thing here

The Bundle Builder is the store's highest-AOV feature — customers
building knife sets of 3–8 items at tiered discounts. The product
cards had `role="button"` and `tabindex="0"` (correctly declared as
interactive) but zero keyboard support:

1. No `on:keydown` handler — pressing Enter or Space on a focused
   card did nothing. Keyboard-only users could not build a bundle.
2. No `aria-label` — screen readers announced every card as an
   unnamed "button". A blind user had no idea what product they
   were selecting.

Both are WCAG 2.1 AA violations: 2.1.1 (Keyboard) and 4.1.2
(Name, Role, Value). On a premium knife store targeting serious
home cooks, this silently excludes an entire group of customers
from the store's flagship feature.

A 1-line config change that fixes a real bug beats a 500-line
redesign — this is two files, four lines total.

## What I did

- Added `on:keydown="/handleCardKeydown"` to each bundle card div
- Added `aria-label="{{ product.title | escape }} — add to bundle"`
  to each bundle card div
- Added `handleCardKeydown(event)` method to `BundleBuilderComponent`
  that intercepts Enter/Space and delegates to `handleCardClick`

## What I'd do next

- Add the same keyboard fix to the series tab buttons (same pattern)
- Write a Playwright accessibility test to prevent regression
- Run axe-core against the full bundle builder page and fix
  remaining violations
- Consider adding a visible focus ring style to `.bundle-card:focus`
  for better keyboard UX
