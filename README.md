# Gallop & Friends 🐎

A horse website and learning games made for Ella:
- a horse picture game,
- a German "Vet Maths" game (double-digit multiplication and triple-digit division, with a points and vet-rank system),
- and a just-for-fun Feed Kitchen.

## Run it on this computer

Double-click `index.html`. It opens in your web browser. No internet is needed, except for the fancy font, which simply falls back to a plain font when offline.

Keep these three things together in the same folder, or the photos and sounds will not load:
- `index.html`
- the `horse-images` folder
- the `horse-sounds` folder

## Put it online for free with GitHub Pages

This turns the folder into a real website with a link you can open on any device.

### One-time setup

1. Create a free account at https://github.com
2. Click the **+** in the top right, then **New repository**.
   - Name it something like `ella-horse-game`
   - Choose **Public**
   - Do NOT tick "Add a README file" (this folder already has one)
   - Click **Create repository**
3. On the new repository page, click the link **uploading an existing file**.
4. Drag in EVERYTHING from this folder: `index.html`, `README.md`, and the `horse-images` and `horse-sounds` folders. You can drag whole folders into the upload box, and the structure is kept.
5. Click **Commit changes**.

### Turn the website on

6. In the repository, open **Settings**, then **Pages** in the left menu.
7. Under "Build and deployment", set **Source** to **Deploy from a branch**, choose branch **main** and folder **/ (root)**, then click **Save**.
8. Wait about a minute and refresh. A box shows your link, something like:
   `https://YOUR-USERNAME.github.io/ella-horse-game/`
9. Open that link on any device. Done.

### Changing it later

Edit a file with the pencil icon, or use **Add file** to upload a new version. After you commit, the live website updates within about a minute.

## Good to know

- The site is **public**. Anyone with the link can see it. The content is harmless, but it is not private.
- The **points and vet rank are saved per device and per browser**, not inside these files. Each device keeps its own score, and scores do not sync between devices.
- To set a score by hand: open the game, press **F12**, click the **Console** tab, type `localStorage.setItem('ellaVetPoints', '90')` (any number), then reload. Feed-kitchen plays use `ellaFeedTokens`.
- The two `.md` note files are just the source material used to build the games. They do not affect the website, so you can keep or delete them.

## Later: the git way (optional)

When you are comfortable, you can update the site from your computer with git instead of uploading by hand. After creating the empty repository on GitHub, run these in this folder:

```
git init
git add .
git commit -m "First version of Ella's horse game"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ella-horse-game.git
git push -u origin main
```

The first time you push, GitHub will ask you to sign in and create an access token. After that, updating is just `git add .`, `git commit -m "what changed"`, `git push`.
