# Android APK wrapper notes

A compiled APK is not included because the build environment has no Android SDK/Gradle. After deploying Lifetrakcia to HTTPS, create a Capacitor Android wrapper or use PWABuilder. Configure the wrapper to load the deployed URL and keep the Gemini API key only in Cloudflare Pages Secrets. Do not store the key in Android resources, JavaScript, or the APK.

Recommended: first verify the deployed PWA works in Chrome on Android using **Install app**. A native wrapper is useful only after the website and Cloudflare Function have passed the smoke tests in the main README.
