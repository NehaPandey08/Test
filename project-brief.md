# Project Brief: PR Velocity Ledger

AI adoption impact dashboard for GitHub pull requests. Compares delivery metrics before and after the AIDLC adoption date across one or more repositories, rendered as a public-safe web dashboard for leadership.

---

## 1. Objective

Quantify the impact of AI-assisted development (AIDLC) on delivery velocity using merged PR data. Every metric is computed twice, once for PRs merged before the adoption cutoff date and once for PRs merged on or after it, and presented side by side with percentage deltas.

## 2. Metrics

| Metric | Definition |
|---|---|
| Cycle time | Merge timestamp minus PR creation timestamp |
| Time to first review | Earliest submitted review timestamp minus creation timestamp |
| Review to merge time | Merge timestamp minus earliest submitted review timestamp |
| Commits per PR | From PR detail endpoint |
| Files changed per PR | From PR detail endpoint |
| Lines changed per PR | Additions plus deletions |
| Copilot comments per PR | Review comments where author login matches /copilot/i |
| Human review comments per PR | Total review comments minus Copilot comments |
| PR volume | Count of merged PRs on each side of the cutoff |

Methodology notes:
- Only merged PRs count. Closed-unmerged PRs are excluded.
- Averages exclude PRs where a field cannot be determined, for example a PR merged with no review submitted contributes nothing to review timing averages.
- Copilot detection is username based. It covers github-copilot[bot] and Copilot code review bots. If your org uses a differently named bot, extend the regex in one place (both collector and dashboard share the same expression).

## 3. Components

Five files, all delivered:

| File | Role |
|---|---|
| pr-velocity-dashboard.html | The dashboard. Static, self-contained, Chart.js via CDN |
| config.json | Single configuration file, shared by dashboard, collector, and proxy |
| collect-metrics.js | Server-side collector. Node 18+, zero npm dependencies. Writes metrics.json |
| proxy-server.js | Optional read-only GitHub proxy. Node 18+, zero npm dependencies |
| update-metrics.yml | GitHub Actions workflow. Scheduled collector runs, commits the snapshot |

## 4. Data source modes

Set by "dataSource" in config.json.

**snapshot (recommended for the public deployment)**
Dashboard renders a precomputed metrics.json. Viewers' browsers make zero GitHub calls. No credential exists client-side in any form. Freshness equals collector schedule.

**proxy (live data, public-safe)**
Dashboard calls your proxy, proxy attaches the credential server-side and forwards to GitHub. Use only if on-demand freshness is genuinely required.

**direct (dev and internal use only)**
Browser calls api.github.com with a token typed into the page. Never deploy this publicly. The token field is hidden automatically in the other two modes.

## 5. Security decisions and rationale

These are the constraints the design enforces. Do not relax them.

1. **No credential in the browser for any public deployment.** A token typed into a public page is readable in dev tools by anyone who opens it. This rules out direct mode for the leadership deployment.
2. **No credential in config.json ever.** The file is served as a static asset. Anything in it is public.
3. **No classic PAT.** Use a GitHub App installation token (short-lived, auto-rotating, repo-scoped) or a fine-grained PAT with read-only Pull Request permission on only the analyzed repos. Blast radius on leak: read PRs on those repos, nothing else.
4. **Proxy is deny-by-default.** GET only. Path allowlist covers exactly four endpoint shapes (PR list, PR detail, reviews, review comments). Repo allowlist read from config.json. Only known query parameters forwarded. Optional CORS origin restriction via ALLOWED_ORIGIN.
5. **Snapshot mode is the lowest-risk option.** If the dashboard host is compromised, the only thing exposed is metrics already visible to every viewer. Prefer it unless live data is a hard requirement.
6. **Secrets live in GitHub Actions secrets or server environment variables only.** METRICS_TOKEN secret for the workflow, GITHUB_TOKEN env var for local or server runs.
7. **Internal repo names are data.** metrics.json contains repo names and PR titles. If the dashboard is public beyond the org, confirm that is acceptable or strip titles in the collector before publishing.

## 6. Step-by-step build guide

### Phase A: Repository setup

1. Create a repo to host the dashboard, for example network-solutions/pr-velocity-dashboard.
2. Add the five files. Place update-metrics.yml at .github/workflows/update-metrics.yml. The other four sit at repo root.
3. Edit config.json:
   - dataSource: "snapshot"
   - repos: your ModelHub and Network Solutions repos as owner/repo strings
   - cutoffDate: the AIDLC adoption date, format YYYY-MM-DD
   - maxPRsPerRepo: start at 60, raise once the token and rate limits are confirmed
   - dateStart and dateEnd: optional, leave empty for no bound

### Phase B: Credential

4. Preferred: create a GitHub App in the org, permissions Pull requests read-only, install it on only the analyzed repos. Alternative: fine-grained PAT, same scope.
5. In the dashboard repo, add the token as an Actions secret named METRICS_TOKEN. If the analyzed repos are in the same org and the default GITHUB_TOKEN has read access to them, you can skip the secret; the workflow falls back to it.

### Phase C: First snapshot and validation

6. Run locally once to validate before automating:
   GITHUB_TOKEN=xxx node collect-metrics.js --config config.json --out metrics.json
7. Check the output: recordCount above zero, errorCount at or near zero, cutoffDate correct, spot-check two or three PRs against GitHub UI for cycle time sanity.
8. Serve the folder locally (npx serve or python3 -m http.server) and open the dashboard. Confirm the snapshot renders, the pre and post split looks right, and the generated timestamp shows.

### Phase D: Automation

9. Commit metrics.json alongside the dashboard. Push everything.
10. The workflow runs weekdays at 03:00 UTC and on manual dispatch from the Actions tab. Adjust the cron to your reporting cadence. It commits the refreshed metrics.json only when it changed.
11. Trigger one manual run from the Actions tab to confirm end to end.

### Phase E: Hosting

12. Simplest: GitHub Pages on the same repo. Settings, Pages, deploy from branch. Internal alternative: any static host or internal web server, the dashboard is a flat folder with no build step.
13. Access control is the host's job in snapshot mode. If this should be org-internal only, use an internally hosted static server or Pages with org SSO rather than a public Pages URL, because metrics.json exposes repo names and PR titles.

### Phase F: Optional live proxy (only if needed)

14. Deploy proxy-server.js on an internal host:
    GITHUB_TOKEN=xxx PORT=8787 ALLOWED_ORIGIN=https://your-dashboard-host node proxy-server.js
15. Point config.json at it: dataSource "proxy", apiBase set to the proxy URL. Repos in config.json double as the proxy allowlist, keep both deployments reading the same file.
16. Put TLS in front of it (reverse proxy or platform default). Do not expose it without ALLOWED_ORIGIN set.

## 7. Operating notes

- **Rate limits.** Each analyzed PR costs three API calls plus pagination for the list. 60 PRs across 3 repos is roughly 550 to 600 calls, well within the 5,000 per hour authenticated limit. Both collector and dashboard back off automatically on 403 rate-limit responses.
- **Changing the cutoff or repo list.** Edit config.json, rerun the collector (or wait for the schedule). The dashboard picks up the new snapshot on reload.
- **Export.** The dashboard has a raw JSON export for feeding slide decks or further analysis.
- **Extending metrics.** Add fields in analyzePR in collect-metrics.js, mirror in the dashboard's summarize and KPI render functions. The record shape is identical between collector and dashboard by design.

## 8. Leadership framing

The scatter trend (cycle time by merge date, colored pre and post) is the single most persuasive visual. The KPI cards give the numbers, the scatter shows the shift is real and not an artifact of a few outliers. Lead with those two. Caveat to state up front when presenting: this measures correlation with the adoption date, not causation, and PR mix (feature size, team composition) can move these numbers independently of AI usage. Pair it with the AI usage tracking data from the PR merge gating for the causal story.
