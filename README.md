# Java V — Formulari i misionit sekret

Skedarët: `index.html` (formulari + tabela), `pranuar.html` (faqja e demonstrimit), `style.css`.

## Para kodimit

**Hyrjet:** pseudonimi (tekst 3–20), email (format email-i), persona (numër 1–5),
kategoria (Art / Shkencë), pranimi i rregullave (checkbox).

**Daljet:** nëse ka gabim, shfletuesi ndalon dërgimin dhe tregon mesazh te fusha e parë
e gabuar; nëse gjithçka është valide, hapet `pranuar.html?pseudonimi=...&email=...`.

**Rast normal:** `Hija`, `hija@example.org`, `3`, Shkencë, rregullat ✔ → kalon te pranuar.html.

**Raste kufitare:**
- Pseudonim me 2 karaktere (`Ab`) → nuk kalon; me 3 (`Abc`) ose 20 karaktere → kalon.
- Persona `0` ose `6` → nuk kalon; `1` dhe `5` → kalojnë.
- Email `hija.example.org` (pa @) → nuk kalon.

## Rastet e pranimit

- [x] Formulari bosh nuk kalon (`required` te të gjitha fushat, radio dhe checkbox).
- [x] Email pa format email-i dhe grup 0 ose 6 nuk kalojnë (`type="email"`, `min="1" max="5"`).
- [x] Të dhënat valide çojnë te `pranuar.html`; tabela ka `caption`, `th scope="col"` dhe `th scope="row"`.

## Shënim për GET

Formulari përdor `method="get"`, prandaj vlerat shfaqen në URL dhe ruhen në historikun
e shfletuesit. `pranuar.html` është faqe statike: **nuk ka server dhe asgjë nuk ruhet**.
Për këtë arsye përdoren vetëm të dhëna të sajuara.

## Reflektim individual

`required` dhe atributet e tjera kontrollohen vetëm në shfletuesin e përdoruesit, i cili
është nën kontrollin e tij. Ai mund t'i heqë atributet me DevTools, të çaktivizojë JavaScript-in,
ose të dërgojë kërkesë direkt te serveri (p.sh. me `curl` ose duke shkruar URL-në me dorë)
pa kaluar fare nga formulari. Validimi në frontend shërben për përvojë më të mirë
(gabimet shihen menjëherë), por nuk është mbrojtje. Në një sistem real serveri duhet t'i
kontrollojë përsëri të gjitha të dhënat dhe t'i trajtojë si të pabesueshme.
