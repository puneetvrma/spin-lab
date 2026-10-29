# Spin Lab — local iPhone prototype

## Use on iPhone via GitHub Pages
Open the deployed GitHub Pages URL in Safari, then tap Share → Add to Home Screen for a convenient launcher. The hosted app runs without your Mac. Videos and saved delivery records remain in your browser, and are not uploaded to the repository. Pose model assets require internet. This is a testing prototype with guided ball measurements and single-frame pose analysis, not automatic full-delivery analysis.

## Run locally
1. On your Mac, double-click `Start.command`. Requires Python 3. If macOS blocks the downloaded launcher, open Terminal, type `python3 -m http.server 8765 --bind 0.0.0.0 --directory `, drag the spin-lab folder into Terminal, and press Return.
2. Keep Mac and iPhone on the same trusted Wi-Fi. Find the Mac's local IP at System Settings → Wi-Fi → Details → TCP/IP.
3. On your iPhone open `http://MAC-IP:8765` in Safari. Allow the Mac's incoming connection if prompted. Keep the Mac running. Stop the server with Control-C when finished. This server shares this app folder with devices on the same network.
4. Tap Record a delivery. The iPhone camera opens through a file input; finish recording and choose Use Video. Or import a clip from Photos. This approach does not require the HTTPS live-camera API. iOS controls camera settings; test the capture workflow on your phone.

This is a browser app served from your Mac, not a standalone installed native iOS app. It has no account or paid service. Safari home-screen installation and offline launch are not implemented. Record with the ordinary Camera app whenever convenient and analyse later.

## Useful workflow
Film short single-delivery clips. Place the phone safely outside the ball's path. Keep the whole body visible from side-on for action review. Move behind the bowler for line and bounce review. Use a fixed tripod, good light and the original real-time clip for timing. Imported slow motion may use an altered timeline; do not calculate speed from that timeline.

Pause at release, bounce and finish; mark each event. Enter the actual measured horizontal release-to-bounce distance for estimated average ground speed. Enter measured bounce-to-batter-stumps distance for length. This does not automatically locate the bounce or derive its distance from video.

Pause and tap the visible ball at multiple times to trace its image path. Tapping the bottom video-controls strip is ignored. Scale with a known reference in the same plane as the trace; airborne movement generally does not stay in that plane. The resulting 2D traced distance is only a projected approximation. Image-plane turn uses three ground-path points and is strongly distorted by perspective. From a rear view, use this qualitatively; it is not a validated true turn measurement. No automatic ball tracking, 3D trajectory, release speed or spin RPM.

The optional paused-frame pose analysis downloads a pinned MediaPipe runtime and model from jsDelivr/Google. Internet is required for loading those assets. Processing occurs locally; the app code does not upload clips. External asset hosts receive normal asset requests. It reports visible 2D joint angles with a visibility threshold, not a coaching score. Whole-action automatic assessment and follow-through scoring are not implemented. Use event markers, video playback and the observation checklist to review those aspects.

Save one record per delivery. Notes and measurement data use localStorage in this browser; original videos are not stored. Export JSON regularly. No JSON import/restore is currently provided. Storage is specific to the server address and browser and can be evicted by iOS.

## Validation status
Prototype; no physical iPhone or real bowling footage validation has been performed. Camera capture, model compatibility, frame seeking and measurement accuracy require testing on your iPhone. Do not use it to assess bowling legality or as medical advice.
