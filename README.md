# Minimal reproducible example for accordion menu accessibility issues

This repository covers a minimal reproducible example regarding
Lighthouse accessibility issues/warnings.

- It is either required to have `yarn`, `gulp` and `node.js` system-
  wide installed.

- Or it should be possible to launch this against the Nix flake
  shipped with this repository.

The example can then be started with `yarn start`.

**Issues**

- Running Lighthouse against the dev server (e.g. localhost:3001) in
  Chrome-based browsers, e.g. chromium, leads to following issues:

  - `[aria-*] attributes do not match their roles`

    - This happens due to the fact `aria-multiselectable`
      is passed which is disallowed for `role="menubar"`

  - `Elements with an ARIA [role] that require children to contain a specific [role] are missing some or all of those required children.` and `[role]s are not contained by their required parent element`

    - IMHO this addresses the same issue: `<li>` has role="none"
      and the wrapped `<a>` role="menuitem". I assume it is the
      role="none" which "breaks" everything and tells a
      screenreader the menu has no child items, even if there
      are further descendants with the proper role.


**Expected behavior**

I would expect this minimal reproducible example to not face ARIA issues in Lighthouse.

**Possible solution**

- Omit `aria-multiselectable` entirely. I understand the idea of setting
  this attribute dependent on whether multiple items may be unfolded, but
  as far as I understand the standards, this does not comply with the
  intention of `aria-multiselectable`.

- Fix `role="none"`. I honestly don't know what's a good solution here.
  Setting `role="menuitem"` as well? Setting no role and assuming screenreaders
  will follow `role="menu"` descendants transitively until they find `menuitem`?
