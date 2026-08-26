# Meero

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Meero is the Paris-founded AI visual-content company that rebranded to **Diffusely** in December 2024 after pivoting away from on-demand photography marketplaces toward vertical AI imaging software. Its surviving operating brand is **CarCutter** (DIFFUSELY Austria GmbH), a B2B vehicle merchandising platform for dealership groups, OEMs and used-vehicle marketplaces.

The public developer surface profiled here belongs to CarCutter:

- **Car-Cutter API** — OpenAPI 3.1, 14 operations, bearer token, `https://api.car-cutter.com` — [docs](https://cloud.car-cutter.com/doc/api.html)
- **Public keyless demo** — `POST /vehicle/composition/segment` plus a hosted UI at https://demo.car-cutter.com/
- **Remote MCP server** — `https://www.carcutter.com/wp-json/mcp/mcp-oauth-server` (OAuth-gated, RFC 8414 + RFC 9728 discovery)
- **CarCutter WebPlayer** — five first-party npm packages (`@car-cutter/*`, Apache-2.0) and a CDN build
- **Status page** — https://carcutter.statuspage.io/
- **llms.txt** — https://www.carcutter.com/llms.txt

Ownership evidence: `https://api.car-cutter.com/` returns `{"copyright":"(c) 2026 Meero Austria GmbH","description":"A service by CarCutter"}`, and Diffusely's own domain `diffuse.ly` redirects to `www.carcutter.com`.

Two former Diffusely verticals have left the group and are **not** attributed here: **autoRetouch** merged into Grand Shooting (February 2026) and **ProperShot** joined Nodalview. The legacy `meero.com` domain no longer serves the company — it answers with an Apache2 Ubuntu default page under a certificate for `CN=forlog.fr`, and `api.meero.com` no longer resolves.

- https://www.carcutter.com/
- https://forgeglobal.com/meero_stock/ (secondary-market listing that seeded this profile)
