# Websikkerhet

For nettsider og serverløse endepunkter.

## Hoder

- [ ] Content-Security-Policy med `default-src 'none'` og bare de kildene som trengs. Ingen `'unsafe-inline'` eller
      `'unsafe-eval'`.
- [ ] Ingen innebygd kode i HTML: skript og stilark ligger i egne filer. En kontroll i bygget stopper brudd.
- [ ] Trusted Types: `require-trusted-types-for 'script'`, og bare smale policyer med navn.
- [ ] HSTS med `includeSubDomains`, `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy`,
      `Cross-Origin-Opener-Policy` og `Cross-Origin-Resource-Policy`.
- [ ] Rammer: `frame-ancestors 'none'` med mindre siden skal kunne bygges inn.
- [ ] Sider på verter som ikke kan sette hoder (for eksempel GitHub Pages), har hodene satt i en proxy foran, eller
      avviket er beskrevet i repoet. Slike sider har ikke skjemaer eller skript fra tredjeparter.
- [ ] Rapporter om brudd (`report-uri` og `report-to`) går til et eget endepunkt som ikke lagrer personopplysninger.

## Kode i nettleseren

- [ ] Ingen `innerHTML`, `outerHTML`, `insertAdjacentHTML` eller `document.write`. Elementer bygges med DOM-metoder.
- [ ] Tredjeparter (skript, rammer, skrift) lastes bare når de trengs, og helst først når brukeren ber om det.
- [ ] Ingen informasjonskapsler uten at det er nødvendig og beskrevet i personvernerklæringen.

## Serverløse endepunkter

- [ ] Bare metodene som trengs. Andre metoder avvises med 405.
- [ ] Ingen parametere fra forespørselen bestemmer hvor data hentes fra.
- [ ] Størrelsen på innkommende data er begrenset der kroppen leses, ikke bare ut fra `Content-Length`.
- [ ] Rate limiting per IP.
- [ ] Svar mellomlagres når det går, så trafikk ikke går videre til andre tjenester.
- [ ] Logger inneholder ikke IP, nettleserinfo, spørrestrenger eller innhold fra brukeren. Det gjelder også
      plattformens egne logger for kall (for eksempel logger for hvert kall til en Worker, med IP).
- [ ] Ingen hemmeligheter i koden. Trengs de, ligger de som secrets i plattformen.
- [ ] Kontaktinformasjon som sendes til andre tjenester (for eksempel i `User-Agent`), tilhører organisasjonen, ikke
      en person.

## Skjemaer

- [ ] Skjemaer sender til et eget endepunkt som følger reglene over, med beskyttelse mot roboter (for eksempel
      Turnstile) og et skjult felt som fanger enkle roboter.
- [ ] Innsendinger lagres bare der de trengs (for eksempel issues i et privat repo), og bare feltene som trengs.
- [ ] Personvernerklæringen sier hva som samles inn, hvorfor, hvor det lagres og hvor lenge.
- [ ] Skjemasidene har egen CSP med bare de ekstra kildene skjemaet trenger.

## Domene

- [ ] DNSSEC, CAA med bare utstederne som brukes, og varsling ved nye sertifikater (Certificate Transparency).
- [ ] Domenet er verifisert hos tjenester som ellers kan la andre ta i bruk underdomener (for eksempel GitHub Pages).
- [ ] E-post etter hvordan domenet brukes:

| Domenet | SPF | DMARC | MX |
|---|---|---|---|
| Har ikke e-post | `v=spf1 -all` | `p=reject` | Null MX (`0 .`) |
| Tar bare imot og videresender | Bare videresendingstjenesten, `-all` | `p=reject` | Videresendingstjenesten |
| Sender og tar imot via en e-posttjeneste | Bare tjenestene som sender, `-all` | `p=reject` når rapportene viser at all e-post er justert, `p=quarantine` til da | E-posttjenesten |

Domener som sender, har DKIM hos alle tjenestene som sender, og DMARC-rapporter til en adresse eller tjeneste som
leses.
