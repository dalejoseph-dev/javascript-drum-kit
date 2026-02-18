# JavaScript Drum Kit

A browser-based drum kit that plays sounds when you press keyboard keys. Built with vanilla HTML, CSS, and JavaScript — no frameworks or libraries required.

---

## 🥁 How It Works

Each key on the keyboard (`A`, `S`, `D`, `F`, `G`, `H`, `J`, `K`, `L`) is mapped to a drum sound. Pressing a key triggers the corresponding audio and a visual animation on the matching key card.

---

## 🛠️ How I Built It

### 1. HTML Structure (`index.html`)

The UI consists of a row of `.key` `<div>` elements, each with a `data-key` attribute set to the **keyCode** of its corresponding keyboard key (e.g., `65` for `A`).

Each key card displays:
- The keyboard letter inside a `<kbd>` tag
- The sound name in a `<span class="sound">`

A matching set of `<audio>` elements sit in the HTML, each with a `data-key` attribute that mirrors the `.key` divs. This is how the script connects a keypress to a sound file.

```html
<div data-key="65" class="key">
  <kbd>A</kbd>
  <span class="sound">clap</span>
</div>
<audio data-key="65" src="sounds/clap.wav"></audio>
```

---

### 2. JavaScript Logic (`script.js`)

The script handles two things: **playing audio** and **managing the CSS animation**.

**Playing a sound (`playSound`)**

When a `keydown` event fires on the window, the handler:
1. Finds the `<audio>` element whose `data-key` matches `e.keyCode`
2. Resets `currentTime` to `0` so rapid keypresses retrigger the sound from the start
3. Calls `.play()` on the audio element
4. Adds the `playing` CSS class to the matching `.key` div

```js
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if (!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
}
```

**Removing the animation (`removeTransition`)**

Rather than using a `setTimeout` to remove the `playing` class, the script listens for the `transitionend` event on each `.key` element. When the CSS transition finishes, the class is removed automatically — keeping the animation in sync with the CSS without any hardcoded timing.

```js
function removeTransition(e) {
  if (e.propertyName !== 'transform') return;
  this.classList.remove('playing');
}
```

---

### 3. Styling (`styles.css`)

- The keys are laid out in a centered row using **Flexbox**
- Each `.key` has a short `transition: all .07s ease` applied
- The `.playing` class scales the key up (`transform: scale(1.1)`) and adds a **yellow glow** (`box-shadow`) to give tactile visual feedback
- A background image fills the page using `background-size: cover`

---

## 📁 Project Structure

```
/
├── index.html       # Markup and audio elements
├── script.js        # Keydown listener and sound logic
├── styles.css       # Layout and key animations
├── background.jpg   # Full-page background image
└── sounds/          # WAV audio files for each drum sound
    ├── clap.wav
    ├── hihat.wav
    ├── kick.wav
    ├── openhat.wav
    ├── boom.wav
    ├── ride.wav
    ├── snare.wav
    ├── tom.wav
    └── tink.wav
```

---

## 🚀 Running the App

Simply open `index.html` in a browser — no build step or server required. For audio to work properly, serve the files through a local server (e.g., VS Code Live Server or `npx serve .`) since some browsers block audio on `file://` URLs.

---

## 💡 Key Concepts Used

- `keydown` event listener on `window`
- `data-*` attributes to link DOM elements to audio
- `transitionend` event to clean up CSS classes
- CSS `transform` and `box-shadow` for tactile key animations