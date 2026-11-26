# Broncos Live Tracker for Laptop

A standalone, single-file browser scoreboard that mirrors the ESPN game
selection and live-game details in the ESP32 project. It runs on the laptop,
so there is no firmware build, audio server, or extra application to install.

## Publish with GitHub Pages

The site is static and does not need a build step. To host it with GitHub Pages:

1. Create a GitHub repository and add `README.md`, `.gitignore`, and `index.html`
   from this project. The older `index v*.html` drafts are intentionally
   excluded from Git uploads.
2. In the repository, open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select the
   `main` branch and `/(root)`, then save.
4. Once GitHub Pages finishes deploying, open the URL shown on that settings
   page. For a repository named `nfl-live-tracker` under `imadeLunatic`, the
   expected URL is `https://imadelunatic.github.io/nfl-live-tracker/`.

GitHub Pages requires HTTPS for some browser features, including Screen Wake
Lock. If you first create the repository as private, check your GitHub plan's
Pages availability; you may need to make the repository public before hosting
it. Review the repository contents and visibility before making it public.

The page calls ESPN's public endpoints directly from each visitor's browser.
Preferences and play history stay in that browser's local storage; ESPN still
receives the visitor's requests. The endpoints are third-party services and
may change or become unavailable.

No license is included. Without a license, others should not assume they have
permission to reuse or redistribute the code.

## Start

Open `index.html` in an up-to-date Microsoft Edge or Google Chrome browser.
The page requests ESPN's public scoreboard, Broncos schedule, and live game
summary APIs directly. ESPN permits browser cross-origin requests, so the
dashboard can be opened as a local file without running a web server.

The page refreshes ESPN every 30 seconds by default while visible, and refreshes
immediately when the tab becomes visible, regains focus, or the network
reconnects. Open the collapsed **Game & audio settings** section to adjust the
refresh interval and recent-play history, choose a game, or enable the screen
wake lock. It remembers
the selected game, favorite team, rooting choice for each matchup, audio mode,
custom rooting-team phrases, voice, speech speed, player-detail display mode,
and screen-awake/audio preferences, refresh interval, recent-play count, and
per-game play history in that browser's local storage, across browser sessions
using the same browser profile and file location. **Game & audio settings**
always starts collapsed.
The tracker retries the saved screen-awake preference at startup. Audio intent
is remembered, but the browser requires a click on **Enable Audio** after each
refresh before sound can actually play. Click **Audio Enabled** to turn audio
off; this also stops speech already in progress. Setting **Audio updates** to
**Off** stops any current play description and prevents further play-by-play
announcements. **Rooting-team phrases** are optional and off by default; they
add a chosen phrase to key-play descriptions when speech is selected.
The **Player details display** setting defaults to **Until next play**; timed
display is also available and remembers its selected duration.

## Team colors and preferences

**Favorite Team** is your all-season favorite and defaults to Denver. It is
separate from **Team You're Rooting For**, which only offers teams in the
selected matchup. The rooting choice defaults to your favorite team when they
are playing; otherwise it defaults to the home team. You can choose either
team, and that choice is remembered for that matchup. The app uses the rooting
team's colors for its accents and dark surfaces. Page, card, control, and panel
backgrounds are derived from the selected team's primary and secondary colors
over a neutral near-black base rather than a fixed navy foundation. Play
gains/losses and network status keep their green/red meaning. During a live drive, each
field end zone uses the primary color of its corresponding home or away team,
regardless of offense direction; labels contrast against the end-zone colors.
If you previously saved a rooting team, it is used to initialize Favorite Team.

## Keep the display awake

Click **Keep screen awake** to request the browser Screen Wake Lock while the
tracker tab is visible. The preference is saved and retried at startup and when
you return to the tab, including after the browser releases the lock. Browsers
can deny the lock or release it when the tab is hidden, the device enters a
power-saving state, or its battery is low. If the browser does not support wake
locks, or the laptop still sleeps, adjust the Windows screen and sleep
settings.

## Audio and Bluetooth

The default audio mode plays original, browser-generated cinematic/arcade-style
stingers for touchdowns, turnovers, made field goals, conversions, and first
downs. It does not include movie dialogue, music, or copied sound effects.
Choose **Spoken play descriptions** or **Speech + key-play effects** to use the
speech voices available on the laptop.

1. Pair the Bluetooth speaker using Windows **Settings → System → Sound**.
2. Select it as the laptop's sound output device.
3. Open the dashboard and click **Enable Audio**. Click **Audio Enabled** to
   turn audio off. Browsers require a user click before playing audio.
4. Choose an audio mode and click **Test selected audio**. When speech and
   rooting-team phrases are enabled, the test uses the first positive phrase.

The browser routes both speech and effects to the current Windows audio output;
the page does not pair with or select Bluetooth devices itself. Speech uses
the browser/operating-system speech synthesis engine and its installed voices.
The audio-enabled preference is saved, but browser autoplay policy prevents
reliable audio activation on refresh. The button and status indicate that
audio still needs a click before it is active. Audio stays local to the laptop
and speaker. Spoken play descriptions omit
abbreviated player initials (for example, `W. Smith` is spoken as “Smith”),
expand team abbreviations to names such as “Denver,” and end with the current
down and distance without a spoken label (for example, “first and ten at
Denver thirty-five”).

Enable **Rooting-team phrases** to add customizable positive or negative
phrases to spoken key plays. Enter one phrase per line; a phrase is selected
randomly when a key play is announced. Use `{team}` for the selected rooting
team or `{play}` to insert the play description. If `{play}` is omitted, the
play description is spoken after the phrase. Positive phrases are used when
the rooting team benefits; negative phrases are used when the opponent does.
The tracker limits callouts to key plays for which it can identify a benefiting
team. Audio effects remain independent of these spoken callouts.

## Game selection and status

The selector lists the in-progress and upcoming games in ESPN's current NFL
week. The favorite team's matchup appears first when it has not yet finished;
the remaining games are ordered by kickoff time. Selecting an upcoming game
lets you prepare its matchup and rooting-team settings in advance. The
selector includes the current score for games in progress. The dashboard shows
score, quarter and clock, down and distance, and the latest play where ESPN
provides it. When an upcoming game is selected, the scoreboard shows a live
countdown to its scheduled kickoff; it changes to the game status when ESPN
reports that play has started. ESPN's broadcast network appears in the game
selector and beneath the selected game's status when the feed provides it.
The live-game badge sits beside the network and ESPN synchronization
indicators. Each team record appears beside its abbreviation. Network
connectivity and ESPN synchronization status are shown separately.

The chart lays each of the selected number of most recent plays horizontally on
a football field. It keeps recent plays across possession changes, turnovers,
and scores, and saves the play history per game in this browser. Its left and
right end zones stay fixed to the home and away teams and use
their primary colors, regardless of who is in possession. Field coordinates
are oriented to those fixed end zones, while gains and losses follow play
yardage so positive plays remain positive for either team. Yard lines are
marked every ten yards, with discreet five-yard ticks along the sidelines.
When ESPN provides live field position and distance, a gold sideline marker
shows the line to gain for a first down. The most recent play appears at the
top of the chart and older plays follow below. Green segments are gains, red
segments are losses, and
gray segments indicate no gain or unknown yardage. Penalty movement is yellow
for either positive or negative yardage. White dots mark play starts. Score badges show points on the scoring play;
touchdown conversion attempts supplied inside ESPN's touchdown entry are
shown on a separate row. INT and FUM badges identify turnovers. Official
timeouts and other informational events are omitted from the field chart.
Hover a row for its description, or select it by click or tap to show the
description below the field; select it again to dismiss it. The field scales
to the available width without an inner scrollbar, and play rows are taller
for readability.
Field goals, kickoffs, and punts use their ESPN start/end field positions to
show the actual field movement; those paths are dashed and colored for the
special-teams unit. Field-goal endpoints include a high-contrast background,
uprights, and a check or X for the result, while punt and kickoff endpoints
are marked P and K.
Each team shows its score,
record, and remaining timeouts; the quarter, clock, and down/distance are
centered between the teams. The latest-play panel remains beneath the chart.

When ESPN includes player participants for a newly detected play, the latest
play panel shows key-player cards with profile photos, names, positions, teams,
and play roles; the cards appear without a repeated play narrative.
By default, the cards remain visible until the next actual play is detected. A
timed display option is available in **Game & audio settings**; plays without
participant data are skipped. Timeouts, two-minute warnings, period breaks, and
other no-play events are informational and do not clear player cards.

The scoreboard shows down and distance without a text label, at the same size
as the team scores. Select **Latest ESPN news & analysis** in **Game & audio
settings** to show ESPN's available news headlines and descriptions beneath
the player cards. Availability and relevance depend on what ESPN provides for
the selected game.

At large desktop resolutions (1800 pixels wide and above), the dashboard uses
an expanded layout sized for large screens, including 2880 × 1920 displays.

## Limitations

- ESPN's public endpoints and the browser's speech-synthesis support are
  third-party dependencies and may change.
- ESPN's public scoreboard API does not provide a documented push or
  subscription feed. Live game updates therefore use periodic polling, with
  immediate refreshes on tab focus/visibility and network reconnection.
- The browser must remain open and the laptop awake to receive updates.
- Browsers may restrict local storage for local files; if so, game/audio
  preferences may not persist after closing the tab.
- The screen wake lock may be denied by the browser or device power settings;
  the saved preference is retried when the tracker tab is visible.
- Audio permission must be activated again in each browser session because
  browsers require user interaction for playback.
- Audio notifications begin after the initial scoreboard load so opening the
  page does not unexpectedly announce an already-running play.
