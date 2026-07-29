## Week 7 — Issue Selection

**Issue link:** https://github.com/jamjamgobambam/pathreview/issues/101

**Issue title:** Add a "Copy link" button to share a public review summary

**Tier:** [ ] Tier 1  [x] Tier 2  [ ] Tier 3

### Problem summary

PathReview currently does not provide a way for users to share a completed review summary with someone who does not have access to their account. This issue will add a "Copy link" button to the review page that generates a public link to a read-only version of the review summary. Anyone with the link should be able to view the summary without logging in, but they should not be able to edit the review. The shared link must expire after 30 days, so the solution will require coordinated changes to the frontend review page, the frontend sharing service, and the backend review API.

### Issue fit and selection reasoning

I can explain the issue and its expected behavior in my own words. Before the fix, users can only view their review summaries while signed into PathReview and have no simple way to share the results. After the fix, the owner of a review should be able to generate and copy a temporary public link that displays the review in a read-only format without requiring authentication.

This issue is labeled Tier 2 because the solution requires understanding how multiple parts of the application connect. The feature involves `frontend/src/pages/ReviewPage.tsx`, `frontend/src/services/shareService.ts`, and `api/routes/reviews.py`. The review page will need a button and user feedback, the sharing service will need to communicate with the backend, and the API will need to generate and validate expiring public links.

I have located the relevant files named in the issue and confirmed that they exist in the codebase. Before implementing the feature, I will read the surrounding functions, existing API request patterns, authentication behavior, and related unit tests. I will also look for existing patterns for structured logging, API error handling, frontend service functions, and read-only pages so that my implementation follows the conventions already used by the project.

The issue has a clear expected result and an estimated effort of 5–8 hours. I believe this scope is realistic for Weeks 8 and 9 because I have experience with Python and frontend development, and the feature is limited to a small number of connected modules. The issue does not list any unresolved blockers or dependencies. I checked the issue comments and the cohort ledger before claiming it, and I am comfortable proceeding with the number of students working on the issue.

### Definition of done

The issue will be complete when:

* A signed-in user can generate a public sharing link from the review page.
* The link is copied to the user's clipboard.
* The public link opens a read-only review summary.
* The public page can be viewed without logging in.
* The shared link expires after 30 days.
* Expired or invalid links return an appropriate error.
* Relevant frontend and backend tests are added or updated.
* `make check` and `make test-unit` pass before the pull request is submitted.

**Branch name:** `feat/101-copy-review-link`

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

**Issue claim:** [x] Commented on Issue #101 to claim the issue

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/andynguyen01/pathreview/commit/dc5441e27a14c4551a215a07eb14c38889e6c310

**Reproduction summary:**

I logged into PathReview and opened a completed review. I clicked the existing Share button, which copied the normal authenticated review URL. When I opened the copied URL in an Incognito window, I could not view the review without logging in. This confirms that the current Share button does not generate the public, read-only link with a 30-day expiration required by Issue #101.

**PLAN.md link:** https://github.com/andynguyen01/pathreview/blob/feat/101-copy-review-link/PLAN.md

**Walkthrough video (recommended):** https://www.loom.com/share/fe7c7d3c93d944f68e59dac2c3900303

**Blockers or open questions:**

I still need to determine where the share token and expiration date should be stored, what public frontend route should display the shared review, and which existing backend and frontend test patterns should be followed.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**  
Implemented the main public review sharing flow for Issue #101. I added share token and expiration fields to the Review model and created migration `003`. I added backend service logic to generate secure share tokens that expire after 30 days and a public endpoint that allows shared reviews to be viewed without login.

On the frontend, I updated the Share button so it generates a public link instead of copying the authenticated review URL. I also added a public read-only shared review page.

**Next steps:**  
Add automated tests, run lint and unit checks, review the final diff, and prepare the pull request.

**Blockers:**  
I encountered a Pydantic validation error when the public endpoint tried to convert the SQLAlchemy Review model into `PublicReviewResponse`. I fixed it by adding `model_config = {"from_attributes": True}` to the public response schema.

---

### Check-in 2 (end of week)

**PR link:**  
[add PR link after opening the PR]

**Branch:**  
`feat/101-copy-review-link`

**What you built:**  
Implemented public review sharing for Issue #101. Users can generate a secure shareable URL for a completed review. The shared URL can be opened without authentication and displays a read-only version of the review. Share tokens expire after 30 days. Invalid or expired tokens cannot access the review.

I manually verified the feature by generating a link while logged in and opening it in an Incognito window without being logged in. I also changed the token in the URL and confirmed that an invalid token shows the shared review unavailable page.

**Tests added or updated:**  
Added automated tests covering:
- successful share-link creation
- 30-day expiration time
- rejection of incomplete reviews
- valid public token lookup
- expired token rejection
- invalid token rejection

`tests/unit/test_review_service.py` passes with:

`24 passed`

Ruff also passes for the updated test file:

`.venv/Scripts/python.exe -m ruff check tests/unit/test_review_service.py`

**Self-review confirmation:**
- [ ] `make check` passes
- [ ] `make test-unit` passes

`make check` currently reports existing repository-wide lint errors in unrelated files.

`make test-unit` completed with 393 passed and 40 failed. The failing tests are in unrelated areas such as bias detection, PII scrubbing, parsers, security, skill extraction, and technology detection. None of the failing tests are in `test_review_service.py` or the Issue #101 share-link functionality.

**Draft PR feedback received from:**  
[add person/name after feedback]