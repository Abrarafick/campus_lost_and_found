# Campus Lost & Found — Electron Desktop App

SE236 Lab Final Project

## How to run

1. Install Node.js if you don't have it (https://nodejs.org)
2. Open a terminal inside this folder and run:
   ```
   npm install
   npm start
   ```
3. The app window will open. Register with an email ending in `@diu.edu.bd`, then log in.

## Folder structure

```
campus-lost-found/
├── main.js          -> Main process (Node.js backend: file storage, IPC handlers)
├── preload.js        -> Bridge between main process and renderer (contextBridge)
├── renderer.js        -> Frontend logic (runs inside the HTML page)
├── index.html          -> UI structure (login / register / dashboard)
├── style.css            -> Styling
├── data/
│   ├── users.json        -> Registered users (passwords stored as SHA-256 hash)
│   └── items.json          -> Lost & found item records
└── uploads/                 -> Uploaded item photos are saved here
```

## Key features

- Register / Login restricted to university email (@diu.edu.bd)
- Passwords hashed with Node's built-in `crypto` module (never stored as plain text)
- Post a found item with photo, description, location, and contact info
- Photos saved to disk using Node's built-in `fs` module
- Search items by name / description / location
- Mark an item as "Claimed" once returned to its owner
- Campus Lost & Found Office contact shown as a fallback

## Notes for the report / viva

- **main.js** is the only file that touches the file system — this follows Electron's
  security model (renderer stays sandboxed, `contextIsolation: true`, `nodeIntegration: false`).
- **preload.js** exposes a small, specific set of functions (`window.api.*`) instead of
  giving the renderer full Node access — this is the recommended safe pattern.
- Data is stored as simple JSON files (`users.json`, `items.json`) instead of a real database,
  to keep the project focused on the Electron/Node.js architecture required by the lab.
