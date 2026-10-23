# Accessibility and Mobile - Contrast, Keyboard, States, Reflow

Derived from antislop (MIT, `miqdadbadjuber/anti-slop`), merged and adapted. The rules cited here are defined in `rule-gate.md`. Accessibility is part of C-4, not a pass bolted on at the end.

## Contrast

### Thresholds

- Normal text: 4.5:1 minimum (R-25).
- Large text, 18px+ or 14px+ bold: 3:1 minimum.
- Component boundaries, icons, focus indicators, and chart segments: 3:1 against adjacent colors (WCAG 1.4.11).
- Test the worst point of the area the text crosses. White text over a photo or gradient must be checked across the whole text block, not at one bright spot.

### Formula (WCAG 2.x)

1. Contrast ratio = `(L1 + 0.05) / (L2 + 0.05)`, where L1 is the lighter luminance.
2. Per channel: `c = hex / 255`. If `c <= 0.03928`, `c_lin = c / 12.92`; otherwise `c_lin = ((c + 0.055) / 1.055)^2.4`.
3. `L = 0.2126*R + 0.7152*G + 0.0722*B`.
4. Compare the unrounded ratio. Round only for display. Maximum is 21:1.

### Checker script

`references/contrast-check.py` ships with this skill. Run it instead of eyeballing:

```bash
python3 references/contrast-check.py "#FFFFFF" "#777777"
# normal text: FAIL (4.48 < 4.5)
# large text:  PASS (4.48 >= 3.0)
```

On Windows use `python`. If Python is unavailable, the formula and table are complete on their own. Never claim a pairing passes without one of the three: script, formula, or table.

### Reference pairings

| Pairing (text on background) | Ratio | Normal 4.5 | Large 3.0 |
|---|---|---|---|
| Black on white | 21.00 | Pass | Pass |
| White on black | 21.00 | Pass | Pass |
| White on #333333 | 12.63 | Pass | Pass |
| White on #666666 | 5.74 | Pass | Pass |
| #777777 on white | 4.48 | Fail | Pass |
| White on #888888 | 3.54 | Fail | Pass |
| White on #999999 | 2.85 | Fail | Fail |
| #555555 on black | 2.82 | Fail | Fail |

The common hallucination is "dark grey on black" or "light grey on white" asserted as passing. #555555 on black is 2.82:1 and fails. Compute it.

## Keyboard and focus

- Tab order follows visual order. Do not give real content `tabindex="-1"` outside a controlled dialog focus trap.
- Buttons and links activate with Enter or Space. Dialogs close with Escape.
- Every focused element has a visible indicator at 3:1 against its neighbors, in every shipped theme.
- Never `outline: none` or `outline: 0` without a replacement `:focus-visible` style.
- No hover-only menus, click-open dropdowns that cannot open by keyboard, or drag-and-drop without a keyboard path.
- Add a skip link on long pages.

## Feedback and states

- Never signal status by color alone. Pair every color with text, an icon, or a pattern. Error text says what failed and how to fix it ("Password must be at least 8 characters"), not just a red border.
- Empty, loading, and error states must each be perceivable and informative:
  - Empty: why it is empty and the one action that fills it.
  - Loading: what is loading, not a bare spinner.
  - Error: what failed and what to do next.
  - First run, filtered-to-nothing, and permission denied are different screens.
- Announce async state changes with a live region where the UI updates without focus moving.

## Resize and zoom

- Text must resize to 200% without clipping or forced horizontal scroll.
- Avoid fixed pixel type for body copy; use a fluid scale (`clamp()`) or a smaller step at the breakpoint.
- Do not put text in `overflow: hidden` containers that clip it when the viewport narrows.
- On mobile, a focused input scrolls above the on-screen keyboard, with enough bottom padding that the keyboard never covers it.

## Mobile layout

Mobile is a different layout, not the desktop layout squeezed smaller. It reflows: re-stack, rescale, re-order with intent.

### Breakpoints

- Set breakpoints where the content breaks, not at a device list (375, 414, 768). Narrow the viewport and watch where the layout stops holding.
- Mobile rules are not a trailing override. Give the narrow viewport its own deliberate sizes and stacking.
- Verify the whole layout at the narrowest target, not only the patched spots (R-35).

### Scale

- Use a smaller type scale, section padding, and gaps on mobile.
- Do not carry desktop hero heights and 96-128px section padding onto a phone.
- Avoid `100vh` for sections. Size to content, or use `dvh` where a true full-height section is intended.
- Keep tap targets at 44x44 px minimum even when the visual is smaller. Leave clear space between adjacent targets.

### Grids and overflow

- Multi-column grids collapse to one column at the breakpoint.
- Use `minmax()`, `auto-fit`, and `auto-fill`; fixed-pixel tracks force overflow.
- Do not force a 12-column system onto content that needs one or two columns.
- Find horizontal overflow at the narrowest width: wide tables (reflow or a scroll container), code blocks (wrap), images (`max-width: 100%`), long unbroken strings.
- Do not use `overflow: hidden` to hide content that does not fit. Let it reflow. Clip only when cropping is the intent.
- Flex and grid children need `min-width: 0` or fluid widths so they can shrink.

### Interaction

- Hover-only interactions need a tap equivalent and visible `:active` feedback.
- Navigation reflows: bottom nav for the few primary destinations, a labeled menu for the rest. A bare hamburger with no label hides the only route.
- Fixed bottom bars must reserve their height in page padding and respect safe-area insets. Sticky headers stay compact.

## Checklist

- [ ] Every text and background pairing is verified by script, formula, or table, including text over media.
- [ ] Every component boundary and status indicator meets 3:1.
- [ ] Focus is visible on every interactive element, in every theme.
- [ ] Full keyboard operation, Escape closes dialogs, no `outline: none` without replacement.
- [ ] Empty, loading, and error states exist and are perceivable, not color-only.
- [ ] Text resizes to 200% without clipping; the mobile keyboard never covers a focused input.
- [ ] The layout reflows into a distinct mobile state with a mobile scale.
- [ ] No horizontal overflow, no clipped content, no colliding grid.
- [ ] Targets are at least 44x44 px with spacing.
- [ ] Navigation reflows and fixed bars never cover content.
