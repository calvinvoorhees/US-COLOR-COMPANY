US COLOR COMPANY site fixes (2026-10-14)

How to apply
1. Copy every file in this folder into the repo root, replacing the existing
   index.html, about.html, contact.html and poster images.
2. Delete newshoes.png from the repo (it is replaced by newshoes.jpg).
3. Commit and push. GitHub Pages redeploys on push to main.

What changed, index.html
- Posters recompressed to JPEG quality 72, at most 1920 px wide, never
  upscaled: 15.7 MB down to 2.2 MB. This was the slow first load.
- The looping feed is back, done safely: scrolling near the bottom appends
  another lap of the eight projects, but the feed is capped at 24 rows. Rows
  that scroll well out of view are removed and their Vimeo players destroyed,
  and the view stays on the same row while that happens. (Before, rows grew
  8, 16, 24, 32... forever, each able to start its own player.)
- The preview embed no longer forces 1080p.
- iPhone/iPad in Low Power Mode: iOS refuses the page's tap-to-play because
  the play happens after the tap, inside Vimeo's iframe. The frame now
  switches to a Vimeo embed with controls and lets the tap through, so
  Vimeo's own play button starts the video; after that, normal tap to
  pause/resume works for that frame. This was the "first video never plays"
  report; in Low Power Mode every frame is refused until it gets an
  in-player tap.
- Starting a video now pauses the other players. On a phone, tapping a
  second and third frame used to leave all of them decoding at once.
- The fullscreen handler could never find its player (it read data-index
  from the frame, which only the row had), so entering fullscreen did not
  play or unmute and leaving did not mute or pause. Fixed.
- A desktop click before any hover opened fullscreen on a static poster and
  a second click threw. The click now loads and plays the video first. The
  50 ms delay before a frame's listeners were attached is gone.
- The invisible FULLSCREEN button on phones no longer catches taps while the
  overlay is hidden.
- The Vimeo SDK script is deferred so it no longer blocks first paint.
- The page scroller uses 100dvh on iOS so the bottom of the feed is not
  hidden behind Safari's toolbar.
- Leftover touch-active class toggling removed; bare play/pause/setMuted
  calls now swallow rejections like the other call sites.

What changed, all three pages
- Pinch zoom is allowed again (maximum-scale and user-scalable removed).
- The nav hover style on index and about applies only to pointers that can
  hover, so a tap on a phone no longer leaves a nav pill stuck black.

Not changed, worth a look later
- about.html still ships lorem ipsum as its statement and body copy.
- ButWhatIf.jpeg, abandominium.jpg and slasher.jpg are not referenced by any
  page but still deploy.
- The header's backdrop blur sits over playing video; the background is 95%
  opaque so dropping the blur would be nearly invisible and cheaper on phones.
- Rows are 100vw wide inside a scroller; on Windows with classic scrollbars
  that is about 15 px too wide and shifts the frames slightly. width: 100%
  would fix it.

Verified
- Feed loops and holds at 24 rows with no leftover players after ten scrolls
  to the bottom, on desktop and phone layouts.
- Hover autoplay works in Safari, Safari Technology Preview and Chrome under
  Safari's default Auto-Play setting.
- Phone (iPhone 17 Pro Max, iOS 27.2): tap to play works; in Low Power Mode
  the first tap is refused, Vimeo controls appear, the play button starts it,
  later taps pause and resume. Tapping a second frame pauses the first.
- Desktop click with no prior hover plays the video.

Known limits (browser rules, not site bugs)
- A Mac with Safari set to "Never Auto-Play", or in Low Power Mode, cannot
  start hover previews; a click still opens the video fullscreen with sound.
