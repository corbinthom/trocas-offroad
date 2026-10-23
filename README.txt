TROCAS MAMALONAS OFF ROAD: PUTTING THE GAME ONLINE
===================================================

This folder is the whole website. Upload EVERYTHING in it, together, to any free host.
The host must give an https:// link (all of these do). Then anyone can open the link,
no account needed, and phones can add it to the home screen like an app.

Files:
  index.html              the game
  manifest.webmanifest    tells phones the name, icon, full screen and sideways
  sw.js                   keeps the game on the phone so it opens fast and works offline
  icon-*.png, apple-touch-icon.png, favicon.png   home screen and browser icons
  share.jpg               the picture shown when you send the link in a text or post

OPTION 1: Netlify Drop (quickest, free)
  1. Go to app.netlify.com/drop
  2. Drag this whole folder onto the page.
  3. You get a link like https://something.netlify.app right away.
  4. Make a free account to keep the link and rename it (for example trocas-offroad.netlify.app).

OPTION 2: GitHub Pages (free, good for the long term)
  1. Make a free GitHub account and a new public repository (for example "offroad").
  2. Upload all the files in this folder (Add file, Upload files).
  3. Settings, Pages, Source "Deploy from a branch", branch "main", folder "/ (root)", Save.
  4. In a minute the link is https://YOUR-NAME.github.io/offroad/

OPTION 3: Cloudflare Pages (free)
  1. In Cloudflare: Workers & Pages, Create, Pages, Upload assets.
  2. Drag this folder in. You get https://your-project.pages.dev

OPTION 4: Your own website
  Upload the folder to a subfolder, for example yoursite.com/offroad/ , and link to it.
  To show the game inside a page instead, use:
  <iframe src="https://YOUR-LINK/" style="width:100%;aspect-ratio:16/9;border:0"
          allow="fullscreen; autoplay; screen-wake-lock" allowfullscreen></iframe>
  (On phones, sending people straight to the link is better than a page with the game inside it.)

ON PHONES
  Android (Chrome): open the link, tap "Install the game" on the start screen
    (or the browser menu, "Add to Home screen"). It opens full screen and sideways.
  iPhone (Safari): open the link, tap Share, then "Add to Home Screen".
  Playing in the browser without installing also works: tapping Race goes full screen
    and sideways where the phone allows it.

UPDATING THE GAME LATER
  Replace index.html with the new one. Phones show the saved copy right away and fetch the
  new one in the background, so the new version shows up the next time the game is opened.
  If you ever change the icons, also change "trocas-offroad-v1" in sw.js to "v2".
