# Tilgjengelighet og UX

## Krav

- [ ] WCAG 2.2 AA, kontrollert med axe i testene på alle sider.
- [ ] Siden fungerer på 320 px uten horisontal rulling (WCAG 1.4.10).
- [ ] Lys og mørk visning, som følger systemet til brukeren velger selv. Begge testes.
- [ ] Alt kan brukes med tastatur, med synlig fokus og logisk rekkefølge.
- [ ] Trykkflater på minst 24 × 24 px.
- [ ] Bilder har beskrivende alt-tekst, eller `alt=""` når de bare er pynt.
- [ ] Bevegelse er slått av ved `prefers-reduced-motion`.
- [ ] `lang` er satt på siden og der språket skifter.
- [ ] Kjernen fungerer uten JavaScript.

## Ytelse

- [ ] Lighthouse i CI med grenser for tilgjengelighet og SEO (stopper) og ytelse og beste praksis (advarer).
- [ ] Ytelsesbudsjett per side for JavaScript, CSS, skrift, bilder, totalt og antall forespørsler til tredjeparter,
      med god margin over dagens nivå.
- [ ] Bilder i moderne formater (AVIF og WebP) med riktige størrelser.
- [ ] Ukentlig sjekk mot produksjon, som fanger opp endringer utenfor repoet.

## Visuelle tester

- [ ] Skjermbilder av de viktigste sidene i begge visninger og på mobil og skrivebord, sammenlignet i CI.
- [ ] Bildene lages og sammenlignes på samme plattform (Linux i CI), fordi skrift tegnes ulikt på ulike systemer.
- [ ] Innhold som endres uten kodeendring (live-status, tall), dekkes til.
- [ ] Nye bilder lages av en manuell kjøring i CI, gjennomgås og sjekkes inn i samme pull request som endringen.
