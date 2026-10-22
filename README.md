# Java IV — Festivali që përshtatet në çdo ekran

Faqja e programit të një festivali shkencor (të dhëna të sajuara) që lexohet mirë si në telefon ashtu edhe në ekran të gjerë.
Temat: Flexbox, Grid, media queries dhe pamje responsive (kap. 6–7 të librit).

## Çfarë realizova

1. **Gjashtë karta aktivitetesh.** Secila ka kategori, titull, orar (`<time datetime>`) dhe lidhjen „Detajet”. Lidhja çon te seksioni *Detajet e aktiviteteve*, ku aktiviteti i zgjedhur theksohet me `:target`.
2. **Menu fleksibile me Flexbox.** `display: flex` + `flex-wrap: wrap`: në ekran të gjerë menuja rri djathtas titullit, në ekran të ngushtë kalon poshtë dhe lidhjet thyhen në rreshta të rinj.
3. **Grid me media queries (mobile-first):**
   - parazgjedhja: `grid-template-columns: minmax(0, 1fr)` → **1 kolonë** (< 600 px)
   - `@media (min-width: 600px)` → `repeat(2, minmax(0, 1fr))` → **2 kolona** (600–899 px)
   - `@media (min-width: 900px)` → `repeat(3, minmax(0, 1fr))` → **3 kolona** (≥ 900 px)
4. **Teksti i gjatë nuk del jashtë.** `minmax(0, 1fr)` dhe `min-width: 0` lejojnë kolonat të tkurren, kurse `overflow-wrap: anywhere` thyen fjalët shumë të gjata. Asnjë element teksti nuk ka lartësi fikse; kartat në të njëjtin rresht barazohen me Flexbox (`flex: 1`), dhe lidhja „Detajet” shkon në fund me `margin-top: auto`.
5. **Imazh fleksibël lokal.** `assets/festivali.svg` me `max-width: 100%; height: auto`. Atributet `width`/`height` në HTML ruajnë raportin 3:1 dhe e parandalojnë kërcimin e faqes gjatë ngarkimit.
6. **Aksesueshmëria.** Lidhje „Kalo te përmbajtja” (skip link), fokus i dukshëm (`:focus-visible`, outline 3 px), tekst i fshehur për lexuesit e ekranit te çdo „Detajet”, që lidhjet të mos jenë të paqarta.
7. **Seksioni Informacione** përdor Flexbox (`flex: 1 1 220px`) për kutitë e informacionit, si krahasim me Grid-in e programit.

### Struktura

```
JavaIV/
├── index.html
├── style.css
├── README.md
├── assets/
│   └── festivali.svg
└── screenshots/
    ├── 360.png
    ├── 768.png
    └── 1280.png
```

## Hapat e hapjes

1. Shkarko ose klono repository-n dhe hap dosjen `JavaIV/`.
2. Hap `index.html` me dopio-klik në shfletues (Chrome, Firefox ose Edge). Nuk nevojitet server ose instalim.
3. Për testim responsive: `F12` → *Toggle device toolbar* (`Ctrl + Shift + M`) → *Responsive* → shkruaj gjerësinë 360, 768 ose 1280.

## Testet

Mjedisi: Chromium (Playwright, headless), 22.10.2026. Kolonat u numëruan nga `getComputedStyle(grid).gridTemplateColumns`; scroll-i horizontal u kontrollua me `document.documentElement.scrollWidth` kundrejt `clientWidth`.

| # | Hyrja | Rezultati i pritur | Rezultati i marrë | Statusi |
|---|-------|--------------------|-------------------|---------|
| 1 | Gjerësia 360 px | 1 kolonë, pa scroll horizontal | 1 kolonë; scrollWidth 360 = clientWidth 360 | ✅ |
| 2 | Gjerësia 768 px | 2 kolona, pa scroll horizontal | 2 kolona; scrollWidth 768 = clientWidth 768 | ✅ |
| 3 | Gjerësia 1280 px | 3 kolona, pa scroll horizontal | 3 kolona; scrollWidth 1280 = clientWidth 1280 | ✅ |
| 4 | Kufijtë 599 / 600 / 899 / 900 px | 1 / 2 / 2 / 3 kolona | 1 / 2 / 2 / 3 kolona; pa scroll horizontal në asnjërën | ✅ |
| 5 | `Tab` nga fillimi i faqes (360 px) | Fokusi kalon me radhë nëpër menu, me kornizë të dukshme | Kalo te përmbajtja → Programi → Detajet → Informacione → Kontakti → Detajet (karta 1); secila me outline 3 px | ✅ |
| 6 | `Tab` ×3 + `Enter` (lidhja „Detajet” në menu) | Faqja shkon te seksioni i detajeve | URL bëhet `#detajet` | ✅ |
| 7 | Klik „Detajet” te karta 3 | Hapet dhe theksohet detaji përkatës | `:target` = `#d3`, korniza blu shfaqet | ✅ |
| 8 | Titull me 100 karaktere (karta 3) | Thyhet në disa rreshta, nuk del jashtë kartës | 4 rreshta (360), 4 (768), 3 (1280); 0 elemente me overflow në kartë | ✅ |
| 9 | Imazhi në 360 / 768 / 1280 px | Shkallëzohet pa u shtrembëruar (raporti 3:1) | 328×109 / 736×245 / 1200×400 px | ✅ |

Pamjet e ekranit: [`screenshots/360.png`](screenshots/360.png), [`screenshots/768.png`](screenshots/768.png), [`screenshots/1280.png`](screenshots/1280.png).

## Rastet e pranimit

- [x] Në 360/768/1280 px shfaqen përkatësisht 1/2/3 kolona.
- [x] Nuk ka scroll horizontal në faqen e plotë.
- [x] Menuja mbetet e përdorshme me tastierë.

## Sfida e transferimit

Karta 3 ka titullin me saktësisht 100 karaktere: *„Nga laboratori në kuzhinë: eksperimente të thjeshta kimike që mund t'i provoni në shtëpi me familjen”*. Ai përshtatet sepse:

1. kolonat e Grid-it janë `minmax(0, 1fr)`, jo `1fr`, kështu që përmbajtja nuk mund ta zgjerojë kolonën;
2. titulli ka `overflow-wrap: anywhere`, kështu që edhe një fjalë shumë e gjatë thyhet;
3. karta nuk ka lartësi fikse: rritet sa duhet, dhe kartat e tjera në rresht barazohen me të.

## Reflektim individual

**Kur do të zgjidhja Flexbox dhe kur Grid?**

Flexbox e zgjedh kur elementet rreshtohen në **një drejtim** (rresht ose kolonë) dhe dua që madhësia e tyre të vendoset nga përmbajtja, p.sh. menuja: nuk më intereson sa lidhje bien në secilin rresht, vetëm që të thyhen kur nuk ka vend. E përdora edhe brenda kartës, për ta shtyrë lidhjen „Detajet” në fund me `margin-top: auto`.

Grid e zgjedh kur dua të kontrolloj **dy drejtime njëherësh** (rreshta dhe kolona) dhe paraqitja vendoset nga kontejneri, jo nga përmbajtja. Te programi doja saktësisht 1, 2 ose 3 kolona me gjerësi të barabartë, pavarësisht nëse një titull është i shkurtër apo 100 karaktere. Me Flexbox kjo do të kërkonte llogaritje me `flex-basis` dhe kartat e rreshtit të fundit do të zgjeroheshin ndryshe nga të tjerat.

Shkurt: Flexbox për komponentë dhe rreshta të thjeshtë, Grid për paraqitjen e faqes ose rrjeta kartash.

## Deklarimi i AI dhe burimeve

- **AI:** Gjatë kësaj detyre u përdor **Claude (Anthropic)** për te ndihmuar ne shkrimin  e kodit HTML/CSS, ilustrimit SVG, testeve automatike (Playwright) dhe draftit të këtij README-je. Kodin e kam lexuar, testuar dhe kuptuar.

