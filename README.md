# Ocean Water Mass Explorer

WebGL2 volume renderer for World Ocean Atlas 2023 temperature, salinity and dissolved oxygen (annual and monthly),
with a water-mass diagram (T-S, T-O2, S-O2) whose selections (rectangles and curves) extract fiber surfaces in the 3D volume.

**Live app: https://chinmay0303.github.io/oceanmassexplorer/**

## Open the app (no installation)

Open the link above. The app is a static website on GitHub Pages, so nothing has to be installed.
Use a current Chrome, Edge, Firefox or Safari (WebGL2 is required). The first load downloads about 19 MB (annual data);
choosing Monthly downloads about 150 MB more (all 12 months, kept in the browser's graphics memory, roughly 0.4 GB).

## How the site is published

The site is already live. Every push to `main` that changes code (not only documentation) is built and published automatically by
`.github/workflows/pages.yml`: it generates the data from the public NOAA files (not stored in git), builds the app and deploys it.
A run takes about 2 minutes (a few seconds longer without the cached data). Pushes to other branches only build, without deploying.
Progress and errors: the Actions tab on GitHub.

To publish from a fresh repository: make it public (free Pages needs a public repository), set Settings > Pages > Source to "GitHub Actions",
and push or merge the code into `main`.

## Development setup

Needs Git and Node.js 20.19 or newer.

1. Clone the repo and switch to the working branch:

       git clone git@github.com:Navinas3460/oceanmassexplorer.git
       cd oceanmassexplorer

2. Install the dependencies:

       npm install

3. Download the data (341 MB unpacked) from Google Drive and save `woa23.zip` in your downloads directory:
   https://drive.google.com/file/d/1OTczr9tlrDxFRFFEb6hOi05PDWdETCT9/view?usp=drivesdk
   (for a large file Drive shows a virus-scan page: click "Download anyway")

4. Unzip it into `public/data/` (replace `<downloads-directory>` with the folder the zip was saved to):

       cd public/data
       unzip <downloads-directory>/woa23.zip
       cd ../..

5. Check that this file exists (the folder must not be nested twice):

       ls public/data/woa23/meta.json

6. Start the app and open the address it prints (usually http://localhost:5173):

       npm run dev

If the data is missing, the app falls back to the old annual temperature/salinity files and shows "Annual (legacy)".

## Using the app

- **Data**: choose Annual (0-5500 m) or Monthly (0-1500 m). Monthly loads all 12 months first (about 0.4 GB of GPU memory),
  then the month slider, Play/Pause (also Space), speed, month blending and looping become available.
- **Water-mass diagram**: choose Temperature-Salinity, Temperature-Oxygen or Salinity-Oxygen. Tools: Rectangle and Curve draw
  (up to 8 rectangles and 4 curves), Select picks an item, drag moves it, a corner resizes a rectangle, the Delete key removes it.
  The list below the diagram shows every selection: choose how a rectangle is drawn in 3D (Surface, Volume or both), hide it, or delete it
  (Delete all removes everything). The 3D view shows all ticked selections of every diagram together.
- **Volume**: tick "Colour the ocean by one field" to colour by temperature, salinity or oxygen with its oceanographic colour map
  (thermal, haline, oxy; Viridis or Cividis can be chosen instead); a colour bar shows the values.
- **Quality** (Volume section): Fast / Normal / High sets the number of ray-march steps; High gives the most exact surfaces.
- **Picking**: click the ocean in the 3D view to read temperature, salinity, oxygen, position and the selections containing the point;
  the point is marked in the diagram and pinned in 3D (Esc or a click on empty space clears it).
- Camera: drag to rotate, right/Shift-drag to pan, scroll to zoom; Reset view button or the Home key resets it.

## More

`log.md` has the decisions and history (including how the data was built with `tools/download.py` and `tools/prep.py`),
`CODE_MAP.txt` shows where everything is in the code, `ROADMAP.txt` the planned improvements.
