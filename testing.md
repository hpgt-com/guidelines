# Testing

Hva som skal testes, hvordan, og når testene stopper en endring.

## Lag

| Lag | Hva | Verktøy (eksempler) | Kjører |
|---|---|---|---|
| Statisk kontroll | Typer, lint, formatering, skjema for innhold og oppsett | `tsc`/`astro check`, `tofu validate`, zizmor | Hvert bygg |
| Bygg-kontroller | Ingen innebygd kode, gyldige filer (`security.txt`), dokumentasjonen peker på ting som finnes | Egne skript | Hvert bygg |
| Enhetstester | Ren logikk uten nettleser eller nettverk (tolking, utregning, formatering) | Node-testkjøreren, Vitest | Hver pull request |
| Ende-til-ende | Sidene i en ekte nettleser, med de samme sikkerhetshodene som i drift | Playwright | Hver pull request |
| Tilgjengelighet | WCAG 2.2 AA på alle sider, begge visninger, mobil og skrivebord | axe i Playwright | Hver pull request |
| Visuelle tester | Skjermbilder av de viktigste sidene | Playwright | Hver pull request (Linux) |
| Ytelse | Poenggrenser og ytelsesbudsjett | Lighthouse CI | Hver pull request |
| Endepunkter | Metoder, statuskoder, størrelsesgrenser og at svar ikke lekker data | Playwright `request` eller enhetstester | Hver pull request |
| Infrastruktur | Planen viser bare det som er ment | `tofu plan` | Pull requests i `infra/` |
| Produksjon | De samme testene mot driftsmiljøet | Planlagt kjøring | Ukentlig |
| Lenker | Interne lenker på pull request, eksterne ukentlig | lychee | Pull requests og ukentlig |

## Regler

- **Ny funksjon, ny test.** Et nytt endepunkt, skript i nettleseren eller ny logikk får tester i samme pull request.
- **Feil får en test.** En rettet feil får en test som ville ha fanget den.
- **Testene kjører mot det som publiseres.** Ende-til-ende-testene bruker det ferdige bygget med de samme hodene som i
  drift, ikke utviklingsserveren.
- **Ingen avhengighet av eksterne tjenester i testene.** Svar fra tredjeparter erstattes med faste data, eller så
  tåler testen at tjenesten er nede.
- **Ustabile tester rettes.** En test som av og til feiler, rettes eller fjernes. Den hoppes ikke over i stillhet, og
  CI kjører uten automatiske nye forsøk.
- **Tester som hoppes over, har en grunn** i koden (for eksempel plattform eller miljø).
- **Påkrevde sjekker.** Bygget (med statisk kontroll og bygg-kontroller) er påkrevd i regelsettet. Tester som tar lang
  tid, kan være ikke-påkrevde, men en rød test merges ikke.
- **Lokalt først.** Kommandoene for å kjøre testene lokalt står i `AGENTS.md` og `README.md`.

## Gjennomgang

- [ ] Det nye eller endrede er dekket av tester på riktig lag.
- [ ] Testene feiler uten endringen og passerer med den.
- [ ] Ingen nye tester som hoppes over uten grunn, og ingen avhengighet av eksterne tjenester.
- [ ] Skjermbilder og grenser er bare endret med vilje, og endringen er forklart i pull requesten.
