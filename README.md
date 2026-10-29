# Spin Lab V3 — automatic action review prototype

Open https://puneetvrma.github.io/spin-lab/ in your iPhone browser. In Safari, Share → Add to Home Screen adds a convenient launcher. Your Mac is not required for the hosted version.

## Workflow
1. Import an original bowling clip (MOV/MP4 supported when the browser supports its codec). Use short 5–20 second clips; maximum scan length is 60 seconds.
2. Tap Play to check the clip. Manual touch marking is off by default and cannot intercept playback. Dedicated Play/Pause and seek buttons are provided.
3. Choose bowling arm and camera view, then tap Analyse video. Stay on the page until the scan finishes. No event marking is required.
4. Read the report and select the generated approach, arm-up and follow-through review frames. The skeleton follows the sampled pose during playback.
5. Save notes and report in browser storage. Export a backup; Restore history merges a JSON backup into the current history. Keep original clips in Photos.

## What automatic analysis does
The scan samples the full clip at 12 frames per second, at up to 720 pixels width. MediaPipe Pose Landmarker detects up to three people per sampled frame; a prominence heuristic selects the largest visible body. Tracking can switch people, so always check the skeleton. Tracking coverage is a detection rate, not measurement accuracy or model confidence.

The scanner looks for a visible bowling wrist raised above its shoulder and selects the largest rise relative to torso length. It reports that review phase, its 2D elbow angle, and tracked follow-through frames over the next 1.5 seconds. This heuristic is not confirmed ball release. Shoulder-to-hip angle changes describe image-plane movement, not a balance score. No delivery-quality score, injury judgment or legality assessment is provided. A clip containing multiple deliveries should be trimmed to one delivery.

## What is not implemented
Automatic ball tracking, bounce detection, release detection, 3D trajectory, release speed, landing length, turn and spin RPM. This is not a Fulltrack AI equivalent. Body pose alone cannot produce those measurements. The UI marks all ball analytics unavailable.

Optional manual tools live under a collapsed section. Enable touch marking explicitly to trace visible image points or define an image-plane turn angle. Manual speed uses measured horizontal release-to-bounce distance divided by marked flight time, giving average ground-projected speed rather than release speed. Landing length is a supplied measurement, not a video-derived result. Perspective invalidates image-plane angles as true turn measurements, and exported slow-motion timing can invalidate speed.

## Privacy and dependencies
No clip, frame, notes or report is uploaded by the application code. Files are opened through local blob URLs, decoded by the browser and processed in memory. Keyframe images are temporary and are not persisted. History stores report summaries, manual points and notes in localStorage and may be evicted by iOS; export backups regularly. Changing domain/browser changes the storage location.

Internet is required to load the pinned @mediapipe/tasks-vision 0.10.21 runtime/WASM from jsDelivr and the Google-hosted Pose Landmarker Lite model. Those hosts receive ordinary asset requests. There is no service worker/offline guarantee. The older 0.10.22 URL was invalid and has been replaced with a verified existing version.

## Local development
Run `python3 -m http.server 8765 --bind 127.0.0.1 --directory PATH_TO_APP`, then open http://127.0.0.1:8765. Or use Start.command to allow access from a phone on the same trusted Wi-Fi. Stop the server with Control-C. Browser automatic analysis works locally when its model assets are reachable.

## Verification
Tested locally with the supplied original IMG_0385.MOV: imported 1584×2816 rotated H.264 video, duration 8.27 seconds; playback started; automatic scan completed 100 samples, generated three review images and a saved report; default overlay pointer events were disabled. Reviewed skeleton placement on the bowler. Pure summary checks cover missing detections, zero-length joints, arm-up selection and follow-through windows. Physical iPhone/browser performance and measurement validity remain unverified. The source video and extracted frames are not committed or published.

## V3 phase review
Six sections now describe run-up rhythm (alternating visible knee-lift peaks), flow (relative joint-motion continuity), back-foot and front-foot placement, knee drive and follow-through. These are experimental descriptive heuristics, not validated biomechanical assessments. Every timestamp can be opened in the video.

Foot placement requires the user to confirm a fixed tripod. A three-frame low-ankle-motion heuristic proposes tentative contact times around the arm-up phase, with conventional same-side back foot and opposite-side front foot assumptions. Ground contact is not established, target-line alignment is not calibrated and the scan only samples 12 fps. Knee drive is only reported for a selected side view and visible hip/knee/ankle. It describes projected knee height and flexion, not force or forward propulsion. Moving-camera, occluded and missing-event cases show explicit unavailable states.

For useful review record separate fixed rear and side views, including the full approach and two seconds after delivery. Do not relabel a rear or moving clip as side-on or fixed to obtain metrics.
