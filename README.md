# SmartDrive Web Shell

Modern single-page web app that gives you:

- **Login interface** with Google / Apple sign-in buttons (wired as placeholders so you can connect your own auth).
- **Home page** with clean, ready-made containers for your own content.
- **Monitoring page** that uses the **Google Maps JavaScript API**:
  - Captures **user location** (with browser geolocation, when allowed).
  - Lets users **enter a destination** and shows a **driving route** on the map.
  - Splits the screen into a **top map** and a **bottom panel** where you can add any content you like.

Everything is contained in a single `index.html` using React loaded from a CDN, so there is **no build step required**.

---

## 1. Getting started

1. **Install Node (optional but recommended)**  
   Node is only needed if you want to run a small dev server. The app also works by simply opening `index.html` in a browser.

2. **Clone or open this folder**  
   The entry file is `index.html` at the project root.

3. **Run a simple static server (optional)**  
   From the `smartDrive` folder:

   ```bash
   npx serve .
   ```

   Then open the printed URL (usually `http://localhost:3000`) in your browser.

4. **Or just open the file directly**  
   Double-click `index.html` or drag it into Chrome, Edge, or Safari.

---

## 2. Configure Google Maps

The monitoring page uses the **Google Maps JavaScript API** with directions.

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create or select a project.
3. Enable:
   - **Maps JavaScript API**
   - **Directions API**
4. Create an **API key** (and restrict it to your domains in production).
5. In `index.html`, update the script tag:

   ```html
   <script
     id="google-maps-script"
     src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_MAPS_API_KEY&libraries=places&callback=initSmartDriveMap"
     async
     defer
   ></script>
   ```

   Replace `YOUR_GOOGLE_MAPS_API_KEY` with your real key.

Once set, open the Monitoring tab, allow browser location, type a destination (e.g. `"Jomo Kenyatta International Airport"`), and you should see:

- Your location (or a fallback center) on the map.
- A driving route and summary with distance + ETA.

---

## 3. Login (Google / Apple)

The login screen already shows **"Continue with Google"** and **"Continue with Apple"** buttons.  
Right now, they are **mock buttons** that just simulate a successful login in the UI.

To connect real authentication:

- **Google**  
  Use **Google Identity Services**:
  - Docs: <https://developers.google.com/identity/gsi/web>
  - Replace the mock `onMockLogin("Google")` handler with the real callback that you get from Google.

- **Apple**  
  Use **Sign in with Apple JS**:
  - Docs: <https://developer.apple.com/documentation/sign_in_with_apple>
  - Replace `onMockLogin("Apple")` with the response handler from Apple.

You can keep the rest of the UI as-is; only the button click handlers need to change once you have real credentials.

---

## 4. Where to plug in your content

### Home page

On the Home tab you have:

- A large **primary content card** – ideal for dashboards, charts, or any main content.
- Two smaller cards on the right, where you can put:
  - Stats, quick metrics, short tables.
  - Notifications or any sidebar content.

Every card is a styled container; just replace the placeholder text in the `HomePage` React component.

### Monitoring page

The Monitoring tab is split into:

- **Top half**: the **map** and a search bar where the user types a destination.
- **Bottom half**:
  - Left card: shows **route summary**, but you can replace it with anything.
  - Right card: labeled **"Your lower panel canvas"** — this is the exact area you mentioned where you want to add your own content.

Inside `MonitorPage`, search for the comment about ideas (trip timeline, diagnostics, etc.) and replace that block with whatever UI you want.

---

## 5. Tech choices

- **React 18** and **ReactDOM 18** from a CDN (no bundler).
- **Babel Standalone** in the browser to support JSX.
- Pure **HTML + CSS** for layout and styling.
- **Browser geolocation API** to get the user’s location (if they allow it).
- **Google Maps JavaScript API** for maps and driving directions.

This gives you a clean, modern UI that you can extend without having to configure a full build pipeline immediately.

