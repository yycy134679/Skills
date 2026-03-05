# Compliance Guardrails (Baseline)

## 1) Objective

Keep copy conversion-oriented but safe. Remove high-risk language (absolute, medical, guaranteed outcome), then keep transparent and testable expressions.

## 2) Risk Levels

- `high`: must replace or remove.
- `medium`: replace with qualified wording.
- `low`: keep with caution and context.

## 3) Banned or Restricted Expressions

| Risk | Pattern / Example | Replace With |
|---|---|---|
| high | "100% effective", "guaranteed results" | "designed to help", "may support", "many users report" |
| high | "cures", "treats disease", "medical-grade cure" | "supports comfort", "improves routine experience" |
| high | "works for everyone", "zero side effects" | "results vary by person", "patch test or first-use check recommended" |
| medium | "best on market", "number one" | "popular choice", "commonly preferred for X scenario" |
| medium | "instant transformation" | "visible change may appear quickly in this demo" |
| medium | "must buy now" | "worth considering if this scenario matches your needs" |
| low | "amazing", "great" | keep if supported by visible evidence in shot |

## 4) Auto-Replacement Priority

1. Replace `high` risk phrases first.
2. Replace `medium` risk phrases second.
3. Keep `low` only if adjacent evidence exists in the script.
4. If no safe replacement is available, delete the risky phrase.

## 5) Soft CTA Policy

Use suggestion-based conversion language:

- "If this fits your routine, it may be worth trying."
- "Start with one scenario and compare your result."
- "Save this and test when you need it."

Avoid pressure-only CTA:

- "Buy now or miss out"
- "Last chance for everyone"
- "You must get this today"

## 6) Compliance Notes Requirement

Always output a `Compliance Notes` section with:

- `Replaced Terms`: list of risky terms replaced or removed.
- `Risk Reminders`: concise reminders (for example, "results vary", "avoid medical claims").

If no replacements were needed, write:

- `Replaced Terms: none`
- `Risk Reminders: no high-risk claims detected`
