## Week 7 — Issue selection

**Issue link:** https://github.com/jamjamgobambam/pathreview/issues/101

**Issue title:** Add a "Copy link" button to share a public review summary

**Tier:** [ ] Tier 1  [x] Tier 2  [ ] Tier 3

**Problem summary:**

PathReview currently does not provide a way for users to share a completed review summary with someone outside their account. This issue will add a button on the review page that generates and copies a public link to a read-only version of the review summary. The shared page must be accessible without requiring the viewer to log in, while preventing them from editing the review. The link must also expire after 30 days, which will require changes to the frontend review page, the sharing service, and the review API routes.

**Selection notes — "Is this issue right for me?" checklist reasoning:**

I can explain the expected behavior and identify the main parts of the application involved. The issue affects the frontend review page, the frontend sharing service, and the backend review routes, so it matches the description of a Tier 2 issue that requires understanding how multiple modules connect. I have located the relevant files listed in the issue and will read the existing review and API test patterns before implementing the feature. The issue has a clear outcome, an estimated effort of 5–8 hours, and no stated blockers, so I believe it is realistic to complete before the Week 9 deadline.

**Branch name:** `feat/101-copy-review-link`

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
