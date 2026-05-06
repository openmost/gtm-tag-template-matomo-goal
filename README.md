# Matomo Goal — Google Tag Manager template

A Google Tag Manager community template that records a **goal conversion** in Matomo through the standard `_paq` queue.

Authored by Ronan HELLO — [Openmost](https://openmost.com).

---

## What this tag does

When the tag fires, it pushes a `trackGoal` call to the Matomo tracker:

```js
_paq.push(['trackGoal', goalId, goalValue]);
```

This manually triggers a goal conversion for the current visitor — useful when the goal cannot be detected automatically by Matomo's URL / event matching rules (e.g. a single-page-app confirmation, a JavaScript-driven action, a dataLayer event…).

The conversion appears in Matomo under *Goals → Overview* and the per-goal report.

---

## Prerequisites

1. A working **Matomo** instance (self-hosted or Matomo Cloud).
2. A **Google Tag Manager** container loaded on your site.
3. The Matomo base tracker (`_paq`) must already be initialised on the page.
4. A **goal** must already be configured in Matomo (*Administration → Goals → Manage goals*) — note its **ID**.

This template only **adds the call to the `_paq` queue**; it does not load the Matomo tracker itself.

---

## Installation

### Recommended — install from the Community Template Gallery

This is the easiest path and gives you automatic update notifications when a new version is published.

1. In GTM, open your workspace and go to **Templates → Tag Templates → Search Gallery**.
2. Search for **"Matomo Goal"** by **Openmost**.
3. Click **Add to workspace** and accept the requested permissions.

That's it — the tag type **Matomo Goal** is now available when you create a new tag.

### Alternative — import `template.tpl` manually

Use this only if you can't access the Community Gallery (e.g. private GTM environment) or want to fork / customise the template.

1. In GTM, go to **Templates → Tag Templates → New**.
2. Open the menu (⋮) → **Import**.
3. Select `template.tpl` from this repository.
4. Save.

---

## Configuration

Once the template is added, create a new tag of type **Matomo Goal** and fill in the fields below.

| Field           | Required | Description                                                                                                                                                  |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Goal ID**     | Yes      | The numeric ID of the goal as configured in Matomo (e.g. `1`, `2`, `3`…).                                                                                    |
| **Goal value**  | No       | An optional revenue / value associated with this conversion. Overrides the default revenue defined for the goal in Matomo. Leave empty to use Matomo's default. |

### Tip — using GTM variables

Both fields accept GTM variables, so the goal value can be built dynamically — e.g. `{{DLV - ecommerce.value}}`, `{{Form Field - amount}}`.

---

## Triggering

Attach the tag to the trigger that represents the conversion event:

- *Page View* on a confirmation / thank-you URL
- *Form Submission* on a contact / signup form
- *Custom Event* pushed from the dataLayer (`purchase`, `subscription_started`, …)
- *Click* on a key CTA

> **Important:** make sure the Matomo base tag has fired **before or with** this tag (use *Tag Sequencing* or share the same trigger), otherwise `_paq` may not yet exist.

---

## Permissions

The template requests one permission:

- **Access global variables → `_paq`** (read / write / execute) — required to push the call into Matomo's tracker queue.

No external network requests are made directly by the template; the goal conversion request is sent by the Matomo tracker library (`matomo.js`).

---

## Goal vs. Event — which to use?

| You want to… | Use |
|---|---|
| Record a **business KPI / conversion** that's already defined in Matomo's admin (with revenue, funnel, etc.) | **Matomo Goal** |
| Record a **user interaction** for behavioural analysis (clicks, plays, scrolls) | **Matomo Event** |
| Both | Fire both tags — they don't conflict |

---

## Troubleshooting

- **Goal not appearing in Matomo** — confirm the goal **ID** matches the one configured in Matomo's admin and that the goal is *active*.
- **Conversion counted twice** — if the goal is also configured to be triggered automatically (e.g. on a specific URL), and you also fire this tag on that URL, Matomo may count two conversions. Use one method or the other.
- **`_paq is not defined`** — the Matomo base tracker hasn't loaded yet. Adjust your trigger or use Tag Sequencing.

---

## License

See [LICENSE](LICENSE).
