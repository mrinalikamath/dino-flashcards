# Dino Flashcards

An interactive dinosaur flashcard app for kids. Flip through 20 illustrated dinosaur cards, each with a fun fact on the back.

## Features

- 20 dinosaur cards with illustrated images
- Tap/click to flip a card and reveal the fact
- Swipe or use arrow buttons to navigate between cards
- Animated card stack with 3D flip effect
- Responsive — works on mobile and desktop

## Stack

- Single-file HTML/CSS/JS — no build tools, no dependencies
- CSS 3D card flip (`transform-style: preserve-3d`, `rotateY`)
- `object-fit: cover` + per-card `object-position` for cropping portrait cards from landscape images
- Google Fonts (Inter)

## Running locally

```bash
python3 -m http.server 8082 --directory /path/to/dino-flashcards
```

Then open [http://localhost:8082](http://localhost:8082).

## Project structure

```
dino-flashcards/
├── index.html        # Entire app (HTML + CSS + JS inline)
├── images/           # Dinosaur illustration images
│   ├── trex.jpg
│   ├── stegosaurus.png
│   └── ... (20 total)
├── README.md
└── design.md
```

## Image sources

Dinosaur illustrations sourced from [Active Wild](https://www.activewild.com/list-of-dinosaurs-names-with-pictures/). PNG images with transparent backgrounds were processed with Python Pillow to remove white backgrounds.
