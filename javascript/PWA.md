## Add to Homescreen

Criteria for the automatic prompt:
- Site is not already installed
- Meets user engagement heuristic
- Meets PWA Criteria:
    - A web app manifest file
    - Served Over HTTPS
    - A service worker with fetch event handler
- Call `prompt()` on `beforeinstallprompt` event	