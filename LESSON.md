# Blog tusul: API fetch hicheel (material)

## Zorilgo
Figma-giin blog zagvariig **zuvhun HTML + Tailwind CSS** bolon **fetch()** ashiglan ajilladag bolgoh.
JS file bish, `<script>` tag-iin dotor bichne.

## Tusliin bagts
```
blog/
├─ index.html   <- huudasnuudiin jagsaalt (home)
└─ post.html    <- neg huudsiig unshih huudas
```

## Gol point-uud

### 1. Tailwind CSS (CDN)
Install hereggüi, neg mor:
```html
<script src="https://cdn.tailwindcss.com"></script>
```
Daraa n class-aar zagvar bichne: `bg-[#050A24] text-white max-w-[1100px] mx-auto grid md:grid-cols-2 gap-8`
- `md:` = tom delgets deer l ajillana (responsive)
- `[#050A24]` = custom ongo

### 2. API gej yu ve?
Programm hoorondoo data solilcdog "tsonh". Bid **dev.to (Forem) API** ashiglasan:
- Jagsaalt: `GET https://dev.to/api/articles?per_page=9`
- Neg huudas: `GET https://dev.to/api/articles/{id}`
Bürelzel: https://developers.forem.com/api/v0#tag/articles/operation/getArticles
Brauzer deer shuud URL-iig neeh ni JSON-g haruulna.

### 3. fetch() - hamgiin engiin baidal
```js
fetch(API_URL)
  .then((res) => res.json())   // hariug JSON bolgono
  .then((posts) => { ... })    // data-g ashiglana
  .catch(() => { ... });       // aldaa garval
```

### 4. Data-g HTML bolgoh
`posts.map(...)` bolgon element deer template literal (`` `...${p.title}...` ``) bichij, `.join("")`-ээр niilüülj `innerHTML`-d onoono.

### 5. Daraagiin huudas ruu data damjuulah
Index deer: `<a href="post.html?id=${p.id}">`
Post deer: `new URLSearchParams(location.search).get("id")` -> id-g unshaad `fetch(.../articles/${id})`.
Ene ni "URL query parameter" - huudas hoorond medee damjuulah hamgiin ner arga.

### 6. Fieldüüd (list vs neg huudas)
| Utga | Jagsaalt | Neg huudas |
|---|---|---|
| Garchig | `title` | `title` |
| Tovch | `description` | `description` |
| Ognoo | `readable_publish_date` | adil |
| Zurag | `cover_image` (null baij boldog!) | adil |
| Tag | `tag_list` (array) | `tags` (array) |
| Content | - | `body_html` |
Anхаar: jagsaalt deer `tag_list` array, neg huudas deer `tag_list` string baidag, tiimees `tags` ashiglasan.

### 7. Anhaaraх zuilüüd
- `cover_image` null baij boldog -> `||` ashiglan fallback zurag.
- `body_html`-g `innerHTML`-d tavih ni ayulgüi bolohgüi bol data-d itgeh heregtei (XSS). Ene bol surgaltiin jishee.
- Local file-aas ajilluulahad CORS asuudal garahgüi: dev.to API CORS-g zuvshuuruuldeg.

## Ajilluulah
Browser deer `index.html`-iig neeh (esvel VS Code "Live Server").

## Dasgal
1. Jagsaalt deer "reading time" (`reading_time_minutes`) nemeh.
2. `?tag=javascript` shuuj: `.../articles?tag=javascript`.
3. Loading gesen text-iig spinner bolgoh.
4. Projects / About huudas nemeh.
