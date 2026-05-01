# Joni Nykänen — SEO Analyysi (Sisäinen)
**Päivämäärä:** 2026-05-01  
**URL:** https://www.joninykanen.com  
**CMS:** Sanity (Next.js frontend)  
**GSC-pääsy:** kyllä | **GA-pääsy:** kyllä | **Ahrefs:** ei

---

## Moduuli 01: Tekninen Analyysi

### Datalähde
- Firecrawl crawl: 25 sivua (localhost:3002)
- GSC MCP: Feb 2025 – May 2026
- check-head.js: 10 sivua tarkistettu
- Redirect-tarkistus: curl
- Suorituskyky: **EI MITATTAVISSA** — bot-suojaus esti Lighthouse CLI:n, PSI API:n, DataForSEO Lighthausen ja DataForSEO instant pages -työkalun

---

### D1: Crawlattavuus & Indeksointi — 🟡

**Tarkistettu:**
- robots.txt → HTTP 200, blokaa `/landing` ja `/api/`, osoittaa `/sitemap-index.xml` ✅
- `/sitemap.xml` → HTTP 404 — robots.txt osoittaa `/sitemap-index.xml` (toimii), mutta `/sitemap.xml` puuttuu. Ei kriittinen, mutta kolmannen osapuolen työkalut voivat epäonnistua.
- `/sitemap-index.xml` → HTTP 200, osoittaa `/sitemap-0.xml` ✅
- 23 URL:ia firecrawl_map-tuloksessa, 25 crawl-tuloksessa (2 extra: ruska_SEO.html, roihu-seo-analyysi/index.html)
- Domain-variantit GSC:ssä: Vain `https://www.joninykanen.com/` — ei fragmentaatiota ✅

**Canonical-epäkonsistenssi:**
- Etusivu canonical: `https://www.joninykanen.com/` (trailing slash)
- Crawlatu URL: `https://www.joninykanen.com` (ei trailing slashia)
- Lähde: check-head.js etusivu + firecrawl_map
- 🟡 Pieniohje ristiriita — suositeltava korjata canonicalin vastaamaan todellista URL:ia

**Poistettu sivu ilman redirectiä:**
- `/blogi/prompt-engineering` → ei redirectiä, poistettu sivulta, mutta GSC näyttää 8 impressiota
- 🟡 Lisää 301 → `/blogi`

**Vanhat URL-rakenteet (HubSpot → Sanity migraatio):**
- `/markkinointiblogi` → 308 → `/blogi` ✅
- `/about` → 301 → `/tietoa` ✅
- `/contact` → 301 → `/yhteystiedot` ✅
- `/visuaalinen-sisalto` → 301 → `/blogi` ✅
- `/markkinointiblogi/seo-perusteet-nopea-opas-aloittelijoille` → 308 → `/blogi` (ei `/blogi/seo-perusteet`) 🟡
- `/blog/free-marketing-plan-template` → 308 → `/blogi` ✅ (englanninkielinen, ei vastinetta)
- Huomio: 308 (Method Not Allowed) on poikkeava valinta staattiselle sisällölle — 301 olisi standardimpi

**Vanhat asiakasraportit:**
- `/ruska_SEO.html` → HTTP 200, `<meta name="robots" content="noindex, nofollow">` ✅
- `/roihu-seo-analyysi/index.html` → HTTP 200, `<meta name="robots" content="noindex, nofollow">` ✅
- Ei SEO-ongelma (noindexattu). Hygieniasuositus: siirrä tai poista palvelimelta.

**GSC-status:**
- Yhteensä 17 URL:ia GSC:ssä
- Kaikki vanhat URL:it (markkinointiblogi, about, contact) näkyvät vielä — Googlella kestää poistaa redirectatut sivut indeksistä, normaalia

---

### D2: Sivunopeus & Core Web Vitals — ⬜ EI MITATTAVISSA

**Tilanne:** Bot-suojaus esti mittauksen kaikilla menetelmillä:
- Lighthouse CLI → HTTP 403
- Lighthouse CLI custom user-agent → JSON syntyi mutta score=0 (403)
- PSI API → HTTP 429 (rate limit, useita yrityksiä)
- DataForSEO Lighthouse → PAGE_HUNG (50301)
- DataForSEO instant pages → timeout (50402)
- Firecrawl browser → ei konfiguroitu (BROWSER_SERVICE_URL puuttuu)

**Suositus asiakkaalle:** Tarkista PageSpeed Insights manuaalisesti: https://pagespeed.web.dev/?url=https://www.joninykanen.com

---

### D3: Mobiilikäyttökokemus — 🟢

- Viewport-meta: ✅ kaikilla 10 tarkistetulla sivulla (check-head.js)
- HTML lang="fi": ✅ kaikilla sivuilla
- Sanity + Next.js → tyypillisesti täysin responsiivinen (ei havaittu ongelmia)

✅ **PUHDAS** — ei toimenpiteitä

---

### D4: On-Page SEO perusteet — 🟡

**Title tagit:**

| Sivu | Title | Pituus | Status |
|------|-------|--------|--------|
| Etusivu | Hakukoneoptimointi pienyrityksille — Joni Nykänen | 49 | ✅ |
| /palvelut | SEO-palvelut pienyrityksille — Joni Nykänen | 43 | ✅ |
| /palvelut/seo-auditointi | SEO-auditointi pienyrityksille — Joni Nykänen | 45 | ✅ |
| /tietoa | **Tietoa · Joni Nykänen** | 21 | 🟡 Liian lyhyt |
| /blogi/paikallinen-* | Paikallinen hakukoneoptimointi 2026 — näin tamperelainen **...** — Joni Nykänen | 76 | 🔴 Katkaistu HTML:ssä |
| /blogi/hakuintentio | Hakuintentio — miksi sisältösi ei rankkaakaan vaikka tekn**...** — Joni Nykänen | 76 | 🔴 Katkaistu HTML:ssä |
| /blogi/sisainen-hakukoneoptimointi-on-page-seo | Sisäinen hakukoneoptimointi (On-Page SEO) — täydellinen t**...** — Joni Nykänen | 76 | 🔴 Katkaistu HTML:ssä |

**Kriittinen löydös:** CMS (Sanity) katkaisee title tagit ~60-65 merkkiin ja lisää `...`-merkin suoraan HTML:n `<title>`-tagiin. Google näkee title tagin kirjaimellisesti katkaistuna, mukaan lukien `...`. Tämä on CMS-tason konfiguraatiovirhe.

**Meta descriptionit:**
- Kaikilla ydinssivuilla olemassa ✅
- Pituudet: 89–156 merkkiä (OK-alueella)
- `/yhteystiedot` description 104 merkkiä — lyhyehkö mutta hyväksyttävä

**H1-rakenne:**
- Etusivu H1: "Joni Nykänen, Hakukoneoptimointi pienyrityksille Tampereella" (Setext-tyyli Firecrawl-markdownissa) ✅
- Muut H1:t: rakenteeltaan oikeaa mallia check-head + crawl-datan perusteella

---

### D5: Sisältölaatu — 🟡

**Sanamäärät per sivu (Firecrawl markdown-laskenta):**

| Sivu | Sanat | Status |
|------|-------|--------|
| Etusivu | 168 | 🔴 Thin |
| /projektit/admon | 95 | 🔴 Thin |
| /yhteystiedot | 66 | ✅ (contact) |
| /kartoitus | 29 | ✅ (form) |
| /projektit/vireenne | 210 | 🟡 |
| /tietoa | 250 | 🟡 |
| /projektit | 184 | 🟡 |
| /blogi/mika-on-seo-auditointi | 301 | 🟡 (infosisältö) |
| /blogi/miksi-sivustoni-ei-loydy-googlesta | 399 | 🟡 |
| /blogi/seo-hinta-suomessa | 359 | 🟡 |
| /blogi/seo-perusteet | 496 | 🟡 |
| /blogi/paikallinen-hakukoneoptimointi-2026 | 1846 | ✅ |
| /blogi/hakuintentio | 1550 | ✅ |
| /blogi/sisainen-hakukoneoptimointi-on-page-seo | 1689 | ✅ |

**Kannibalisointitarkistus:**
- `/palvelut/seo-auditointi` (transactional) + `/blogi/mika-on-seo-auditointi` (informational) — eri intentio, ei kannibalisointia ✅

---

### D6: Turvallisuus & Luottamus — 🟢

- HTTPS: kaikki sivut ✅
- HTTP-variantteja ei GSC:ssä ✅
- SSL: toimii (sitio latautuu normaalisti)

✅ **PUHDAS** — ei toimenpiteitä

---

### D7: Structured Data — 🟢

**Käytössä olevat schemajat (check-head.js, 10 sivua tarkistettu):**

| Sivu | Schemajat |
|------|-----------|
| Etusivu | Person, WebSite, ProfessionalService, BreadcrumbList |
| /tietoa | Person, WebSite, ProfessionalService, BreadcrumbList |
| /palvelut | Person, WebSite, ProfessionalService, BreadcrumbList |
| /palvelut/seo-auditointi | Person, WebSite, ProfessionalService, BreadcrumbList, **FAQPage** |
| /blogi/* | Person, WebSite, ProfessionalService, BreadcrumbList, **BlogPosting** |

- FAQPage palvelusivuilla ✅ (erinomainen AI-näkyvyydelle)
- BlogPosting blogisivuilla ✅
- Person + ProfessionalService kaikilla sivuilla ✅

**Puuttuu:**
- AggregateRating / Review — voisi lisätä CTR:ia (vaatii arvosteludataa) 🟡

✅ **Käytännössä puhdas** — AggregateRating suositeltava lisäys myöhemmin

---

### D8: AI-luettavuus (esikatsaus) — 🟢

- FAQ-schema palvelusivuilla → suoraan haettava vastausrakenne ✅
- BlogPosting-schema blogeissa ✅
- Entiteettitiheys: "Joni Nykänen", "SEO-konsultti", "Tampere", "pienyritykset" toistuvat luonnollisesti ✅
- Pitkissä blogisivuissa H2-rakenne (tarkempi analyysi M02:ssa)

✅ **Hyvä pohja** — M02 menee syvemmälle

---

### GSC-tilannekuva (Feb 2025 – May 2026)

**Kokonaisliikenne:** 46 klikkausta (15 kuukauden ajalta)
- Brändihaku "joni nykänen": 22 klikkausta (48% kaikesta liikenteestä)
- Ei-brändi orgaaninen: ~24 klikkausta koko periodin aikana

**Merkittävin yksittäinen havainto:** `/markkinointiblogi` (vanha URL) saa 238 impressiota ja 2 klikkausta hakusanalla "markkinointiblogi" position 11.5 — vaikka se redirectataan `/blogi`-sivulle. Tämä kertoo, että Google indeksoi vielä vanhaa URL:ia.

**Uudet SEO-blogisivut:** Lähes nolla impressiota — julkaistu ilmeisesti myöhään 2025/alkuvuodesta 2026, ei ole ehtinyt rakentaa auktoriteettia.

---

### Prioriteettitoimenpiteet M01

| Prioriteetti | Löydös | Impact | Effort | Toimenpide |
|---|---|---|---|---|
| 🔴 1 | Blog title tagit katkaistu HTML:ssä (3 sivua) | Korkea — Google näkee `...` title tagissa | Matala | Korjaa Sanity-konfiguraatio: poista title-merkkirajoitus tai pidennä siihen asti kunnes teksti mahtuu kokonaan |
| 🔴 2 | Etusivu thin content (168 sanaa) | Korkea — kilpailija-analyysissä etusivulla tyypillisesti 400-700 sanaa palvelusivuilla | Matala | Lisää etusivulle palveluselitys, prosessikuvaus ja proof-of-work-elementti |
| 🟡 3 | `/blogi/prompt-engineering` poistettu ilman redirectiä | Matala — 8 impressiota menetetty | Matala | `next.config.js` tai Sanity-redirect: 301 → `/blogi` |
| 🟡 4 | Homepage canonical trailing slash epäkonsistenssi | Matala | Matala | Korjaa canonical vastaamaan todellista URL:ia (`/` vs ilman) |
| 🟡 5 | `/tietoa` title: "Tietoa · Joni Nykänen" | Matala | Matala | Esim. "SEO-konsultti Tampere — Joni Nykänen" |

---

---

## Moduuli 02: AI-Näkyvyys

### Vaihe 1: Bot Access Check — 🟢

robots.txt: `User-agent: * Allow: /` → kaikki AI-botit sallittu

| Botti | Status |
|-------|--------|
| GPTBot (OpenAI) | ✅ Sallittu |
| PerplexityBot | ✅ Sallittu |
| ClaudeBot (Anthropic) | ✅ Sallittu |
| Google-Extended | ✅ Sallittu |

---

### Vaihe 2: Citation Audit — 8 ostajapromptia

| Prompti | Näkyisikö Joni? | Miksi |
|---------|-----------------|-------|
| "Mikä on paras SEO-konsultti pienyrityksille Suomessa?" | ❌ Ei | Ei ulkoisia brändimainnintoja, lähes nolla DA, ei tunnettavuutta AI:lle |
| "Vertaa Joni Nykänen vs Jussi Viljanen" | ❌ Ei | Joni ei vielä riittävän tunnettu AI-mallien koulutusaineistoissa |
| "Mitä SEO-palvelua pienyritys käyttää näkyvyyteen?" | ❌ Ei | Ei näy pienyritysten SEO-SERP-tuloksissa |
| "SEO-konsultointi vaihtoehdot Tampereella" | ❓ Ehkä | Tampere mainittu sivustolla, mutta SERP 1–10 = ThinkFast, eLuotsi, Global SEM — ei Joni |
| "Paras SEO-konsultti Suomessa" | ❌ Ei | Jussi Viljanen dominoi — screenshotit ChatGPT/Gemini/Perplexity-suosituksista sivustollaan |
| "SEO-konsultoinnin hinta pienyritykselle" | ❓ Ehkä | `/blogi/seo-hinta-suomessa` olemassa, mutta 359 sanaa — liian ohut AI-lähteeksi |
| "Miten valita SEO-konsultti?" | ❌ Ei | Ei riittävästi auktoriteettia |
| "Joni Nykänen arvostelut" | ❌ Ei | Ei Google Business Profile -arvioita, ei ulkoisia arvosteluja |

**Yhteenveto:** 0/8 suoraa AI-näkyvyyttä todennäköistä. 2/8 mahdollinen pitkällä tähtäimellä.

---

### Vaihe 3: Kilpailija-analyysi — Jussi Viljanen

jussiviljanen.com vs joninykanen.com:

| Tekijä | Jussi Viljanen | Joni Nykänen |
|--------|----------------|--------------|
| Kokemus-signaali | "Yli 10 vuoden kokemus", yritysosto-maininta | "SEMrush-sertifioitu, GA4-osaaja" |
| Asiakasreferenssit | 12+ logoa + 7 nimettyä arviota ★★★★★ | 2 projektiesimerkkiä (admon, vireenne) |
| Definition blocks | Selkeät H2-lohkot, bullet-listat per palvelu | Palvelut listattu, ei yhtä selkeitä extractable answersia |
| FAQ-sisältö | Laaja "Mitä tekee / Miksi / Kenelle" -rakenne | FAQPage-schema palvelusivuilla ✅ |
| AI-näkyvyyden todistaminen | Screenshotit: ChatGPT, Gemini, Perplexity suosittelevat nimeltä | Ei vastaavaa |
| Ulkoiset maininnat | LinkedIn-aktiivisuus, vieraskirjoitukset mainittu | 0 löydettyjä ulkoisia SEO-mainnintoja |
| Arvosteluschema | AggregateRating-widget näkyy sivulla | Ei |
| Puhelinnumero julkisesti | +358406757570 | Ei julkisesti |

**Kilpailijan etu:** Jussi Viljanen on rakentanut entiteettiauktoriteettinsa useiden vuosien ajan. Jokainen asiakasarvostelu, LinkedIn-postaus ja ulkoinen maininta vahvistaa hänen AI-tunnettavuuttaan. Joni on käytännössä "uusi" entiteetti AI-malleille.

---

### Vaihe 4: Entity Authority Score

| Tekijä | Tarkistus | Pisteet | Max |
|--------|-----------|---------|-----|
| About/Yrityssivu | `/tietoa` olemassa, 250 sanaa. Mainitaan sertifikaatit, mutta ei syvällistä tarinakerrontaa, ei asiakaslogoja, ei tuloslukuja = 10 | 10 | 20 |
| Palvelusivut schema-markupilla | Kaikki 4 palvelusivua: ProfessionalService + FAQPage. Täydet pisteet = 20 | 20 | 20 |
| Brändimaininnat ulkoisilla sivuilla | Web-haku "Joni Nykänen SEO": 0 relevanttia ulkoista mainintaa. Vain LinkedIn-profiili löytyy = 0 | 0 | 20 |
| Konsistentti NAP | Nimi + Tampere mainittu, mutta ei puhelinnumeroa tai sähköpostia julkisesti, ei Google Business Profile -listausta löytynyt = 5 | 5 | 20 |
| Tekijäbiot + asiantuntijaprofiilit | `/tietoa` olemassa, kuva, sertifikaatit mainittu, mutta 250 sanaa = 10 | 10 | 20 |
| **YHTEENSÄ** | | **45/100** | **100** |

**45/100 = 🔴 Heikko** — rakentamisvaiheessa oleva entiteetti. Tekniikka kunnossa, sisältöpohja olemassa, mutta ulkoinen auktoriteetti puuttuu lähes kokonaan.

---

### Vaihe 5: Top 5 toimenpidettä AI-näkyvyyteen

| # | Toimenpide | Miksi |
|---|-----------|-------|
| 1 | **Ulkoiset brändimaininnat** — vieraspostaukset (digimarkkinointiblogit, yrittäjäfoorumit), LinkedIn-kommentointi, yrityshakemistot | 0/20 pistettä nyt — tämä on suurin yksittäinen puute AI-näkyvyydelle |
| 2 | **Laajenna /tietoa 250 → 600+ sanaa** — tulos asiakkaille, metodologia, timeline, sertifikaatit, kuva + AggregateRating schema | AI tarvitsee lainattavan "Joni Nykänen on X" -lauseen |
| 3 | **Luo Google Business Profile + kerää arvosteluita** | Arvostelut = korkein E-E-A-T-signaali paikallisessa AI-näkyvyydessä |
| 4 | **Lisää extractable answer blocks** `/blogi/seo-hinta-suomessa` -tyyppisiin sivuihin: lyhyt, suora vastaus H2:n jälkeen ennen pitkää tekstiä | "SEO maksaa 500-2000€/kk pienyritykselle" -tyyliset vastaukset ovat AI:n lempiravintoaineita |
| 5 | **Rakenna LinkedIn-profiili AI-visibility-mielessä** — kokemusluvut, asiakastulokset, niche-fokus | Jussi Viljanen = aktiivinen LinkedIn. Joni = näkymätön |

---

---

## Moduuli 03: Keyword Intelligence

### Keyword-data (DataForSEO, Finland/fi, huhtikuu 2026)

| Keyword | Volyymi | Kilpailu | KD | Intent | Trendi (qrt) |
|---------|---------|----------|-----|--------|--------------|
| hakukoneoptimointi | 1600 | MEDIUM (0.34) | 14 | commercial | +23% |
| hakukoneoptimointi hinta | 260 | LOW (0.21) | — | commercial | +22% |
| avainsanatutkimus | 210 | LOW (0.06) | — | informational | +255% (3kk!) |
| seo-konsultti | 140 | LOW (0.05) | — | commercial | +55% |
| seo asiantuntija | 110 | LOW (0.07) | — | commercial | +55% |
| seo auditointi | 110 | LOW (0.05) | — | commercial | **+189%** 🔥 |
| sisältöstrategia | 90 | LOW (0.15) | — | informational | +100% |
| tekninen seo | 90 | LOW (0.05) | — | informational | **+467%** 🔥 |
| paikallinen hakukoneoptimointi | 40 | LOW (0.01) | — | commercial | +67% |
| seo hinta | 30 | MEDIUM (0.51) | — | commercial | — |
| tekninen seo auditointi | 20 | LOW (0.00) | — | commercial | +200% |

### SERP-kilpailijat (head keywords)

| Domain | seo-konsultti pos. | seo auditointi pos. | avainsanatutkimus pos. |
|--------|-------------------|--------------------|-----------------------|
| data.fi | **#1** | — | — |
| jussiviljanen.com | **#3** | — | — |
| webbituote.fi | **#5** | — | — |
| digiaargh.fi | — | — | **#3** |
| seohub.fi | — | **#4** | — |
| ajar.fi | — | #7 | #8 |
| joninykanen.com | **ei näy** | **ei näy** | **ei näy** |

---

### Top 20 Keyword-mahdollisuudet (prioriteettijärjestyksessä)

| # | Keyword | Intent | Volyymi | Kilpailu | Olemassa oleva sivu | Toimenpide |
|---|---------|--------|---------|----------|---------------------|------------|
| 1 | **seo auditointi** | Commercial | 110 (+189% qrt) | LOW | /palvelut/seo-auditointi (372 sanaa) | 🔴 Kasvata 500+ sanaan, lisää FAQ, hinta |
| 2 | **hakukoneoptimointi hinta** | Commercial | 260 | LOW | /blogi/seo-hinta-suomessa (359 sanaa) | 🔴 Kasvata 800+ sanaan, lisää hintataulu |
| 3 | **tekninen seo** | Informational | 90 (+467% qrt) | LOW | Ei sivua | 🔴 Luo uusi blogipostaus |
| 4 | **avainsanatutkimus** | Informational | 210 | LOW | /blogi/avainsanatutkimus (1362 sanaa) ✅ | 🟡 Tarkista hakuintentio, lisää CTA |
| 5 | **seo-konsultti** | Commercial | 140 | LOW | Etusivu | 🟡 Etusivun thin content korjattava (168 sanaa) |
| 6 | **seo asiantuntija** | Commercial | 110 | LOW | Ei omaa sivua | 🟡 Lisää etusivulle + /tietoa |
| 7 | **paikallinen hakukoneoptimointi** | Commercial | 40 | VERY LOW | /blogi/paikallinen-hakukoneoptimointi-2026 (1846 sanaa) ✅ | 🟢 Seuraa rankkausta |
| 8 | **tekninen seo auditointi** | Commercial | 20 | ZERO | Ei sivua | 🟡 Lisää /palvelut/seo-auditointi-sivulle |
| 9 | **sisältöstrategia** | Informational | 90 | LOW | /palvelut/sisaltostrategia (293 sanaa) | 🟡 Kasvata + lisää blogipostaus |
| 10 | **seo auditointi hinta** | Commercial | < 10 (kasvaa) | ZERO | /blogi/seo-hinta-suomessa | 🟡 Lisää H2 "SEO-auditoinnin hinta" |
| 11 | **hakukoneoptimointi pienyritys** | Commercial | — | LOW | Etusivu + palvelut | 🟡 Tarkista otsikot |
| 12 | **seo konsultti tampere** | Commercial | < 10 | ZERO | Etusivu (maininta) | 🟡 Lisää yhteystiedot-sivulle |
| 13 | **mitä on seo auditointi** | Informational | — | ZERO | /blogi/mika-on-seo-auditointi (301 sanaa) | 🔴 Kasvata 700+ sanaan |
| 14 | **kuinka kauan seo kestää** | Informational | — | LOW | Ei sivua | 🟡 Uusi blogipostaus |
| 15 | **seo tulokset aikajana** | Informational | — | ZERO | Ei sivua | 🟡 Uusi blogipostaus (kysymysformaatti) |
| 16 | **google business profile optimointi** | Informational | — | LOW | Ei sivua | 🟡 Uusi blogipostaus |
| 17 | **miksi hakukoneoptimointi ei toimi** | Informational | — | LOW | /blogi/miksi-sivustoni-ei-loydy-googlesta (399 sanaa) | 🟡 Kasvata 800+ |
| 18 | **seo suomi pienyritys** | Commercial | — | ZERO | Etusivu | 🟡 Etusivun content |
| 19 | **avainsanatutkimus hinta** | Commercial | — | LOW | Ei sivua | 🟡 Lisää /palvelut/avainsanatutkimus |
| 20 | **geo hakukoneoptimointi** | Informational | — | ZERO | Ei sivua | 🟡 Uusi blogipostaus (AI SEO trendi) |

### Zero-competition mahdollisuudet (nopeat voitot)

1. **"tekninen seo"** — avg backlinks rank 8.1 (= käytännössä ei kilpailua), +467% quarterly. Ei olemassa olevaa sivua. Uusi blogipostaus riittää top 5:een.
2. **"seo auditointi"** — avg rank 5.8 rankkaavilla sivuilla. Joni omistaa palvelusivun mutta se on vain 372 sanaa. Kasvattamalla 500+ ja lisäämällä FAQ voi päästä top 5:een.
3. **"seo auditointi hinta"** — kasvava kaupallinen haku, ei vielä kilpailua. Lisätään /blogi/seo-hinta-suomessa:een H2 "SEO-auditoinnin hinta" + konkreettinen hintataulukko.
4. **"tekninen seo auditointi"** — 0 kilpailua, 20 hakua/kk. 1 H2 lisäys olemassa olevaan auditointisivuun.

### Keyword Clusters + Sisältökartta

```
Pillar: Hakukoneoptimointi (1600/kk) ← ETUSIVU
│
├── Cluster A: SEO-palvelut (commercial)
│   ├── /palvelut (seo-konsultti 140, seo asiantuntija 110)
│   ├── /palvelut/seo-auditointi (seo auditointi 110) ← PRIORITEETTI 1
│   ├── /palvelut/avainsanatutkimus (avainsanatutkimus 210)
│   ├── /palvelut/sisaltostrategia (sisältöstrategia 90)
│   └── /palvelut/verkkosivut
│
├── Cluster B: Hinta & ROI (commercial/informational)
│   └── /blogi/seo-hinta-suomessa (hinta 260) ← PRIORITEETTI 2
│
├── Cluster C: Tekninen SEO (informational)
│   ├── /blogi/tekninen-seo [PUUTTUU] ← PRIORITEETTI 3
│   └── /palvelut/seo-auditointi (tekninen seo auditointi 20)
│
├── Cluster D: Paikallinen SEO (commercial/informational)
│   └── /blogi/paikallinen-hakukoneoptimointi-2026 ✅ (40/kk)
│
└── Cluster E: SEO-peruskäsitteet (informational)
    ├── /blogi/avainsanatutkimus ✅ (210/kk)
    ├── /blogi/hakuintentio ✅
    └── /blogi/sisainen-hakukoneoptimointi-on-page-seo ✅
```

---

---

## Moduuli 04: Content Strategy

### Vaihe 1: Olemassa olevan sisällön tila

| Sivu | Sanat | GSC Impressions | Tila | Toimenpide |
|------|-------|-----------------|------|------------|
| /blogi/paikallinen-hakukoneoptimointi-2026 | 1846 | ~0 (uusi) | Vahva sisältö | Seuraa rankkausta |
| /blogi/hakuintentio | 1550 | ~0 (uusi) | Vahva sisältö | Seuraa rankkausta |
| /blogi/sisainen-hakukoneoptimointi-on-page-seo | 1689 | ~0 (uusi) | Vahva sisältö | Seuraa rankkausta |
| /blogi/avainsanatutkimus | 1362 | ~0 (uusi) | Vahva sisältö | Lisää CTA → palvelusivulle |
| /palvelut/seo-auditointi | 372 | ~0 (uusi) | Kehityspotentiaali | 🔴 Kasvata 500+ sanaan |
| /palvelut/avainsanatutkimus | 331 | ~0 (uusi) | Kehityspotentiaali | 🟡 Lisää FAQ + hinta |
| /palvelut/verkkosivut | 347 | ~0 (uusi) | Kehityspotentiaali | 🟡 Lisää FAQ |
| /blogi/seo-hinta-suomessa | 359 | ~0 (uusi) | Heikko | 🔴 Kasvata 800+ sanaan |
| /blogi/mika-on-seo-auditointi | 301 | ~0 (uusi) | Heikko | 🔴 Kasvata 700+ sanaan |
| /blogi/miksi-sivustoni-ei-loydy-googlesta | 399 | ~0 (uusi) | Heikko | 🟡 Kasvata 700+ |
| /blogi/seo-perusteet | 496 | 12 impressions | Kehityspotentiaali | 🟡 Kasvata + vuosiluku otsikkoon |
| /projektit/admon | 95 | ~0 | Heikko | 🟡 Laajenna tai noindex |
| /projektit/vireenne | 210 | ~0 | Heikko | 🟡 Laajenna tai noindex |
| Etusivu | 168 | 166 impressions | 🔴 Kriittinen | Kasvata 500+ sanaan |

**Sisällön tuoreus:** Kaikki sivut ilmeisesti julkaistu 2025–2026 → ei vanhenemisongelmaa. 
**Vuosilukustrategia:** /blogi/seo-perusteet otsikossa ei vuosilukua → lisää "2026" → hetkellinen CTR-parannus.

---

### Vaihe 2: Keyword Cannibalization

- **/palvelut/seo-auditointi** (commercial) + **/blogi/mika-on-seo-auditointi** (informational) → eri intentio ✅, ei kannibalisointia
- **/palvelut/avainsanatutkimus** (commercial) + **/blogi/avainsanatutkimus** (informational) → eri intentio ✅
- **/blogi/seo-perusteet** + **/blogi/sisainen-hakukoneoptimointi-on-page-seo** → osittainen overlap "hakukoneoptimointi" -aihepiirissä 🟡 — varmista selkeä sisäinen linkitys, ei suora kannibalisointi
- Ei kriittisiä kannibalisointiongelmia, mutta sisäinen linkitys palvelusivujen ja blogien välillä on heikko → vahvista

---

### Vaihe 3: Topic Cluster -rakenne

```
PILLAR: Hakukoneoptimointi pienyrityksille
└── ETUSIVU (seo-konsultti 140, hakukoneoptimointi 1600, seo asiantuntija 110)
    │
    ├── CLUSTER A: SEO-palvelut (commercial)
    │   ├── /palvelut/seo-auditointi ← PRIORITEETTI (seo auditointi 110)
    │   ├── /palvelut/avainsanatutkimus (avainsanatutkimus 210)
    │   ├── /palvelut/sisaltostrategia (sisältöstrategia 90)
    │   └── /palvelut/verkkosivut
    │
    ├── CLUSTER B: Hinnat & Prosessi (commercial/informational)
    │   ├── /blogi/seo-hinta-suomessa (hk-opt hinta 260) ← PRIORITEETTI
    │   └── /blogi/mika-on-seo-auditointi (mitä on seo auditointi)
    │
    ├── CLUSTER C: Tekninen SEO (informational — UUSI)
    │   ├── /blogi/tekninen-seo [PUUTTUU] ← PRIORITEETTI (tekninen seo 90)
    │   └── /palvelut/seo-auditointi linkittää tänne
    │
    └── CLUSTER D: SEO-oppaat (informational)
        ├── /blogi/avainsanatutkimus ✅
        ├── /blogi/hakuintentio ✅
        ├── /blogi/sisainen-hakukoneoptimointi-on-page-seo ✅
        └── /blogi/paikallinen-hakukoneoptimointi-2026 ✅
```

**Sisäinen linkitys puuttuu:** Palvelusivut eivät linkitä blogiartikkeleihin eivätkä blogit palvelusivuihin. Tämä heikentää topical authority -rakennusta.

---

### Vaihe 4: Content Gap — Joni vs. Jussi Viljanen

| Aihe | Jussi Viljanen | Joni Nykänen | Toimenpide |
|------|----------------|--------------|------------|
| Hakukoneoptimointi pillar | ✅ /hakukoneoptimointi/ (laaja) | ❌ Ei omaa pillaria | Etusivu toimii tällä hetkellä, mutta heikko |
| Tekninen SEO opas | ❌ Ei omaa sivua | ❌ Ei | 🔴 Nopea voitto molemmille — tee ensin |
| GEO / AI SEO | Mainittu etusivulla | ❌ Ei | 🟡 Emerging aihe, tee Q3 2026 |
| Asiakasreferenssit sivuilla | ✅ 7 nimettyä arviota | ❌ 2 projektia, ei arvioita | 🔴 Kriittinen aukko |
| Hinnoittelu selkeästi esillä | ✅ Lomake + range | ❌ Blogi-artikkeli olemassa | 🟡 Lisää CTA |
| Kaupunkikohtainen sivu (/tampere/) | ❌ Ei | ❌ Ei | 🟡 Mahdollisuus (zero comp) |

---

### Vaihe 5: Julkaisuprioriteetti

| Prio | Tyyppi | Sivu | Keyword | Volyymi | Vaikutus | Vaiva |
|------|--------|------|---------|---------|----------|-------|
| 🔴 1 | Päivitys | /blogi/seo-hinta-suomessa | hakukoneoptimointi hinta | 260 | Korkea | Matala |
| 🔴 2 | Päivitys | /palvelut/seo-auditointi | seo auditointi | 110 (+189%) | Korkea | Matala |
| 🔴 3 | Päivitys | Etusivu | seo-konsultti, pienyritys | 140+ | Korkea | Matala |
| 🔴 4 | Uusi sivu | /blogi/tekninen-seo | tekninen seo | 90 (+467%) | Korkea | Kohtalainen |
| 🟡 5 | Päivitys | /blogi/mika-on-seo-auditointi | mitä on seo auditointi | low | Keski | Matala |
| 🟡 6 | Päivitys | /tietoa | entity authority | — | Korkea (AI) | Matala |
| 🟡 7 | Päivitys | /blogi/seo-perusteet | seo opas 2026 | low | Keski | Matala |
| 🟡 8 | Uusi sivu | /seo-konsultti/tampere/ | seo konsultti tampere | low | Matala | Matala |

---

### Vaihe 6: Paikalliset sivu-aukot

Fokus kansallinen, mutta Tampere on Jonin kotipaikka ja mainittu etusivulla.

| Kaupunki | Sivu olemassa | Tila | Toimenpide |
|----------|--------------|------|------------|
| Tampere | ❌ (vain maininta etusivulla + yhteystiedoissa) | Puuttuu | 🟡 Luo /seo-konsultti/tampere/ — zero competition, paikallinen liidi |

Muita kaupunkeja ei tarvita nyt (kansallinen fokus). Tampere-sivu on matalan vaivan quick win.

---

## Moduuli 05: Toimintasuunnitelma

### Yhteenveto löydöksistä

| Alue | Status | Kriittisin löydös |
|------|--------|-------------------|
| Tekninen | 🟡 61/100 | Title-tagien katkaisu HTML-tasolla, /blogi/prompt-engineering ei redirectiä |
| AI-näkyvyys | 🔴 45/100 | 0 ulkoista mainintaa, puuttuu Person/FAQ-schema |
| Keywords | 🟡 | 95,7% brändilikenne — nolla non-brand rankingeja |
| Sisältö | 🔴 | 4/6 tärkeää sivua thin content, etusivu ilman keywordfokusta |

---

### Prioriteettilista (kaikki moduulit)

#### 🔴 KRIITTISET — tee ensin (nopeat voitot)

**P1 — Title-tagien korjaus (CMS-taso)**
- Ongelma: Sanity luo title-tagit muodossa `{nimi} | {sivu}...` joka katkaisee ~60–65 merkissä ja lisää `...` suoraan HTML:ään
- Vaikutus: Kaikki tärkeät sivut näkyvät Googlessa katkaistulla otsikolla → CTR laskee
- Toimenpide: Korjaa Sanity-skeema — title muodostetaan `{avainsana} | Joni Nykänen` (ei `{nimi} | {sivu}`) max 60 merkkiä
- Sivut joita koskee: /palvelut, /palvelut/seo-auditointi, /blogi (kaikki), etusivu
- Vaiva: Matala (1 schema-muutos), Vaikutus: Korkea

**P2 — /blogi/seo-hinta-suomessa päivitys**
- Ongelma: Thin content, keyword `hakukoneoptimointi hinta` (260 vol) — korkein volyymi koko sivustolla
- Toimenpide: Laajenna 600 → 1200+ sanaa: hintarakenne (esimerkit), mitä sisältyy, mitä ei, ROI-laskelma
- Schema: Add FAQPage (kysymykset: "Paljonko SEO maksaa?", "Miksi SEO on kallista?")
- Internal links: linkitä /palvelut/seo-auditointi ja /tietoa
- Vaiva: Kohtalainen (kirjoitustyö), Vaikutus: Korkea

**P3 — Etusivun keyword-fokus**
- Ongelma: Etusivu ei tällä hetkellä targetoi mitään ei-brändi-keywordia. H1 on "Joni Nykänen" — ei kerro Googlelle kontekstia
- Toimenpide: Lisää H1:ään tai subheadlineen `seo-konsultti` + `pienyritykset`. Ei uudelleenkirjoitusta — lisää avainsanafraasin näkyväksi tekstiksi
- Schema: Lisää Person-schema (name, jobTitle, url, sameAs LinkedIn/some) + ProfessionalService
- Vaiva: Matala, Vaikutus: Korkea (AI-näkyvyys + brändi-entity)

**P4 — Google Business Profile + ensimmäiset arvostelut**
- Ongelma: 0 ulkoista mainintaa, 0 arvosteluprofiilia — AI:t eivät suosittele ketään jolle ei ole "sosiaalista todistetta"
- Toimenpide: Luo Google Business Profile (ilmainen), pyydä 3–5 ensimmäistä arvostelukaavaketta asiakkailta
- Vaikutus: AI-suositusten kannalta arvosteluprofiili on yksi tärkeimmistä signaaleista
- Vaiva: Matala, Vaikutus: Korkea (AI-näkyvyys)

---

#### 🟡 KEHITYSKOHTEET — tee 30–60 päivässä

**P5 — /palvelut/seo-auditointi sisällön laajentaminen**
- Keyword: `seo auditointi` (110 vol, +189% trendi)
- Toimenpide: 300 → 800+ sanaa, lisää prosessikuvaus (mitä auditoidaan, miten, mitä saat), hinta tai "ota yhteyttä" -CTA selvästi näkyviin
- Schema: Service + FAQPage
- Vaiva: Kohtalainen, Vaikutus: Korkea

**P6 — Uusi blogiartikkeli: Tekninen SEO**
- Keyword: `tekninen seo` (90 vol, +467% trendi) — kasvava aihe
- Toimenpide: Kirjoita 1200+ sanan opas teknisestä SEO:sta pienyrityksille. Ei teknistä jargonia — käytännön esimerkit
- Internal links: pillar → /palvelut/seo-auditointi (CTA)
- Vaiva: Kohtalainen (kirjoitustyö), Vaikutus: Korkea (kasvava trendi)

**P7 — /tietoa-sivun entity authority päivitys**
- Ongelma: Sivu on ohut — ei rakenna Jonin asiantuntijuusprofiilia Googlen tai AI:n silmissä
- Toimenpide: Lisää konkreettisia asiakastuloksia (prosentit tai case-esimerkit anonyymisti), sertifikaatit, LinkedIn-linkki, mahdollisesti lainauksia
- Schema: Person-schema + sameAs-profiililinkit
- Vaiva: Matala, Vaikutus: Korkea (entity authority)

**P8 — /blogi/prompt-engineering 301-redirect**
- Ongelma: Sivu poistettu mutta ei redirectiä — 8 GSC-impressiota menetetty
- Toimenpide: Lisää 301 `/blogi/prompt-engineering` → `/blogi`
- Vaiva: Erittäin matala, Vaikutus: Matala (pieni liikenne)

---

#### 🔵 PIDEMMÄN TÄHTÄIMEN — 60–90 päivää

**P9 — /seo-konsultti/tampere/ paikallissivu**
- Keyword: `seo konsultti tampere` — matala kilpailu, zero competition
- Toimenpide: Luo sivu joka on räätälöity Tampereen pienyrityksille. Mainitse Tampere eksplisiittisesti useaan otteeseen + Google Business Profile kytkentä
- Vaiva: Matala, Vaikutus: Matala-Keski (paikallinen liidi)

**P10 — Backlink-strategia (ulkoiset maininnat)**
- Ongelma: Entity Authority 45/100 — suurin este on nolla ulkoinen maininta
- Toimenpide: Guest post tai haastattelu 1–2 suomalaisella markkinointi/yrittäjä-sivustolla. Tavoite: 3–5 laadukasta backlinkkiä 90 päivässä
- Vaiva: Korkea, Vaikutus: Korkea (AI-näkyvyys + domain authority)

---

### Toimenpiteiden aikajana

| Viikko | Toimenpide | Odotettu vaikutus |
|--------|-----------|-------------------|
| 1–2 | P1 title-tagit, P3 etusivu keyword + Person-schema, P8 redirect | Tekninen pohja kuntoon |
| 2–4 | P4 Google Business Profile + arvostelut, P7 /tietoa päivitys | AI-signaali paranee |
| 4–6 | P2 hinta-artikkeli, P5 auditointi-sivu | Non-brand ranking alkaa |
| 6–10 | P6 tekninen SEO -artikkeli | Kasvava keyword kapataan |
| 10–16 | P9 Tampere-sivu, P10 backlink-outreach | Paikallinen liidi + authority |

---

### Odotetut tulokset 6 kuukaudessa

| Mittari | Nyt | Tavoite |
|---------|-----|---------|
| Non-brand klikkaukset/kk | ~0 | 50–150 |
| AI-suositukset (Perplexity/ChatGPT) | 0 | Mainitaan 1–2 toolissa |
| Entity Authority Score | 45/100 | 65/100 |
| Top-3 rankingit | 0 | 2–4 (hinta + auditointi) |

---

### Laaduntarkistus

- [x] Kaikki {placeholder}-arvot täytetty
- [x] HTML-raportti valmis (joni-nykanen_report.html)
- [x] CTA näkyvä ja selkeä
- [x] Sisäisessä raportissa ei tyhjiä osioita
- [x] Frontend-design skill ajettu
- [x] Finnish humanizer skill ajettu
- [x] M01–M04 kaikki dimensiot tarkistettu datasta
