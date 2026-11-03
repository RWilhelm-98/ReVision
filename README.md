# Re-Vision

Take a photo of the stuff you want to throw out, and Re-Vision tells you what each item is, what it's made of, and where it goes under your city's rules: recycling, organics, garbage, a depot, or hazardous waste.

The first version supports **Edmonton** and **Calgary**.

> **Status: early development.** The mobile app scaffold is set up with Expo Router. The backend and the image analysis are not built yet.

## How it will work

1. Pick your city.
2. Take a photo, or choose one from your library.
3. The app uploads the photo. The server finds each item, draws a box around it, and works out its material.
4. The server looks up the city's sorting rules and returns one clear answer per item, such as "Recycling cart: empty it, lid on".

```
 Expo app                          Django + DRF server
 Search → Scan → Results  ──photo──▶  background worker
                          ◀─results──    1. detect items + boxes (Google Cloud Vision)
   (polls until done)                    2. identify materials (LLM)
                                         3. apply city rules → where it goes
```

The AI only identifies items and materials. The disposal answer comes from a city rules table that we maintain from each city's official guidance. When the model isn't confident, or no rule matches, the app links to the city's own lookup tool instead of guessing.

## Tech stack

| Part | Tools |
|---|---|
| Mobile app | React Native, Expo (SDK 57), Expo Router, JavaScript |
| Backend (planned) | Django, Django REST Framework, PostgreSQL, Django Tasks |
| Analysis (planned) | Google Cloud Vision for object detection, an LLM for materials |

## Project structure

```
re-vision/      Expo mobile app
  src/app/      screens (file-based routes)
  assets/       app icons and splash screen
server/         Django backend (coming soon)
```

## Getting started

You need [Node.js](https://nodejs.org/) (LTS). To run on a phone or emulator, you also need Android Studio, Xcode, or the Expo tooling for your device.

```bash
git clone https://github.com/RWilhelm-98/ReVision.git
cd ReVision/re-vision
npm install
npx expo start
```

Other commands, run from `re-vision/`:

| Command | What it does |
|---|---|
| `npm run android` | Build and run a development build on Android |
| `npm run ios` | Build and run a development build on iOS (needs macOS) |
| `npm run web` | Run the app in a web browser |
| `npm run lint` | Lint the code |

To add a dependency, use `npx expo install <package>` instead of `npm install`. It picks a version that works with the current Expo SDK.

## Roadmap

- [x] Expo app scaffold with Expo Router
- [ ] End-to-end flow with fake results: pick a photo, upload it, show boxes and items
- [ ] Item detection with Google Cloud Vision
- [ ] Material identification with an LLM
- [ ] Edmonton and Calgary sorting rules
- [ ] Full backend: scans, task queue, image storage
- [ ] Live camera, results screen with bounding boxes
- [ ] Accuracy testing on real photos and real devices
- [ ] Deployment and app store builds

Later, we plan to train our own detection model to replace the paid APIs.
