# Cover Finder

Single-page tool to find the best-skilled replacement when staff are absent.

## Deploy to Vercel

**Option A: drag and drop**
1. Go to https://vercel.com/new and choose to deploy without Git (or use the Vercel dashboard upload).
2. Drop this folder in. No build settings are needed: Framework Preset = "Other", no build command, output directory = root.

**Option B: CLI**
```bash
npm i -g vercel
cd cover-finder-vercel
vercel          # preview deploy
vercel --prod   # production deploy
```

**Option C: GitHub**
Push this folder to a repo, then import it at https://vercel.com/new.

## Files
- `index.html`: the whole app (HTML, CSS, JS). Loads SheetJS from cdnjs for Excel import/export.
- `vercel.json`: static hosting config (clean URLs, basic security and cache headers).

## Note on real-time sync
On Vercel the app runs in **preview mode**: changes are saved in each person's browser and sync across tabs on the same computer, but not between different people's devices. Sharing changes across devices needs a small shared backend (for example Vercel KV / Upstash Redis, or Supabase).

## Excel format
`Name | Skills | <one column per date>`
- Skills: comma-separated
- Shift codes: `AM`, `PM`, `NT`, `OFF`
- Absences: `MC`, `AL`, `ABS`, optionally with a shift, e.g. `MC-AM` (no shift = full day)

Use **Get roster (.xlsx)** in the app to download a ready-made template.
