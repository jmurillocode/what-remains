# jcode.art — Collect Marketplace · Context Handoff

> Paste or attach this file at the start of a new conversation so the assistant can pick up without re-learning everything.

---

## 1. Who & What

- **Artist**: Jonathan Murillo ("JCode" / jmurillo.code@gmail.com)
- **Practice**: Generative art made with p5.js + AI (custom LORA models). Sells work as NFTs.
- **Contracts**: Deployed via **Transient Labs** on **Ethereum mainnet**.
- **Website**: `jcode.art`, deployed on **Vercel**.
- **Working folder**: `jcode-website/` (selected folder: Art Projects)

The site is a static HTML/CSS/JS portfolio. No build step. No framework. Clean, intentional.

---

## 2. Current State (as of 2026-04-17)

Phase 2a is complete for **Proof of Power** and **Fracture**. Both pages are live on the site with real contract data (read-only), secondary market listings via Reservoir, and a lightbox viewer. **What Remains** is still a placeholder — the contract hasn't been deployed yet.

### Phase history

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Static front-end, placeholder data, mock collect/collected states | Done (replaced) |
| Phase 2a | Live Reservoir API data, pre-seeded owner addresses, lightbox viewer | Done — PoP + Fracture |
| Phase 2b | Wallet connect + on-chain `ownerOf()` live reads | Not started |
| Phase 2c | Primary mint wiring (write transactions) | Not started — What Remains only |
| Phase 3 | Secondary market: list / make offer UI | Not started |

---

## 3. Files — Current State

```
jcode-website/
├── index.html                         ← home (has Collect nav link)
├── artworks-web/                      ← day1.jpg … day47.jpg (What Remains editorial images)
├── what-remains/
│   └── index.html                     ← editorial page for What Remains (unchanged)
├── proof-of-power/
│   ├── index.html                     ← editorial page (unchanged)
│   └── artworks-web/                  ← founder.jpg, architect.jpg, spectacle.jpg …
├── fracture/
│   ├── index.html                     ← editorial page (unchanged)
│   └── artworks-web/                  ← i-tried-to-be-seen.jpg, this-box-is-not-me.jpg …
├── collect/
│   ├── index.html                     ← projects grid: PoP (live), Fracture (live), What Remains (coming soon)
│   ├── proof-of-power/
│   │   └── index.html                 ← Phase 2a — 8 works, Reservoir data, lightbox
│   ├── fracture/
│   │   └── index.html                 ← Phase 2a — 15 works, Reservoir data, lightbox
│   └── what-remains/
│       └── index.html                 ← OLD Phase 1 placeholder — do not show, not linked
└── COLLECT_CONTEXT.md                 ← this file
```

**`collect/index.html` grid state:**
- What Remains → `coming-soon` card (no link, "Coming soon" badge, "47 works · minting soon")
- Proof of Power → active link to `proof-of-power/index.html`, "Sold Out" badge, 0/8 available
- Fracture → active link to `fracture/index.html`, "Sold Out" badge, 0/15 available
- Future Series slot → **removed entirely**
- Section meta: "2 live · 1 coming"

---

## 4. Proof of Power — `collect/proof-of-power/index.html`

### Contract
```
0x7c1f95c487D4ee32fa25C6632e79F5eC75ae5594   (Transient Labs, Ethereum mainnet)
```
8 works — all sold out.

### Architecture (Phase 2a)
- **On-chain reads**: `tokenURI(uint256)` selector `0xc87b56dd`, `ownerOf(uint256)` selector `0x6352211e`, called via `mainnet.gateway.tenderly.co`
- **IPFS metadata**: resolved via `w3s.link` gateway
- **Secondary listings**: Reservoir API `https://api.reservoir.tools/tokens/v7?collection=CONTRACT&limit=20&sortBy=tokenId`, `x-api-key: demo`, CORS-compatible, runs client-side at runtime
- **Owner addresses**: pre-seeded in the `PORTRAITS` array (on-chain reads 2026-04-17) — to be replaced by live `ownerOf()` calls in Phase 2b

### PORTRAITS array (token ID order, not editorial order)
```js
const CONTRACT = '0x7c1f95c487D4ee32fa25C6632e79F5eC75ae5594';
const PORTRAITS = [
  { id: 'architect', tokenId: 1, num: 1, title: 'The Architect',  owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'founder',   tokenId: 2, num: 2, title: 'The Founder',    owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'spectacle', tokenId: 3, num: 3, title: 'The Spectacle',  owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'zealot',    tokenId: 4, num: 4, title: 'The Zealot',     owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'profet',    tokenId: 5, num: 5, title: 'The Prophet',    owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'pilgrim',   tokenId: 6, num: 6, title: 'The Pilgrim',    owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'oracle',    tokenId: 7, num: 7, title: 'The Oracle',     owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
  { id: 'trickster', tokenId: 8, num: 8, title: 'The Trickster',  owner: '0x6f398e7872ac5f75121186678c4e6e015e83c49f', collected: true },
];
```

### UI
- 4-column grid, `4:3` aspect ratio images, full colour (no grayscale)
- Collect button greyed out and reads "Collected" for all works
- Filter tabs: All / Listed / Collected
- Lightbox on card click: 2-column modal (image left, details right), ESC + arrow key navigation
- Lightbox details panel: eyebrow "Archetype N of 8", italic Lora title, gold divider, quote, Owner (truncated address), Secondary (price or "No active listing"), Chain (Ethereum Mainnet)

---

## 5. Fracture — `collect/fracture/index.html`

### Contract
```
0x3306b22abc51d1b15df8681e13f0dbfa2c3f6758   (Transient Labs, Ethereum mainnet)
```
15 works — all sold out. Token IDs 2–16 (token 1 is empty/burned).

### WORKS array (token ID order)
```js
const CONTRACT = '0x3306b22abc51d1b15df8681e13f0dbfa2c3f6758';
const WORKS = [
  { id: 'this-box-is-not-me',          tokenId: 2,  num: 1,  title: 'This Box Is Not Me',          owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'there-was-a-center-once',     tokenId: 3,  num: 2,  title: 'There Was a Center Once',     owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'i-tried-to-be-seen',          tokenId: 4,  num: 3,  title: 'I Tried to Be Seen',          owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'the-parts-that-didnt-fit',    tokenId: 5,  num: 4,  title: "The Parts That Didn't Fit",   owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'the-version-they-wanted',     tokenId: 6,  num: 5,  title: 'The Version They Wanted',     owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'signal-with-no-receiver',     tokenId: 7,  num: 6,  title: 'Signal With No Receiver',     owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'the-mask-became-the-face',    tokenId: 8,  num: 7,  title: 'The Mask Became the Face',    owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'i-was-here-briefly',          tokenId: 9,  num: 8,  title: 'I Was Here, Briefly',         owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'still-searching-still-split', tokenId: 10, num: 9,  title: 'Still Searching, Still Split',owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'before-the-mirror-lied',      tokenId: 11, num: 10, title: 'Before the Mirror Lied',      owner: '0x2fb24b1021e51fa23db91a9b995000181fda0963' },
  { id: 'what-the-algorithm-made',     tokenId: 12, num: 11, title: 'What the Algorithm Made',     owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'a-face-no-one-asked-for',     tokenId: 13, num: 12, title: 'A Face No One Asked For',     owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'the-self-that-stayed-online', tokenId: 14, num: 13, title: 'The Self That Stayed Online', owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'all-that-noise-was-me',       tokenId: 15, num: 14, title: 'All That Noise Was Me',       owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
  { id: 'everything-meant-something',  tokenId: 16, num: 15, title: 'Everything Meant Something',  owner: '0xcf88fa6ee6d111b04be9b06ef6fad6bd6691b88c' },
];
```

### UI differences from Proof of Power
- 3-column grid (vs 4-column)
- `1:1` aspect ratio with `object-fit: contain; padding: 8px` (generative art, not portrait photos)
- No per-artwork quotes (lightbox shows title, owner, secondary price, series label, chain only)
- Lightbox eyebrow reads "Work N of 15" (not "Archetype")
- Image path: `../../fracture/artworks-web/${w.id}.jpg`

---

## 6. What Remains — `collect/what-remains/index.html`

**Not linked anywhere.** The old Phase 1 placeholder file still exists at this path but is not shown on the collect grid.

**Collect grid card**: shows as `coming-soon` — greyed out, "Coming soon" badge, no link.

When the contract is deployed, the plan is:
1. Deploy contract on Transient Labs (47 works, 1/1 per day)
2. Build Phase 2a page (same architecture as PoP/Fracture)
3. Activate the collect grid card (change from `coming-soon` to `available`, add link)

---

## 7. Design System

| Token | Value |
|---|---|
| Background | `#080808` |
| Gold accent | `#b89a56` |
| Gold dim | `rgba(184,154,86,0.4)` |
| Text | `#c8c8c8` |
| Dim text | `#666` |
| Bright text | `#ebebeb` |
| Card bg | `#0d0d0d` |
| Subtle line | `#1c1c1c` |
| Border (accent) | `rgba(184,154,86,0.18)` |

- **Fonts**: `Space Grotesk` (UI / labels) + `Lora` (serif, titles / quotes; italic for artwork titles in lightbox)
- **Labels**: Small uppercase (10–11px), wide letter-spacing (0.18em–0.28em)
- **Gold bar**: `width: 28–40px; height: 1px; background: var(--gold)` — recurring divider motif
- **No rounded corners. No shadows. Sharp-edged everything.**
- **Gallery mode**: artwork images are always full colour, no grayscale filter

---

## 8. Technical Reference

### RPC
```
https://mainnet.gateway.tenderly.co   ← works reliably; eth.drpc.org / ankr / 1rpc.io do not
```

### IPFS gateway
```
https://w3s.link/ipfs/{CID}           ← use this; ipfs.io and cloudflare-ipfs throttle/403
```

### Reservoir API
```
GET https://api.reservoir.tools/tokens/v7?collection={CONTRACT}&limit=20&sortBy=tokenId
Headers: x-api-key: demo
```
Runs client-side at runtime. Returns `tokens[].market.floorAsk.price.amount.decimal` for listings.
Note: `api.reservoir.tools` returns ECONNREFUSED from the server/build environment — this is expected and correct. It only works in the user's browser.

### ABI selectors
```
tokenURI(uint256)  → 0xc87b56dd
ownerOf(uint256)   → 0x6352211e
```

---

## 9. Roadmap — What's Next

### Immediate next (Phase 2b)
- Wire live `ownerOf()` calls at page load to replace pre-seeded `owner` values in both PoP and Fracture
- Fetch runs against `mainnet.gateway.tenderly.co` for each tokenId

### Phase 2c — Wallet connect
- Add "Connect Wallet" button to collect page nav
- Use CDN-imported `viem` or `ethers` (vanilla, no build step) with injected provider
- Or defer to Next.js app if/when What Remains primary mint needs SSR share cards

### What Remains — when contract is ready
- Deploy contract on Transient Labs
- Build `collect/what-remains/index.html` (Phase 2a architecture)
- Update collect grid card (remove coming-soon, add link)
- Add primary mint button (Phase 2c)

### Phase 3 — Secondary market UI
- "List" / "Make offer" actions for wallet-connected visitors

---

## 10. Lessons Learned (don't repeat these mistakes)

- **Malformed HTML comment kills script**: `<!-- comment */` (using `*/` instead of `-->`) causes the HTML parser to treat the entire rest of the document as a comment. Always close comments with `-->`.
- **Tag balance regex false positives**: `<div` inside JS template literals in `<script>` blocks are counted by naive regex. Strip `<script>` content before running tag balance checks.
- **IPFS gateway**: Always use `w3s.link`. Other gateways throttle.
- **Reservoir from build env**: ECONNREFUSED is expected — the fetch is client-side only.
