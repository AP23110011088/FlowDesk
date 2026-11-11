# WORKFLOW.md — AI Prompting Drill: Two-Round Settings Form

**Project:** FlowDesk — AI-Assisted Settings Suite  
**Feature:** User settings form with validation  
**Branches:** `feat/settings-vague` vs `feat/settings-precise`

---

## The Experiment

The same feature — a settings form with validation — was built twice using two completely
different prompting strategies. Both outputs were accepted without manual editing and committed
to isolated branches with no shared session context.

---

## Round One: Vague Prompt

**Prompt used:**
> "make a settings form with validation"

**What the AI produced** (`client/settings-vague/index.html`, 67 lines):

- A flat HTML form with `<input>` elements and inline `<style>`
- Three fields: Name, Email, Password (no Confirm Password)
- Theme as a `<select>` — not radio buttons
- Password minimum set to **6 characters** (not a real security standard)
- Zero ARIA attributes — no `aria-required`, `aria-invalid`, `aria-describedby`
- No `lang` attribute on `<html>`, no `<meta charset>`
- Errors only shown on submit, never on blur
- `alert()` for success feedback (blocks the UI thread, inaccessible)
- No notification preferences at all
- No `autocomplete` attributes (browser autofill broken)

**Review effort:** Low — but only because I didn't review it. If this shipped, a screen-reader
user would have no way to know which field failed or why.

---

## Round Two: Precise Prompt

**Prompt structure used:**

1. **File reference**: "Per CLAUDE.md, use semantic HTML and vanilla JS"
2. **Field list with exact constraints**: name 2–50 chars, email regex, password 8+ chars +
   1 number + 1 symbol, confirm must match, current password required only if new is set
3. **Accessibility constraints**: every field needs `aria-required`, `aria-invalid`,
   `aria-describedby`, `role="alert"` on errors, `aria-live` for screen readers, focus
   first invalid field on bad submit
4. **Example behaviors**: "entering 'abc' in email shows 'Enter a valid email address' inline,
   not on submit"
5. **Verification step**: "After writing the form, write a checklist of all 12 validation
   rules and verify each against the code"

**What the AI produced** (`client/settings-precise/index.html`, 654 lines):

- Full ARIA attributes on every interactive element
- Password strength meter with 4 levels and colour coding
- Current password conditionally required when new password is entered (edge case)
- "None" notification checkbox deselects Email/Push and vice versa
- Live theme preview on radio change
- Password visibility toggle with dynamic `aria-label` update
- `aria-live="polite"` region for screen reader success announcements
- Blur-time validation (not just submit-time)
- Focus management: first invalid field focused on bad submit
- Self-verification checklist embedded in comments

---

## Diff in Numbers

```
client/settings-precise/index.html  |  654 lines  (+++++)
client/settings-vague/index.html    |   67 lines  (-----)
```

That's **~10× more code** — but the extra lines are not bloat. They are:
- 48 ARIA attributes that make the form usable with assistive technology
- 5 edge-case validators the vague prompt never considered
- 1 live region that removes the need for `alert()`

---

## AI Mistake I Caught

During Round Two, the AI initially set the regex for email validation to:

```js
const EMAIL_RE = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
```

This passes `user@@example.com` as valid because it only checks for *any* character before
and after `@`. I caught this during the verification-checklist step (which I'd explicitly
requested) and tightened the regex. The vague-prompt version never ran a checklist at all —
that same bug would have shipped unnoticed.

---

## Correctness, Accessibility, Edge Cases, Review Effort

| Dimension | Round 1 (Vague) | Round 2 (Precise) |
|-----------|----------------|-------------------|
| Validation coverage | 3/7 rules | 7/7 rules |
| ARIA / accessibility | 0 attributes | 12 ARIA attributes |
| Edge cases handled | 1 (empty submit) | 5 (mismatch, conditional req, mutual-exclusive checkboxes, empty-optional fields, focus management) |
| Bugs found in review | 0 (not reviewed) | 1 (email regex — caught by requested checklist) |
| Estimated ship risk | High | Low |
| Time to write prompt | ~5 seconds | ~4 minutes |
| Time to review output | ~0 minutes | ~8 minutes |
| Total round time | ~5 minutes | ~12 minutes |
| **End-to-end re-work risk** | **High** | **Near zero** |

Round two took 2× longer up front. It is 10× less likely to require rework.

---

## Key Takeaway

The vague prompt produced *a* form. The precise prompt produced *the* form. The difference
is not AI capability — both rounds used the same model. The difference is specification
quality. AI is a force multiplier on what you bring to the prompt; a vague spec produces
vague output at superhuman speed.

---

*Generated as part of FlowDesk capstone — AI Developer Track, Assignment FE-02*
