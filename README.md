# Singapore Air Quality

A single-page dashboard showing NEA's 24-hour PSI by region, the islandwide UV index and wind, fetched live in the browser from the data.gov.sg real-time APIs.

## Deploy to Vercel

**Option 1: drag and drop**
1. Unzip this folder.
2. In the Vercel dashboard choose **Add New → Project**, then drag the `sg-air-quality` folder in.
3. Framework preset: **Other**. No build command, no output directory. Deploy.

**Option 2: Vercel CLI**
```
npm i -g vercel
cd sg-air-quality
vercel          # preview deployment
vercel --prod   # production
```

**Option 3: Git**
Push this folder to a GitHub repo and import it in Vercel with the **Other** preset.

## Files
- `index.html`: the whole app (HTML, CSS and JavaScript in one file)
- `vercel.json`: clean URLs and basic security and cache headers. The site also works without it.

## Data sources
- PSI, PM2.5, PM10, UV index, wind speed and wind direction: National Environment Agency via https://api-open.data.gov.sg/v2/real-time/api (falls back to https://api.data.gov.sg/v1/environment).
- The page refreshes every 5 minutes. If the APIs can't be reached, it shows a clearly labelled saved NEA reading and leaves the other panels empty instead of estimating.
- data.gov.sg works without an API key but applies rate limits. For heavy traffic, request a key at data.gov.sg and add an `x-api-key` header in the `getJSON` function in `index.html`.
