# Drift som kode

For organisasjoner med flere repoer: innstillingene i GitHub, kontrollene av dem, sertifikatene, utløpsdatoene og
oppskriften for å bygge alt opp på nytt ligger i ett driftsrepo (for eksempel `operations`).

## Driftsrepoet

- [ ] Repoet er privat når planen gir regelsett og miljøer med hemmeligheter for private repoer (for eksempel GitHub
      Team), fordi navn på tokens, utløpsdatoer og oppsettet ikke trenger å være offentlige. På planer der private
      repoer mangler dette, er repoet offentlig, så kontrollen er beskyttet av regelsett og miljø. Valget og at sakene
      fra kontrollen da er offentlige, står som en beslutning i repoet.
- [ ] Ingen hemmeligheter, uansett synlighet: ingen passord, tokens, nøkler eller personopplysninger. Det som
      utløper eller er hemmelig, står bare med navn.
- [ ] README-en har en tabell over filene og arbeidsflytene, og hva hver av dem gjør.
- [ ] Avvik fra malen er beskrevet i README-en. Et privat repo uten GitHub Advanced Security kan ikke bruke CodeQL,
      `dependency-review` eller Scorecard, og kjører en åpen skanner for hemmeligheter (for eksempel gitleaks) i stedet
      (se [github-security.md](github-security.md#organisasjon-og-repo)).

| Fil | Hva |
|---|---|
| `github.json` | Innstillingene for organisasjonen og hvert repo |
| `scripts/apply-settings.mjs` | Viser planen og setter innstillingene fra `github.json` |
| `.github/workflows/audit.yml` | Kontrollerer hver uke at GitHub følger `github.json` |
| `certificates.json`, `.github/workflows/certificate-check.yml` | Kontrollerer sertifikatene for domenene |
| `expiry.json`, `.github/workflows/expiry.yml` | Varsler før tokens, nøkler og domener utløper |
| `infra/` | Infrastruktur for domenene, etter [infrastructure.md](infrastructure.md) |
| `docs/rebuild.md` | Rekkefølgen for å bygge alt opp på nytt |

## Innstillinger som kode

- [ ] Alle innstillingene API-et kan sette, står i én deklarativ fil (for eksempel `github.json`):
  - organisasjonen: tilgang for medlemmer, hvem som kan lage repoer, Actions og `GITHUB_TOKEN`
  - team og apper, med rettighetene og repoene hver app har tilgang til
  - for hvert repo: innstillingene, sikkerhet, Actions, tilgangen for team, regelsettet for `main` med påkrevde
    sjekker og unntak, miljøene med tillatte branches og godkjennere, variablene og Pages
- [ ] Felles verdier står én gang (for eksempel for alle, for offentlige og for private repoer), og hvert repo har bare
      det som skiller seg ut.
- [ ] Hemmeligheter står bare med navn. Variabler står med verdi, fordi de ikke er hemmelige.
- [ ] Et skript setter innstillingene. Uten valg viser det bare planen. Endringer gjøres først med `--apply`, og noe
      slettes bare med `--prune` i tillegg.
- [ ] Skriptet kjøres lokalt av en eier av organisasjonen. Planen skal være tom etter `--apply`.
- [ ] Det API-et ikke kan sette, står i fila (for eksempel under `manual`) og vises av skriptet: tofaktor, medlemmer i
      team, installasjon av apper, verdiene til hemmeligheter, verifiserte domener, synlighet og nye repoer.
- [ ] Skriptet og den ukentlige kontrollen bruker samme kode for lesing og sammenligning, så de alltid er enige om hva
      som er avvik.

Endringer gjøres slik:

| Endring | Slik |
|---|---|
| Nytt repo | En eier lager repoet fra malen. Repoet legges til i fila i en pull request, og etter merge kjøres skriptet med `--apply`. |
| Ny innstilling | Fila endres i en pull request, og etter merge kjøres skriptet med `--apply`. |
| Endring i nettleseren | Meldes av kontrollen, og overskrives ved neste `--apply` hvis fila ikke er oppdatert. |

## Ukentlig kontroll

- [ ] En planlagt arbeidsflyt sammenligner hver uke organisasjonen og hvert repo med fila. Et repo som ikke står i
      fila, meldes også.
- [ ] Kontrollen leser med et token fra en GitHub-app med bare lesetilgang (Administration, Organization
      administration og Members), installert på alle repoene, så nye repoer også kontrolleres.
- [ ] Nøkkelen til appen ligger i et miljø som bare tillater `main`. Client ID ligger som variabel, og jobben hoppes
      over så lenge den ikke er satt.
- [ ] Avvikene samles i én issue, «Avvik i grunnoppsettet», som oppdateres hver uke og lukkes av seg selv når alt er i
      orden. Issuen lages med `GITHUB_TOKEN`.
- [ ] Det appen med vilje ikke kan lese (hemmeligheter, variabler, Pages), står som merknad i issuen og kontrolleres når
      en eier kjører skriptet.
- [ ] På pull requests kontrolleres bare fila og skriptene, med tester, uten nøkkel og uten nettverk.

## Sertifikater

- [ ] En fil (for eksempel `certificates.json`) lister de ventede utstederne, de samme som i CAA, og navnene
      sertifikatene kan gjelde.
- [ ] En planlagt arbeidsflyt leser hver dag sertifikatene for domenet i Certificate Transparency-loggene (for eksempel
      via Cert Spotter).
- [ ] Et sertifikat fra en ukjent utsteder eller for et ukjent navn gir én issue per sertifikat, med en del av
      sjekksummen i tittelen, så samme sertifikat ikke meldes to ganger. Vanlige fornyelser gir ingen issue.
- [ ] Feiler oppslaget, kommenteres det på én egen issue, som lukkes av seg selv når kontrollen går igjen.
- [ ] Arbeidsflyten bruker bare `GITHUB_TOKEN`, uten nøkler.

## Utløpsdatoer

- [ ] En fil (for eksempel `expiry.json`) lister alt som utløper: tokens, nøkler og domener. For hvert element står
      navnet, hvor det brukes, hvordan det fornyes og utløpsdatoen, aldri verdien. Det som ikke utløper, har `null`
      og en merknad.
- [ ] En planlagt arbeidsflyt lager hver uke én issue per element som utløper snart (for eksempel innen 45 dager, og
      90 for domener) eller har utløpt. Issuen lukkes av seg selv når datoen er flyttet fram i fila.
- [ ] En sjekk kontrollerer at fila er gyldig. Er sjekken påkrevd, kjører den på alle pull requests, så pull requests
      som ikke endrer fila (for eksempel fra Dependabot), ikke blir stående.
- [ ] Når noe er fornyet, settes den nye datoen i fila i en pull request.

## Gjenoppbygging

- [ ] `docs/rebuild.md` beskriver rekkefølgen for å bygge opp organisasjonen, infrastrukturen og tjenestene rundt fra
      bunnen, for eksempel etter at en konto er tapt.
- [ ] Dokumentet starter med en tabell over delene, hvor koden for hver del ligger, og hva den settes med.
- [ ] Det som må gjøres for hånd, er merket **manuelt**, med grunnen: kontoene, installasjon av apper, verdiene til
      hemmeligheter, navnetjenere og DS-poster hos registraren.
- [ ] Hemmelighetene står i en tabell med repo, hvor de ligger (repo, miljø eller Dependabot), navn og kilde, aldri
      verdier.
- [ ] Det som ikke kan lages på nytt (for eksempel passfrasen som krypterer state), lagres i en passordbehandler.
      Tokens og app-nøkler lagres ikke andre steder enn der de brukes: trengs en verdi på nytt, rulles tokenet, eller
      det lages en ny nøkkel. Dokumentet sier hvordan for hver av dem.
- [ ] Dokumentet slutter med en kontroll: planen for innstillingene er tom, kontrollene går grønt, tjenestene svarer, og
      nye utløpsdatoer står i fila.
