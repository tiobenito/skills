# Deliver & host — a stable URL for the living dashboard

The whole value of a *living* dashboard is a **stable URL** that always shows the latest — so the
rule is: host it once, update forever. The dashboard is a single self-contained `index.html` with
no build step and no external data, so it hosts anywhere static files can live.

## Hosting options

Pick whatever fits your stack. All of them give you one URL you publish once and overwrite on each refresh:

| Option | How | Good for |
|---|---|---|
| **Open locally** | Just open `index.html` in a browser | Quick personal use; no sharing needed |
| **GitHub Pages** | Commit `index.html` to a repo; enable Pages → served at `<owner>.github.io/<repo>/` | Free, version-controlled, public or private-org |
| **Netlify / Vercel** | Drag-and-drop or connect the repo; redeploys overwrite the same URL | Teams that want a custom domain |
| **S3 / static bucket** | Upload `index.html` to a bucket with static hosting; overwrite the same key on refresh | Internal/company hosting |
| **Internal static host** | Drop the file wherever your org serves static HTML | Locked-down internal dashboards |

## The stable-URL rule

Whatever you choose, **the URL must not change between refreshes** — that's what lets people
bookmark it and what makes "I updated the dashboard" mean something.

- **First host:** note the URL in the project's `README.md` and at the top of `state.md`, with the
  exact command/steps to update it, so no future session re-hosts somewhere new by accident.
- **Every refresh after:** overwrite the file in place (same path / same key / same deploy). Do
  NOT publish to a new location. Confirm to the user the link is unchanged.

## Verify before declaring done

```bash
# 1. div balance (a stray unclosed tag silently breaks layout)
o=$(grep -o '<div' index.html | wc -l); c=$(grep -o '</div>' index.html | wc -l); echo "open=$o close=$c"
# 2. if hosted, confirm the live URL serves the new content
curl -s "<your-stable-url>" | grep -c "Updated"
```
