# megadev-w01c03

S01E03 — [what it shows](https://hq.mega.dev/challenges/what-it-shows).

## Deliverables

- [Parser problem: 379 reports](https://github.com/bennygesserit/megadev-w01c03/issues/2)
- [Booking-v3 hint problem: 33 reports](https://github.com/bennygesserit/megadev-w01c03/issues/1)
- [Signed KB page source](support-kb/src/routes/kb/incidents/4471-review/+page.svx)
- [Rendered KB page](support-kb/build/kb/incidents/4471-review.html)

Counts come from the exported components, not intake tags: 427 export rows become
412 distinct report IDs. Five unresolved symptom reports remain in the parser
model's count and are explicitly discussed, not asserted to have a proven cause.
The accepted historical decision and original incident page are preserved.

The subsequent desk update reports #4471 closed on 5 October: the parser was
fixed, while the hint was not fixed because booking-v3 was retired. Issue #2 is
closed as completed; issue #1 is closed as not planned. These dispositions
come from the course, not independent production verification.

## Run the knowledge base

Requires Node.js 20.19+ or 22.12+ and npm.

```sh
npm ci --prefix support-kb
npm run build --prefix support-kb
npm run preview --prefix support-kb -- --port 4173
```

Open http://127.0.0.1:4173/kb/incidents/4471-review.
The committed `support-kb/build/` directory is the static site, including its CSS
and interactive components. Serve it as a site root; do not open the HTML alone
as a local file. Rebuild after changing the source or exported data.

## Verification

Built the site and inspected the rendered review, original incident, stale
triage warning, and KB index in a browser. The review displays the computed
counts and signature-derived desk reference. Changing the explorer's app/form
channel changes its merge/separate verdict. No production scheduler fix is
claimed or included.