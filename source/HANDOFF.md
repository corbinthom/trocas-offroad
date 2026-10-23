# Handoff: Trocas Mamalonas Off Road

Paste this into a new Claude chat and attach:
- `trocas-off-road-source.zip`: the source
- `Trocas-Off-Road.html`: the current playable game

## What this is
A fresh, one-screen stadium truck racer in the style of the classic arcade off-road games (not the old slither game).
It is an original track and original art, not a copy of any real game. It is drawn in real 3D with three.js
(a 3D drawing library, MIT license, built into the file), in the style of a modern top-down 3D racer: an angled camera that
follows your truck, sunlight and shadows, raised dirt banks, and toy-style vehicles. Kid friendly. ONE html file that works offline,
on phones first, and embedded in a website with an iframe. Branded with the Trocas Mamalonas logo
(start screen, results screen, and painted on the infield like a sponsor sign).

## How it plays
- Pick a ride (Stadium Truck, Jeep, Muscle Car, Classic) and one of 8 paint colors. The computer drivers get other colors and random rides.
- Races of 3, 4 or 5 laps with a 3-2-1-GO countdown, flagman, results table.
- Track: red and white walls, two jump ramps (see Jumps below), three moguls, two water puddles (slow and splash), two mud patches (slow and slide).
- Nitro: start with 3, grab nitro bottles on the track. Money bags give $50. Trucks bump each other like bumper cars.
- HUD: your place, lap, nitro bottles, race clock and a mini-map with every truck. A Low graphics switch (no shadows, fewer pixels) is remembered per device.
- Computer drivers stay close to you (a little faster when far behind, slower when far ahead) so races stay exciting.

## Crashes
- Each truck is a capsule (two circles along its length, radius 8.6, or 10 with monster wheels) so bumpers really meet.
- Hits bounce trucks apart (restitution 0.35), scrape off a little speed, and an off-center hit adds spin (t.yaw).
  Monster trucks count as 1.35 times heavier and gold trucks 1.6, so they shove harder.
- You feel it: crunch sound (thud, metal, clank, louder with bigger hits; nearby computer crashes are quieter and panned),
  screen shake, a phone buzz, sparks and dust at the contact point, and a sideways body jolt.
- Measured (tests/crash-tests.json): rear-ending a parked truck at full speed pushes it to about 1.8 and drops you to 0.8;
  a side hit into the back corner spins it about 34 degrees; bumpers stop exactly at contact (no overlap).

## Gold Trocas coins
- 7 gold coins with the Trocas logo spin on the track at a time; a new one appears about every 0.7 s until there are 7 again.
  Each is worth $100. Any truck can grab them (computer trucks too), but not while high in the air.
- 5 coins: monster wheels (1.6 times bigger, wider stance, body lifted). They also lose less speed in mud and water.
- 10 coins: solid gold paint and fire from twin tailpipes, plus a 4 percent top-speed bonus.
- Counts reset every race. HUD shows coins toward the next upgrade and your money; results show everyone's money.

## Music
- "Joyas de Oro" (instrumental), compressed from the WAV to a 96 kbps MP3 (`music.mp3`, 1.5 MB) and built into the file.
  It loops quietly (gain 0.2) through Web Audio, so the volume works on iPhones, starts on the first tap or key press,
  pauses with the game, and M mutes it with everything else.

## Smooth drawing
- Physics runs in fixed 60-per-second steps; the 3D drawing blends each truck between its last two steps (ipos in world3d.js),
  the camera follows that blended position with frame-rate-independent smoothing, and the suspension ripple follows the
  wheels instead of the clock. This removed the slight vibration.

## Jumps
- A ramp is a wedge: trucks climb the face (lifted, nose tipped up) and launch from the bright lip.
- Every truck that crosses the lip gets air, scaled by speed (launch() gives each jump its own height and hang time).
  Measured: slow 0.68 s / peak 32 / 105 units; normal 0.78 s / 39 / 157; nitro 0.68 s / 46 / 216 (higher, not much longer,
  so it still lands on the straight). Moguls use the same arc at 30 percent size.
- In the air the truck grows up to 30 percent, its shadow stays on the dirt and shrinks, nose up rising and down falling,
  airborne trucks draw on top and fly over other trucks.
- A little steering in the air turns the truck but barely bends its path. Landing crooked (more than about 17 degrees off)
  costs up to 60 percent of speed and shows "Rough landing!". Landing dust and thump scale with the drop.
- Walls still hold trucks in the air but do not scrub their speed there.

## Controls
- Computer: arrows or WASD (Up gas, Down reverse, Left/Right steer), Space or Shift nitro, P or Esc pause, M mute, Enter starts.
- Phone sideways: drag left or right with your left thumb anywhere on the left side (steering wheel); Gas, Nitro, Back stacked on the right.
- Phone upright: the camera follows your truck; arrow buttons, Nitro, Gas and Back below the track.
- Auto gas (on by default on touch screens) means kids only steer.

## How the code works
- `game.src.html` is the game (HTML, CSS, physics, AI, race flow, input, sound). `world3d.js` is the 3D world, vehicles,
  camera and effects. `python3 build.py` inserts three.min.js, world3d.js, the fonts and logos and writes `Trocas-Off-Road.html`.
- The physics is 2D: world is 960 x 600 units plus a height. In 3D, world x is three.js x, world y is three.js z, height is y.
  The track is a smoothed closed centerline (CTRL points) resampled every 6 units; walls are offsets of it.
  Features (ramps, bumps, water, mud) are placed by world point (FEAT). Physics runs at a fixed 60 steps a second.
- `window.__game` exposes state for tests (step(), startRace(), trucks, me, T, FEAT).

## Tests (in tests/)
- Headless browsers draw 3D in software, very slowly. Tests set `window.__hold=true` to pause the game loop, call
  `__game.step()` directly, and `__game.snapCam(); __game.render3D(1/60)` to draw one frame for a screenshot.
- `stress-random-races.json`: 20 races where the player first drives randomly, then the computer takes over. Every race must
  finish and no truck may crawl (speed under 0.6) for more than 2 seconds.
- `jump-measure.json`: airtime, height and distance at slow, normal and nitro speed, and a crooked landing.
- `test.py`: runs steps in a headless browser and screenshots. A full computer-driven race should finish with every truck,
  trucks never past the wall limit (33), no NaN, nobody stuck for long.
- `ctl.py`: keyboard drive, steer, nitro tap, pause, results, race again, and on a sideways phone the Gas button and steering wheel.
- Measured: 3 laps in about 52 to 57 seconds, laps about 15 to 17 seconds, all four finish within a few seconds.

## Fixed bugs worth knowing
- A stopped truck facing the wrong way could never turn (trucks only turn while moving). The computer now keeps a little gas on.
- The computer's back-up-when-stuck rule switched itself on and off and made trucks rock in place. Backing up is now a
  committed 45-frame move that turns toward the track.

## Website and phones
- `python3 build.py` makes two outputs: `Trocas-Off-Road.html` (one self-contained file) and `site/` (the website: index.html
  plus manifest.webmanifest, sw.js, icons, share.jpg, README.txt with hosting steps). Zip site/ and upload it anywhere https.
- Installed from the home screen it opens full screen and sideways (manifest display fullscreen, orientation landscape).
  Android shows an "Install the game" button (beforeinstallprompt); iPhone Safari shows "tap Share, then Add to Home Screen".
- In a phone browser, tapping Race asks for full screen and a sideways lock (launchMobile), where the phone allows it.
- sw.js caches the game for offline play and updates in the background (bump the cache name if icons change).
- Tested: manifest loads, offline helper takes over, the game reloads with the network off, iPhone how-to shows.

## Rules for working on it
- Never use em dashes in replies to me.
- Kid friendly. Phone first. Keep it one file that works inside an iframe.
- Test before handing back. Remind me to put the new file on every device and the website.

## Ideas for next
- Between-race shop (tires, shocks, acceleration, top speed, nitros) paid with the money bags, like the arcade.
- More tracks. Two players on one screen or over the internet.
