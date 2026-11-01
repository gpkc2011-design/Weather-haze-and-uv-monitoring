# PIE & CTE Traffic

A live dashboard for Singapore's Pan Island Expressway (PIE) and Central Expressway (CTE): average speed, a map of every road segment coloured by speed, LTA travel-time estimates, incidents and nearby traffic cameras.

## What you need
A free **LTA DataMall API key**. Request one at https://datamall.lta.gov.sg (Request for API Access). It arrives by email.

## Deploy to Vercel
1. Unzip this folder.
2. In Vercel choose **Add New → Project** and import the `pie-cte-traffic` folder (drag and drop, Git, or `vercel` CLI from inside the folder).
3. Framework preset: **Other**. No build command or output directory.
4. Before or after the first deploy, open **Settings → Environment Variables** and add:
   - Name: `LTA_ACCOUNT_KEY`
   - Value: your DataMall key
5. Redeploy so the function picks up the key.

CLI alternative:
```
npm i -g vercel
cd pie-cte-traffic
vercel env add LTA_ACCOUNT_KEY
vercel --prod
```

## How it works
- `index.html` is the whole front end. It calls `/api/traffic` every 2 minutes.
- `api/traffic.js` is a Vercel serverless function. LTA DataMall needs the key sent from a server, so the function does the fetching and keeps your key private. It:
  - pages through **Traffic Speed Bands (v4)** and keeps only PIE and CTE segments,
  - reads **Estimated Travel Times** for PIE and CTE,
  - reads **Traffic Incidents** and keeps those that mention PIE or CTE,
  - reads traffic camera images from **data.gov.sg** and keeps cameras within 300 m of a PIE or CTE segment.
- Responses are cached at Vercel's edge for 2 minutes, so LTA is queried at most once every 2 minutes however many people open the page.
- `vercel.json` gives the function up to 30 seconds, because speed bands come in many pages of 500 rows.

## Notes
- Average speed uses the midpoint of each LTA speed band (band 8 is 70 km/h and above, counted as 70).
- The coastline on the map is approximate. Road lines come from LTA's own segment coordinates.
- If the key is missing or LTA is unreachable, the page says which feed failed and leaves that panel empty. It never shows made-up readings.
