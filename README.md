# GA4 + GTM Practice Site

A tiny 3-page site for practicing event and conversion tracking with Google Tag Manager and GA4.

## Pages

| File | Purpose |
|---|---|
| `index.html` | Dashboard / landing page — CTA buttons, scroll content |
| `form.html`  | Lead-capture form — name, email, company, size, message |
| `thanks.html` | Confirmation page — fires the conversion event |

## How to run it

Just open `index.html` in your browser. For GTM Preview Mode to work cleanly, serve it locally:

```bash
# Python 3
cd <this folder>
python -m http.server 8000
# then open http://localhost:8000
```

## Step 1 — paste your GTM container

Every page has a clearly marked spot in `<head>` and right after `<body>` that says:

```
<!-- GTM HEAD SNIPPET GOES HERE -->
```

Paste the two snippets from your GTM container (Admin → Install Google Tag Manager) into those spots on **all three pages**.

## Step 2 — events the site already pushes to `dataLayer`

These are ready for you to build triggers around in GTM.

### Custom page view (all pages)
```
event: 'page_view_custom'
page_name: 'dashboard' | 'demo_form' | 'thank_you'
page_type: 'home' | 'lead_form' | 'conversion'
```

### CTA clicks (dashboard + thanks)
```
event: 'cta_click'
cta_name: 'hero_get_started' | 'hero_request_demo' | 'banner_book_demo' | ...
cta_location: 'hero' | 'bottom_banner' | 'nav' | 'thanks_page'
cta_text: '<the button text>'
```

### Scroll depth (all pages)
```
event: 'scroll_depth'
scroll_percentage: 25 | 50 | 75 | 100
page_name: 'dashboard' | 'demo_form' | 'thank_you'
```

### Engagement tick (dashboard, every 15s active)
```
event: 'engagement_tick'
engaged_seconds: 15, 30, 45, ...
```

### Form events (form page)
```
event: 'form_start'             // first focus into any field
event: 'form_field_complete'    // when a field is blurred with a value
   field_name, field_type
event: 'form_submit'            // validated submit
   form_name, form_destination, company_size, has_message
event: 'form_submit_error'      // validation failed
   error_fields: ['name', 'email']
```

### Conversion (thanks page) — GA4-recommended
```
event: 'generate_lead'
currency: 'USD'
value: 50.00
lead_source: 'demo_request_form'
transaction_id: 'demo_<timestamp>_<rand>'
company_size: '1-10' | '11-50' | ...
user_data: { email: '<lead email>' }
```

Plus a generic `event: 'conversion'` push so you can practice configuring conversions on a custom event name too.

## Step 3 — practice ideas

1. **Variables** — create dataLayer variables for `cta_name`, `form_name`, `value`, `transaction_id`.
2. **Triggers** — Custom Event triggers for `cta_click`, `form_submit`, `generate_lead`, `scroll_depth`.
3. **Tags** — GA4 Event tags that fire on each trigger and pass the variables as event parameters.
4. **Conversions** — in GA4 Admin → Events, mark `generate_lead` (and/or `conversion`) as a key event.
5. **DebugView** — use GTM Preview + GA4 DebugView to confirm every event lands with the right params.
6. **Filtering** — try a trigger that only fires `cta_click` when `cta_location == 'hero'`.
7. **Deduping** — the `transaction_id` lets you practice deduping conversions.

Happy tracking!
