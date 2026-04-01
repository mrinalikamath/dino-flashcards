# Design Document — Dino Flashcards

## Visual Design

### Color Palette
- Background: `#e4e0d8` (warm off-white / parchment)
- Card front: warm beige gradient with subtle texture feel
- Card back: contrasting color per card (varies by dinosaur type)
- Text: dark for readability, icon emoji as color accent

### Typography
- Font: Inter (Google Fonts)
- Card name: bold, large
- Fun fact: regular weight, smaller, readable at card size

### Card Dimensions
- Aspect ratio: 320 × 500 (portrait)
- Responsive: `min(320px, 100vw - 48px, (100svh - 80px) * 320/500)`
- Ensures card fits on any screen without scrolling

---

## Card Flip Animation

CSS 3D flip on click:
- `transform-style: preserve-3d` on `.card`
- `.card-front` and `.card-back` use `backface-visibility: hidden`
- Flip: `rotateY(180deg)` toggled by `.is-flipped` class
- Duration: ~500ms ease

---

## Card Stack Animation

4 slots (A/B/C/D) are reused in a ring buffer. Each slot has a `data-pos` attribute (0–3) mapped to visual depth in the stack:

| Position | Visual role         | Transform                     | Opacity | z-index |
|----------|---------------------|-------------------------------|---------|---------|
| 0        | Front (active)      | `translateY(0) scale(1)`      | 1       | 4       |
| 1        | Second card peek    | `translateY(18px) scale(0.95)`| 0.7     | 3       |
| 2        | Third card peek     | `translateY(32px) scale(0.90)`| 0.4     | 2       |
| 3        | Hidden (preloading) | `translateY(46px) scale(0.85)`| 0       | 1       |

On navigation, slots rotate positions with CSS transition. The hidden slot (pos 3) loads the next card's image before it animates into view, preventing flicker.

---

## Image Cropping Strategy

Images are landscape illustrations cropped to portrait card shape using:

```css
.animal-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

Each card has a per-dinosaur positional class to ensure the face/head is visible in the crop:

| Class        | `object-position`    | Use case                        |
|--------------|----------------------|---------------------------------|
| `pos-left`   | `left center`        | Subject on left of image        |
| `pos-right`  | `right center`       | Subject on right of image       |
| `pos-top`    | `center top`         | Full-body image, head at top    |
| `pos-bottom` | `center bottom`      | Head near bottom of frame       |
| *(none)*     | `center center`      | Subject centered                |

---

## Image Types

Two categories of images are used:

**Transparent PNG** — isolated dinosaur renders with white backgrounds removed via Python Pillow. These float cleanly on the card background:
- stegosaurus, velociraptor, spinosaurus, pteranodon, parasaurolophus, archaeopteryx, deinonychus, allosaurus, microraptor

**Scene JPG** — full illustrations with environmental backgrounds (sky, jungle, etc.). Kept as-is with natural backgrounds:
- trex, triceratops, diplodocus, ankylosaurus, pachycephalosaurus, therizinosaurus, carnotaurus, mosasaurus, iguanodon, oviraptor, dilophosaurus

Source: [Active Wild](https://www.activewild.com/list-of-dinosaurs-names-with-pictures/)

---

## Dinosaur List

| # | Name               | Era        | Type        | Icon |
|---|--------------------|------------|-------------|------|
| 1 | T. Rex             | Cretaceous | Theropod    | 🦖   |
| 2 | Triceratops        | Cretaceous | Ceratopsian | 🦕   |
| 3 | Stegosaurus        | Jurassic   | Thyreophora | 🛡️   |
| 4 | Diplodocus         | Jurassic   | Sauropod    | 🦕   |
| 5 | Velociraptor       | Cretaceous | Theropod    | 🦖   |
| 6 | Spinosaurus        | Cretaceous | Theropod    | 🦖   |
| 7 | Ankylosaurus       | Cretaceous | Thyreophora | 🛡️   |
| 8 | Pteranodon         | Cretaceous | Pterosaur   | 🦅   |
| 9 | Pachycephalosaurus | Cretaceous | Marginoceph | 💀   |
|10 | Parasaurolophus    | Cretaceous | Ornithopod  | 📯   |
|11 | Therizinosaurus    | Cretaceous | Theropod    | ✂️   |
|12 | Carnotaurus        | Cretaceous | Theropod    | 🐂   |
|13 | Allosaurus         | Jurassic   | Theropod    | 🦖   |
|14 | Archaeopteryx      | Jurassic   | Avialae     | 🐦   |
|15 | Microraptor        | Cretaceous | Theropod    | 🪶   |
|16 | Deinonychus        | Cretaceous | Theropod    | 🦖   |
|17 | Mosasaurus         | Cretaceous | Marine      | 🌊   |
|18 | Iguanodon          | Cretaceous | Ornithopod  | 🦎   |
|19 | Oviraptor          | Cretaceous | Theropod    | 🥚   |
|20 | Dilophosaurus      | Jurassic   | Theropod    | 🎭   |
