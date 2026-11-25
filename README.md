# FUTA Find — Lost & Found

A mobile-friendly, installable FUTA lost-and-found website. **There are no student accounts, sign-up screens, or passwords.**

## What it does

- Anyone can browse listings without signing in.
- Anyone can post a Lost or Found item with a WhatsApp number.
- Each listing has a **Contact on WhatsApp** link that opens a chat with the poster.
- Search, category and campus-area filters.
- Optional JPG/PNG/WebP photo (under 2 MB).
- Mark a listing Returned.
- Posters can edit/delete their own listings from the same browser session (guest session).
- Firebase anonymous authentication is used silently in the background only to secure ownership; students never create accounts or enter credentials.

## Important privacy note

The WhatsApp number is stored with the listing so the Contact button can work. The full number is not printed as text on the listing, but anyone who can view the listing can open the WhatsApp link and contact that number. Only use a number you are comfortable sharing with people who view your post. Do not put passwords, verification codes, matriculation numbers, or sensitive details in a listing.

## Preview vs shared live site

Without Firebase configuration, preview mode stores posts in that browser only. Different students/devices will not see each other's posts.

To make it shared, configure Firebase:

1. Create a Firebase project at https://console.firebase.google.com/.
2. Add a Web App and copy its Firebase config into `index.html`, replacing the `PASTE_...` values.
3. In Firebase Authentication, enable **Anonymous** sign-in. No email/password provider is needed.
4. Create a Cloud Firestore database.
5. Enable Firebase Storage if you want photo uploads.
6. Deploy `firestore.rules` and `storage.rules` using Firebase CLI:
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase use --add
   firebase deploy --only firestore:rules,storage
   ```
7. Deploy the files to Vercel or another HTTPS static host.

Do not put Firebase service-account keys or admin secrets in this project.

## Guest-session ownership

Firebase anonymous sign-in creates a private guest identity in the browser. It is not a student account and requires no registration. It lets the site associate a post with the browser that created it, so that browser can edit/delete its own reports. Clearing browser data or switching devices may lose access to manage an earlier post. A future version can add a one-time private management code if cross-device management is needed.

## Before public launch

- Test with at least two different phones/browsers.
- Confirm each can post and see the other's listings.
- Confirm one guest cannot edit or delete another guest's reports.
- Confirm WhatsApp links use the correct number.
- Test image upload limits.
- Publish a short privacy notice and a way to report abusive listings.
- Get appropriate permission before presenting this as an official FUTA service. FUTA Find is an independent student project, not an official university service.

## Included files

- `index.html` — website
- `manifest.webmanifest`, `sw.js`, `favicon.svg` — installable web app assets
- `firestore.rules`, `storage.rules` — starting security rules


## Latest card and delete fixes
- Listing photos are constrained to a fixed-height, cropped image area so they cannot stretch the cards.
- Cards now show a Delete action on reports created by the current browser/anonymous Firebase session.
- Demo sample listings are not treated as user-owned; only locally created reports can be edited, marked returned, or deleted in preview mode.
- Firebase deletion still depends on deploying the included `firestore.rules` and keeping Anonymous Authentication enabled.
- Service worker cache was bumped to v3 so browsers fetch the updated files.

## Latest card and delete fixes
- Listing photos are constrained to a fixed-height image area and cannot stretch the card.
- Cards show Delete on listings created by the current browser/anonymous Firebase session.
- Demo sample listings are not treated as user-owned; only locally created reports can be edited, marked returned, or deleted in preview mode.
- Firebase deletion requires Anonymous Authentication and the included `firestore.rules` to be deployed.
- Service worker cache bumped to v3 so browsers fetch the updated files.


## Production cleanup
This build contains no sample/demo listings. Existing demo records stored in this browser's `futaFindReports` local storage are filtered out on startup. Real student reports are not removed.
