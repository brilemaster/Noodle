# Noodle and the Sunbeam Heist — handoff guide

Current version: **v23** (shown bottom-right of the title screen). Check a deploy with `?v=23`.

## Changelog
- v1: World 1 built.
- v2: game-feel update (owner asked: keep art, make it feel like a snappy Mario game). New movement physics, animation, juice, power-ups, treasures, flagpole, HUD clarity.
- v23: near skyline raised further (`MIDY = -VH*0.2`), with the strip under the raised layer filled in the layer colour so no gap shows. Browser zoom fix from v22 kept.
- v22: same look on every browser. Brave on iPad (page zoom below 100% / desktop mode) made the page report a bigger viewport than the real screen, so everything rendered smaller. `resize()` now detects that on touch devices (viewport larger than `screen`), computes `ZF` (zoom factor, 0.5–1), sizes the game in real-screen units (`VW/VH = css × ZF`, `DPR = devicePixelRatio / ZF`) and scales touch/click coordinates by `ZF`. Tested by simulating 85% zoom: identical layout and taps register.
- v21: readability: the near skyline layer is raised (`MIDY = -VH*0.1` in drawBackground; EQ meters, notes and SkyTrain use the same offset) and a soft haze gradient covers the lower 45% of the screen behind the action, so busy buildings never compete with Noodle. Applies to every city. Owner rule: backgrounds must never make the action hard to read.
- v20: **City 1 is now Nashville**, Noodle's hometown (owner and husband met and married there in 2015; he is an audio engineer). Start and finale are Nashville (replaces Magnolia Holler). Levels: Broadway Mornin', Honky-Tonk Hop, Music Row Wires, Ryman Rooftops, Hot Chicken Alley (Rocco). Themes 0–4 now `nash: true` → `makeNashLayer`: far = Batman (AT&T) tower, skyline, Cumberland River footbridge, and a **tiny distant pink neon "J ♥ B · 2015" sign** (owner wants it subtle, a secret for the two of them; keep it small and far away). Mid = Broadway honky-tonks with vertical neon blade signs (HONKY TONK, LIVE MUSIC, BOOTS…), neon boots/guitars, the Ryman. Music visuals: animated rooftop EQ meters and music notes drifting from windows, springs drawn as amp stacks (note-particle burst), secret rooms show a red ON AIR sign. Biplane banner says Y'ALL!. New song `honky` (twang voice) for 1-1 and 1-3. Finds: Tiny wedding ring ("Where it all began. Nashville, 2015. J ♥ B."), Guitar pick, Studio headphones, Mixing-desk fader. Buzz stays in 1-4 (route needs flying).
- v19: **City 2: Bangkok (during Songkran) built.** Levels 2-1 Soi Sunrise, 2-2 Songkran Splash, 2-3 Tangled Wires, 2-4 Tuk-Tuk Dash, 2-5 Lumpini Lake (boss Big Lou). New themes 5–9 (`bkk: true`): skyscraper + golden temple-spire far layer, shophouses with neon/shop signs, tangled wires, SkyTrain line with an animated train, concrete roof caps. New mechanics: Lumpini monitor **lizards** (`lizard()`; can't be beaten: stomp = big bounce, side bump = gentle push with no damage, yarn/slide/zoomies/tuk-tuk flip them for 3s), **Songkran water jets** (`spray()`, fans that also push sideways via `push`), **tuk-tuk** ride (`tuk()`, `L.drone.kind = 'tuk'`: ground ride, top speed 6.5, normal jumps, flips lizards). Big Lou reuses the Rocco boss engine with `arena.kind = 'lou'` (sunning rocks instead of bins, water balls instead of lids, naps in the sun when bop-able). New scenes: bkk1, tuk, lou1, lou2, w3tease; ending of City 1 now says "Next stop: THAILAND." Nap Sack now has one page per city (arrows). Two new songs: `soi`, `songkran`. 15 new finds. All levels pass the normal-player reachability check; Lou and Rocco bot fights pass.
- v18: touch controls nudged lower (bottom gap 8px + 30% of the home-indicator inset).
- v17: touch controls laid out like Super Noah (owner's reference): fully on screen, ~18px in from the side edges and ~20px up from the bottom; arrows ~12.5% of screen height (70–104px), jump 1.22× bigger at bottom-right, yarn the same size as the arrows just left of jump. (v16's tucked-off-edge buttons felt wrong in the hand.)
- v16: touch buttons moved down to the bottom edge (centre at ~44% of button size from the bottom, ignoring most of the home-indicator inset) for comfier thumbs. Tap zones unchanged.
- v15: City 1 is now **Magnolia Holler**, Noodle's small Southern hometown (Mayberry feel, KY/TN/AL mix): rolling hills, Main Street storefronts with awnings, gabled porch houses, oaks, courthouse clock tower, MAGNOLIA HOLLER water tower, tin roofs, a HOWDY! biplane banner, neighbors calling out greetings (`greet()` deco), Miss Pearl (neighbor) in the intro. Levels renamed: Main Street Mornin', Porch Hop, Telephone Lines, Feed Store Flip, Behind the Diner (Rocco). Ending: Noodle naps on a warm jar crate, wakes up in Bangkok. Map lists six cities.
- v14: Home Screen bottom strip: root cause is iPadOS standalone + `black-translucent` status bar leaving a strip below the web view. Switched `apple-mobile-web-app-status-bar-style` to `black` (owner must delete and re-add the Home Screen icon for iOS to pick it up), sizing back to the visible area (min of visualViewport/innerHeight), and the page background now follows the bottom colour of the current scene (`syncPageBg`). World map redrawn as rooftops (one building per level, jump arcs, sunbeam on the chosen roof, boss building with a flag); level numbers like "1-1" removed from the UI; "World" wording changed to "City". **Owner direction:** worlds become cities. Start in American cities, travel abroad, return to America; ~5 levels per city. Owner has specific cities in mind (ask for the list).
- v13: story rewritten so it's clear Noodle can only nap properly in a sunbeam (new lines in `SCENES`). Sun patches (`sunspot()`) on rooftops: stand still in one for ~1.5s and Noodle naps ("Ahh… sun."); in shade Noodle only yawns. Empty sunshine jars (`addJars()`) dot each level along the thief's trail. On the map, Noodle dozes if you leave it alone ~5s. Dialog text now wraps with the same font it draws (no overflow under Next).
- v12: fixed the dark strip at the bottom in Home Screen (standalone) mode, which v9's screen-size guess had made worse (it also cut off bottom buttons). Now: in standalone the canvas is sized with CSS `100vw`/`100vh` and the real size is measured with `getBoundingClientRect()`; browser tabs still use `visualViewport` so toolbars never hide controls.
- v11: punchier jump: same height (~4.2 tiles) but faster (`JV` -14.2, `G` 0.71, airtime ~39 frames); springs, bops, balloons, clotheslines, vine/zip releases scaled to keep the same heights. Boxes that sat 3 empty tiles above a floor are lowered by one (`lowerBoxes()` runs on every level), so a small hop bonks them (~6 bonks a second when holding jump). Checker's normal-player mode now scales the real jump instead of a hard-coded value.
- v10: far fewer pop-ups (no jump/boing/grab/zip words, no +1 per fish, no score numbers on bops; remaining words smaller and shorter; banners and hints moved up under the HUD; finds pulse their HUD icon instead of a message). Secret rooms 6 tiles wider with the exit door at the far right end. Jumping: hold jump to hop again the instant you land (`p.hopHeld`), 10-frame buffer, faster drop after bonking a block; fish boxes (`M`) give 2 fish per bonk and empty in 5 bonks.
- v9: upbeat music (all songs 15–30% faster, plucky lead, octave-hopping bass, claps/hats; menu has a light beat) and a new exciting "treasure" track for secret rooms. Secret rooms look like a party (spotlights, string lights, neon SECRET! sign, sparkles, confetti on entry). Story dialog: always one bar at the bottom, sized to the text, with Next (Go! on the last line) and a small Skip in the corner. 1-4 paint mixing replaced by a **flip switch** (`S` block toggles `L.flip`: `p` pink solid at 0, `q` blue solid at 1), level renamed **Flip-Flop Rooftops**. Noodle ~10% faster (`PH.RUN` 4.4) with a quicker scurry animation and running dust. Level complete now says "You made it across <level>!" with fish, new finds and stars labelled Made it / Found all 3 / Speedy (score hidden). Nap Sack simplified: one row per level with three big icons, tap for a card; stars total moved to the map. Cables now hold Noodle low on the rope (predictable swings); 1-3 paper plane moved onto the natural swing path. Checker now only counts a pickup if Noodle can land safely afterwards. Home-screen launch fills the whole screen.
- v8: music rebuilt for reliability: each built-in song is pre-recorded into a WAV track with OfflineAudioContext at launch and played through a plain `<audio>` element (`SONGEL`), unlocked on the first tap. Ignores the iPhone/iPad silent switch and works inside app viewers. The live Web Audio sequencer remains only as a fallback. Map top bar tidied: Home left, Nap Sack + sound buttons right, world switcher under the world name.
- v7: new grown-up art style across World 1 (owner approved the golden-hour / blue-hour mockups): one time of day per level (morning, noon, afternoon, golden hour, blue-hour dusk with moon and neon), hazy layered skyline with lit windows, water towers, antennas with string lights, richer bricks with dotted texture, thinner outlines, slightly lankier Noodle, pigeons in sunglasses. Flat HUD (hearts, fish count, "Finds", score) with music + sound toggle buttons at the top of play, map, Nap Sack, title and pause. Controls moved to thumb spots (arrows low-left, jump low-right, yarn to the left of jump) with bigger touch zones. Punchier jump (bigger stretch, dust, softer boing). Haptics (Android vibrate; iPhone iOS 18+ hidden-switch tap; iPad has no hardware) with a Vibration toggle in Settings. "Album" renamed **Nap Sack**, "treasures" are now "finds". Redesigned world map: one rooftop path, BOSS label, "Locked" text, "World 1 of 5" switcher, card with finds and best time, star legend. Home buttons on map, Nap Sack, pause. Secret rooms upgraded: treasure chests (+15 fish), bonus hearts, fish rain, fish fountain blocks; 1-4 jackpot has everything. Music fixes: louder mix, plus the silent-clip trick so iPhone/iPad play music with the ringer switch on silent. All glossy buttons replaced with flat pills.
- v6: fish pickups now count anything touching the visible Noodle (`pickBox`); 1-4 opening fish moved onto the path / purple bridge line; zoomed in (phones ~11 tiles tall, bigger screens ~15.5); blocks, paint cans, fish, treasures and power-ups drawn 16–65% bigger; Noodle drawn 10% smaller. Next: owner approved mocking up a less-kiddy art style (bolder contrast, time of day per level, richer rooftops, thinner lines, flat HUD) before redrawing.
- v5: Golden Hour home screen (owner picked direction A from three mockups: dusk sky, Noodle napping on a ledge in a sunbeam, Fraunces serif title, one Play pill, Album/Settings links, player chip). New Settings screen (sound, music, players). **No difficulty choice any more**: one balanced mode between old Normal and Hard (3 hearts, all checkpoints, enemies 1.15x, par 0.92x, sharper Rocco). Built-in music (Web Audio sequencer like Super Noah): title, one song per level, secret-room music box, funky boss, faster during Rainbow Zoomies; a real `music.mp3` replaces it if added. Story cut to 1-3 short lines per scene, Skip always shown. Fixes: Buzz waits again if you respawn before his drop-off (was a softlock), paint resets on respawn, recipe signs by every paint puzzle. Buzz now cruises at a steady height (hold jump to climb, let go to glide back; no fuel, can't sink into pits).
- v4: owner's feedback: v3 felt laggy, too fast, crowded ("like hard mode on Normal"). Owner's favourite reference is their earlier game **Super Noah** (owner uploaded its index.html). Matched its movement numbers, camera and phone zoom, Easy speed (45%), removed hit-freeze, pre-drew level tiles in cached chunks, and rebuilt 1-1, 1-2, 1-3 in Super Noah's open style (mostly flat, 3-tile gaps, ~3-tile-high platforms, spaced enemies, long fish rows). 1-4 lost the flyers in Buzz's path. **Super Noah's feel is the target for all future work.**
- v3: "super tight, crispy" controls; slopes with belly-slides; ziplines; balloons, fans, mattress trampolines, bouncy clotheslines; boomerang yarn ball always available (own round button); round arrow buttons; fish streaks, "Trail clear!" bonuses, 100-fish celebrations; pigeons flap away when bopped; livelier background (neighbours in windows, laundry, blimp, flocks, foreground silhouettes, chimney smoke); empty hearts clearly empty; every goodie re-checked for a normal player.

## What this is
A Mario-style browser platformer for friends and family (teens and adults). One self-contained `index.html`, HTML5 canvas, no frameworks, fixed 60fps loop. Hosted on GitHub Pages.

## Files to upload
- `index.html` — the whole game.
- `HANDOFF.md` — this guide.
- `music.mp3` (optional, owner adds) — looped with a plain `<audio>` element, starts on first tap, stops when the tab is hidden or closed.

## Content rules (updated by owner)
- The original "no holidays, no magic, no luck themes" rules are **lifted**. Holidays/festivals (e.g. Songkran, Yi Peng/Loy Krathong), magical touches and lucky items are all fine. Owner: "No boundaries here, let's get fun." Still keep it family-friendly. Temples stay as background scenery (cultural courtesy).

## The journey (approved plan)
1. **Nashville** (home, v20; was Magnolia Holler in v15) → nap on a jar crate at the Nashville airport → cargo plane.
2. **Bangkok during Songkran**: water-fight crowds that push you back, buckets, water balloons, tuk-tuk ride, Lumpini Park monitor lizards as obstacles you bounce off; boss Big Lou the monitor lizard.
3. **Chiang Mai**: misty jungle, bamboo bridges, waterfalls, Yi Peng lantern night level (floating lantern platforms), elephant buddy; boss a jar-hoarding macaque.
4. **Krabi**: limestone cliffs, beach caves, longtail boats, wall-jumps, fish-stealing monkeys; boss a grumpy upside-down fruit bat.
5. **Phuket**: Old Town shophouses, beaches, rising/falling tide; boss a hermit crab wanting a sunbeam-jar shell.
6. **Koh Samui**: coconut groves, bungalows, full-moon beach party, coconut cannons, wave surfing; boss a giant coconut crab who reveals the next country.
7. More countries (to brainstorm with owner), then home to Nashville for the finale (Don Coo's sunbeam garden). ~5 levels per city, levels ~2–3 minutes, build one city per update.

## Story so far (approved)
- Core idea (owner emphasised in v13): Noodle can only nap properly in a sunbeam. Every morning at 7:42 a sunbeam lands on Noodle's windowsill; one day it doesn't. "No sunbeam, no nap." Ending: Don Coo was bottling sunbeams to grow a rooftop garden; he shares, and Noodle naps in the best sunbeam in town.
- Hero: **Noodle**, a sleepy orange tabby. No pronouns anywhere; other characters call Noodle wrong nicknames ("Loaf", "small orange couch", "Mr. Whiskers?", "the orange one").
- Every sunbeam in Old Town has been bottled; a trail of empty jars leads over the rooftops.
- Final villain: **Don Coo**, a pigeon "mob boss" in a hat, secretly using the sunbeams to grow a rooftop garden for the neighborhood. Ending: Noodle shares a sunbeam and gets the best nap spot.
- Ride-on buddy: **Buzz**, a grumpy delivery drone ("rated for parcels under two kilos").
- Rules: no real people's names, no holidays, no magic/wizards/witches, no luck themes, no lessons or quizzes, no spoken voices.

## Worlds
| World | Name | Status | Boss | Planned mechanic |
|---|---|---|---|---|
| 1 | Old Town Rooftops | **Built (v9), awaiting approval** | Rocco the Raccoon (just guarding his home bins) | basics, springs, cables, paint mixing, Buzz |
| 2 | Neon Night Market | Planned | Bruno the shy bulldog bouncer | neon color signs |
| 3 | The Construction Site | Planned | Wally the clumsy wrecking ball | number blocks bonked in order to build bridges |
| 4 | Rainy Water Towers | Planned | Dramatic hat-stealing seagull | slippery puddles, leaky pipes |
| 5 | The Clock Tower | Planned | Don Coo | everything |

**Once a World is approved, do not change how it plays.** World 1 levels are the functions `b11`–`b15` in the core section.

### World 1 levels
- 1-1 Morning Gutters: basics, umbrella block, secret chimney star room, album: spare house key.
- 1-2 Awning Bounce: striped awning springs, secret window room, album: tiny café spoon.
- 1-3 Antenna Alley: swinging cables, yarn block, album: kite with no string.
- 1-4 Flip-Flop Rooftops: flip switch (pink/blue platforms), Buzz cruise ride, zoomies block, jackpot secret room.
- 1-5 Rocco's Bins: boss. Rocco throws lids from a bin, then sits dazed on the bin; bop him 3 times. Lids can be bopped too. Gate closes on entry; the right wall opens after his apology scene. Album: bottle cap crown.

## Game feel — matched to Super Noah (tuning knobs: `PH` and `MOVE` in the core section)
- Run: top speed 4 (no auto-sprint), accel 0.7 ground / 0.45 air, letting go multiplies speed by 0.7 per frame on the ground (0.94 in the air): a smooth short glide. Over-speed (slides, zoomies 5.6) decays by 0.93.
- Jump: launch 12.5, gravity 0.55 both ways, max fall 13. Releasing jump caps upward speed at 4 (short hop). Jump buffer 8 frames, coyote 7 frames. Ledge assist (≤9px) and a small step-up where slopes meet roofs.
- Easy mode code (`MOVE.easy`) still exists but is no longer used: v5 has one balanced mode for everyone.
- Camera (Super Noah): phones (view < 520px tall) show ~12 tiles of height; bigger screens show the whole 16-tile level and at least ~30 tiles across. Hero sits 40% from the left; follow 0.15 horizontal / 0.12 vertical.
- No hit-freeze on bops (read as lag on phones). Static tiles are pre-drawn into 16-column chunk canvases (`drawStaticChunks`, cleared by `chunkClear()` whenever a tile changes).
- Level style rules (Super Noah): mostly flat ground, gaps ~3 tiles, platforms ~3 tiles above where you stand, ~10 enemies per level and never right after a landing, long fish rows, one checkpoint, one or two special features per level.
- Springs and mattress trampolines (`^`, `T`): always full height (~8 tiles), no holding trick. Bouncy clothesline (`~`): ~6 tiles. Balloons: bounce, higher if holding jump. Enemy bop: high bounce, higher if holding jump.
- Slopes (`Q` rises right, `K` falls right; builders `slopeUp`/`slopeDown`): running downhill becomes a belly-slide that bowls over pigeons and carries onto flat roofs.
- Ziplines (`zip(L,x1,h1,x2,h2)`, high to low): jump into the line to grab, accelerate down, jump to let go with momentum.
- Fans (`fan(L,x,s,height)`): updraft column, hover near the top.
- Yarn ball: always available (pink round button above jump, or X). Boomerangs out and back on a string, hits enemies both ways, reels in fish, hurts Rocco only while he's dazed. One at a time.
- Animation: legs cycle by distance travelled, feet blur at sprint speed, forward lean, skid pose, stretch on takeoff, squash on landing (scaled by fall speed), dust puffs on start/turn/jump/land, tail drags behind.
- Juice: fish streaks (banner every 10), fish trails (`trail()`/`coins()` groups) give "Trail clear! +300", every 100 fish is celebrated. Every pickup has sound + sparkles + "+1"; fish in quick succession climb a musical scale. Blocks bounce, items arc out. Bops: boing, 3-frame hit-freeze, shake, star burst, POW!/NICE!/BOING!; 3 bops without landing = SUPER COMBO! banner + 1000 bonus. Camera leads in the running direction.
- Power-ups: Big Snack (Chonky Noodle, bigger, absorbs a hit), Umbrella (glide), Rainbow Zoomies (invincible, rainbow colors, music 1.3x speed). Transformation flash with fanfare. A pickup applies immediately if Noodle has no power; otherwise it goes to the pouch. Using a pouch item swaps the current power back in.
- Blocks: `?` fish, `M` multi-fish (8 bonks), `C/U/Z` power blocks (paw icon), `H`/`J` hidden blocks (invisible until bonked from below: fish / Big Snack).
- Secret rooms are packed with fish; golden fish = 5. 1-4's secret room is the special **Jackpot Room**.
- 3 hidden treasures per level (15 in World 1) fill the album; HUD shows their silhouettes.
- Level end: flagpole (height bonus up to 5000), sun flag rises, fanfare, Noodle naps on the sunbeam cushion, stars pop in and totals count up.
- HUD: pouch only appears once it holds something (with a bag icon and label); treasures labelled; timer shown on Hard only (the time star still counts on every difficulty).

## Systems (all in index.html)
- **Difficulty** (`DIFF`): Easy 5 hearts, bubble rescue from pits, enemies 0.65x speed, generous boss timing, boss keeps damage after a death. Normal 3 hearts. Hard 2 hearts, faster enemies, tighter par, fewer checkpoints (`flag(..., 'noHard')`).
- **3 stars per level:** finish / all 3 treasures (collected across any runs) / beat par time (`L.par` × difficulty multiplier).
- **Pouch:** holds 3 extra power-ups; tap a slot or press 1/2/3. Saved between levels.
- **Combos:** bops without landing: Bop, Double bop, Triple bop, Quad bop, Purrfect, Cat-astrophic. 50 fish = +1 heart.
- **Secret rooms:** via sparkly chimneys or glowing rooftop windows (`warp(...)`), separate camera room (`secretRoom`).
- **Album:** 3 treasures per level (`ALBUM` in core); tap an item to read its description.
- **Profiles:** up to 4, first name only (DOM overlay so phone keyboards work). Save key `noodle-sunbeam-save` in localStorage, saved automatically.
- **Dialog:** comic panels, typewriter with blip sounds, Skip shows only for scenes already seen (`prof().seen`). Scenes in `SCENES`.
- **Controls:** round left/right buttons (thumb slides between), big round jump button, round yarn button above it. All fingers up resets everything. All fingers up resets everything. Keyboard: arrows/WASD, Space/Z jump, X throw, 1-3 pouch, Esc pause.
- **Phone zoom:** `SCALE` from the real visible height (`visualViewport`), ~9 tiles tall on phones up to 15 on big screens; camera follows up and down. Portrait shows a "turn sideways" screen.
- **Leaderboard:** owner said yes, add later. Not built yet — propose a simple free option and ask before adding.

## Source layout (for editing)
The single file is assembled from parts: `core.js` (physics + levels, no DOM), `audio.js`, `draw.js` (all art), `game.js` (screens, gameplay, HUD, input, save). In a new conversation, work directly in `index.html` — the sections are marked `// ===== CORE`, `AUDIO`, `ART`, `GAME`.

## Testing (repeat for every update)
- The checker also starts from every checkpoint (as a respawn would) to catch softlocks, and models letting go of jump after boarding Buzz.
- `node test/check.js` (expert reach), `CASUAL=1 node test/check.js` (normal player: 80% jump height, slower run, simple timing; every item must pass this), `BIG=1 node test/check.js` (Chonky hitbox).

## Testing done (v1–v3)
- Reachability checker runs the real physics (many jump styles from every standable spot): every coin, star, sun coin, album item, prize block, goal and both Rocco stomp spots reachable; no softlocks; no accidental 1-tile pits.
- v2: checker re-run with the new physics, also with Chonky Noodle's bigger hitbox; coyote/buffer/accel/stop measured in tests.
- Headless runs of all screens at 844×390, 667×375, 568×320, 1180×820, 1440×900 with no errors; bot playthroughs; full boss victory flow; real Chromium multi-touch test.

## Working agreement
- Propose ideas and get OK before big changes.
- Test reachability, boss safety and phone size before sending.
- Bump `VERSION` in the core section every update and tell the owner which files changed plus the `?v=` number.
- Keep this file current.

## Next steps
1. Owner plays World 1 and gives feedback.
2. After approval: pitch World 2 (Neon Night Market) levels for OK, then build.

## Music
- Built-in songs live in `SONGS` (audio section); `renderSong()` records them to WAV, `SONGEL` plays them: NOTE+octave:length in eighth notes, bass = one chord root per bar. `SONG_FOR` maps levels to songs. `playSong(name)`, `stopSong()`, tempo via `musicRate()`.
- If the owner uploads `music.mp3`, it plays instead (plain `<audio>` element). Separate Suno files per level would need a small code change.

## Working faster (owner asked)
- Only re-test levels that changed; run checks in parallel in the background; skip screenshot passes unless visuals changed.
