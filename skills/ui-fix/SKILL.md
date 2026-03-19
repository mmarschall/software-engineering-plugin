---
name: ui-fix
description: Use this skill when fixing UI bugs, layout issues, mobile rendering problems, or visual regressions. Especially relevant for mobile viewport issues, keyboard handling, CSS layout bugs, or any "it looks wrong on device X" report.
---

# UI Bug Fix Protocol

Before writing a single line of code, follow this process.

## Step 1: Find the working reference first

Search the codebase for a similar component that already works correctly on the affected platform/device. Read it before reading the broken one. The project has already solved this problem somewhere — find that solution and understand it before proposing anything new.

```
# Examples of what to look for:
# - Another mobile sheet/modal that works
# - Another component handling the same CSS property
# - Another page with the same layout structure
```

If a working reference exists, the fix is likely: make the broken component match the working one's pattern.

## Step 2: Understand WHY existing code is there before removing it

Every line of layout code in a component that has been touched multiple times exists for a reason. Before removing or restructuring anything, answer:

- What scenario was this solving?
- What breaks if I remove it?
- Is there a test or issue comment that explains it?

If you cannot answer these questions, do not touch that code.

## Step 3: Change one thing at a time

Each hypothesis gets its own isolated change. If you change 5 things and something breaks, you learn nothing. If you change 1 thing and it breaks, you know exactly why.

Prefer: read → reason → change one thing → verify.

## Step 4: Reason through CSS fundamentals before touching layout

For any CSS layout change, explicitly trace through the box model:

**Overflow + Transform trap**: `transform` on a child inside `overflow: hidden` is clipped at the pre-transform layout position. `translateY(50px)` inside `overflow: hidden` means the bottom 50px is invisible — the element appears to move but its clip box does not. Use `top`/`left` on the positioned parent instead.

**Safe area padding trap**: Applying `env(safe-area-inset-*)` padding to a full-screen container shrinks its content area. A child with `height: 100%` or `height: viewportHeight` then overflows the container and gets clipped. Apply safe area padding to individual elements (header, input bar) not to the outer container.

**`w-full` ≠ viewport width**: `width: 100%` is 100% of the parent's content area — which is narrower than the viewport if the parent has horizontal padding (e.g. from safe-area insets in landscape).

**`position: fixed` + iOS keyboard**: Fixed elements are positioned relative to the layout viewport, which does not resize when the iOS keyboard opens. The Visual Viewport API (`window.visualViewport`) is required. The correct pattern is:
```js
// Set top + height on the outer fixed container directly
el.style.top = `${visualViewport.offsetTop}px`
el.style.height = `${visualViewport.height}px`
// NOT: translateY on an inner child inside overflow:hidden
```

## Step 5: For mobile/device-specific bugs, research before guessing

Platform-specific rendering bugs (iOS Safari, Android Chrome, notched devices) have well-documented solutions. Before attempting a fix:

1. Search for the specific behavior ("iOS Safari position fixed keyboard", "visualViewport offsetTop", etc.)
2. Check MDN for the CSS property's actual behavior on mobile
3. Check if the project already has handling for this platform elsewhere

Do not iterate by trial and error on platform quirks. One research pass beats five failed commits.

## Step 6: Verify the invariants you're preserving

Before committing, state explicitly:
- What was the original bug?
- What specific change fixes it?
- What existing behavior could this break, and why won't it?

If you cannot answer the third question, you are not ready to commit.

## Common mobile patterns in this project

- Full-height chat views: `h-full` flex column (inherits `h-dvh` from App.vue) — no JS viewport tracking needed
- Fixed overlays with keyboard: `position: fixed; inset-x-0; top: viewportOffset + 'px'; height: viewportHeight + 'px'` — track both offsetTop and height from `window.visualViewport`
- Safe area on elements: `padding-top: max(0.75rem, env(safe-area-inset-top))` on header, `padding-bottom: max(0.75rem, env(safe-area-inset-bottom))` on input — NOT on the container
- iOS scroll lock: `body { position: fixed; top: -scrollY; width: 100% }` + restore `window.scrollTo(0, scrollY)` on close
- Prevent iOS auto-zoom on input focus: `font-size: 16px` minimum on all inputs
