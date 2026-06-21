# Architecture — DAP Logistics WordPress Theme

Engineering overview for the custom theme built by **Muhammad Hamid**.

## Design principles

1. **Layout in code**  Every section is a PHP partial. Admin never touches HTML structure.
2. **Content via ACF**  Text, images, and video fields with hardcoded fallbacks in templates.
3. **One plugin**  Advanced Custom Fields handles all admin editing. No page builders.
4. **Page-scoped assets**  Each template loads its own CSS/JS bundle only.
5. **CRM-native forms**  HubSpot Forms API through a server-side WordPress proxy.

## Request flow  lead forms

```
User submits custom HTML form
        ↓
dap-hubspot.js (AJAX + nonce)
        ↓
WordPress admin-ajax.php
        ↓
inc/hubspot.php → dap_hubspot_api_submit()
        ↓
HubSpot Forms API v3 (IP + hutk cookie attached)
        ↓
Contact / submission recorded in HubSpot CRM
```

## Content editing flow

```
template-parts/home/hero.php
        ↓
dap_text( 'home_hero_line1', 'Expand Your Business' )
        ↓
get_field() → ACF value OR default string
        ↓
Frontend renders updated copy — layout unchanged
```

## Module map

| Module | File | Responsibility |
|--------|------|----------------|
| Bootstrap | `functions.php` | Assets, page map, theme setup |
| Helpers | `inc/helpers.php` | Field getters, WhatsApp URLs, partials |
| HubSpot | `inc/hubspot.php` | Form GUIDs, AJAX proxy, tracking |
| SEO | `inc/seo.php` | Meta, OG, Schema.org |
| Legal | `inc/legal.php` | Dynamic contact in legal pages |
| ACF | `inc/acf-fields/` | Global settings + per-page fields |
| Activation | `inc/activation.php` | Auto page creation on theme switch |
