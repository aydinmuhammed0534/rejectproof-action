# RejectProof GitHub Action

Fail the build **before** App Review does. Scans an `.ipa` for 43 documented App Store rejection causes — missing privacy manifest, missing purpose strings, no Restore Purchases, ATT prompt, Sign in with Apple, expired provisioning, dead privacy-policy links, template fingerprints — and writes the result to the job summary.

```yaml
- uses: actions/checkout@v4
- name: Build IPA
  run: fastlane build      # or xcodebuild / eas build --local
- name: App Store rejection check
  uses: aydinmuhammed0534/rejectproof-action@v1
  with:
    ipa: build/MyApp.ipa
    fail-on: rejection      # rejection | warning | never
```

## What you get

- A job summary listing every likely rejection and warning with its guideline number.
- Outputs `rejections` and `warnings` for your own gates.
- Exit code 1 on likely rejections (configurable with `fail-on`).

The scan runs on the runner via [`npx rejectproof`](https://www.npmjs.com/package/rejectproof). The binary is not uploaded anywhere; with `network: "false"` the action makes no outbound requests at all.

## Fixes

The action names what is wrong. The fix for each item — and the free re-scans for three days while you fix it — is the paid report: `npx rejectproof MyApp.ipa --report` locally, or drop the build on [rejectproof.com](https://rejectproof.com). $12 per app.

## Inputs

| name | default | |
|---|---|---|
| `ipa` | — | path to the `.ipa` or zipped `.app` |
| `fail-on` | `rejection` | `rejection`, `warning` or `never` |
| `network` | `true` | `false` skips the dead-link check |

## Related

- [`rejectproof` on npm](https://www.npmjs.com/package/rejectproof) — the CLI this action runs
- [App Store rejection index](https://rejectproof.com/rejection) — every check with the guideline it maps to

Not affiliated with Apple. App Store and App Review are trademarks of Apple Inc.
