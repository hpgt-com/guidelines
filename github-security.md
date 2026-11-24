# GitHub-sikkerhet

## Organisasjon og repo

- [ ] Medlemmer har lesetilgang som standard. Skrivetilgang gis per repo til et team, ikke til enkeltpersoner.
- [ ] Tilganger, `CODEOWNERS` og påkrevde godkjenninger i miljøer peker på team (for eksempel
      `@<org>/maintainers`), så nye medlemmer får tilgang ved å legges i teamet.
- [ ] Medlemmer kan ikke lage repoer. Nye repoer lages fra organisasjonens mal av en eier.
- [ ] Actions må være låst til commit-SHA (organisasjonsinnstilling).
- [ ] `GITHUB_TOKEN` har bare lesetilgang som standard, og arbeidsflyter kan ikke godkjenne pull requests.
- [ ] Apper og integrasjoner har bare tilgang til repoene og rettighetene de trenger.
- [ ] Secret scanning med push protection, Dependabot-varsler og sikkerhetsoppdateringer, og privat rapportering av
      sårbarheter er slått på.
- [ ] Offentlige repoer bruker gratis CodeQL og OpenSSF Scorecard.
- [ ] Private repoer uten secret scanning eller code scanning i planen kjører en åpen skanner for hemmeligheter (for
      eksempel gitleaks) som påkrevd sjekk.

## Regelsett for `main`

- [ ] Endringer krever pull request. Force-push og sletting er blokkert.
- [ ] Påkrevde sjekker: bygg eller test, `dependency-review` og `workflow-security` (zizmor).
- [ ] CodeQL-funn med høy alvorlighet eller mer stopper merge.
- [ ] Når en påkrevd jobb får nytt navn, oppdateres regelsettet i samme operasjon som merge.
- [ ] Unntak fra regelsettet gis bare til apper, ikke til personer, og bare når appen ikke kan bruke pull requests
      (for eksempel et innholdsverktøy (CMS) som committer rett til `main`, eller en robot som må pushe). Unntaket og
      grunnen er beskrevet i repoet, med hvilke mapper appen skriver til.
- [ ] Eiere (rollen admin) kan omgå kravene bare gjennom en pull request (`bypass_mode: pull_request`), aldri med
      direkte push. Det trengs for å kunne rette en påkrevd sjekk som selv er ødelagt, og brukes bare da.

## Arbeidsflyter

- [ ] Actions er låst til commit-SHA, med versjonen i en kommentar.
- [ ] `permissions: contents: read` på toppnivå. Jobber som trenger mer, ber om det selv.
- [ ] `actions/checkout` har `persist-credentials: false`. Jobber som må pushe selv, er unntatt, og unntaket er
      beskrevet i repoet.
- [ ] Verdier fra hendelser (titler, branch-navn, kommentarer) brukes via `env`, aldri direkte i `run`.
- [ ] `set -o pipefail` når utdata sendes videre med rør, så feil ikke skjules.
- [ ] Planlagte kjøringer som finner problemer, oppretter én issue og sjekker først om en åpen finnes.

## Avhengigheter

- [ ] Installasjon fra låsefil uten installasjonsskript (for npm: `npm ci --ignore-scripts`).
- [ ] Signaturkontroll av pakkene i bygget (for npm: `npm audit signatures`).
- [ ] Dependabot én gang i måneden, gruppert per økosystem, med `cooldown` på minst 7 dager for patch og minor og
      30 dager for major (`semver-major-days: 30`) der økosystemet støtter det. En versjon foreslås dermed først når den
      har vært ute så lenge. Dependabot støtter ikke `semver-major-days` for blant annet `github-actions` og `opentofu`,
      og avviser hele filen hvis nøkkelen står der.
- [ ] Signaturkontroller som ikke kan kjøres (for eksempel for pakker installert under et alias), er beskrevet i
      repoet med grunnen.

### Automatisk merge

Dependabot-oppdateringer merges automatisk når alle kriteriene er oppfylt:

| Kriterium | Krav |
|---|---|
| Alder | Versjonen har vært ute minst så lenge som `cooldown` sier. |
| Type | Patch eller minor. Major merges manuelt. |
| Pakker | Ikke verktøy som publiserer eller endrer infrastruktur (for eksempel Wrangler og OpenTofu-providere). De merges manuelt. |
| Sjekker | Alle påkrevde sjekker er grønne, også `dependency-review` og signaturkontrollen. |
| Tester | Repoet har tester som fanger brudd på det som publiseres: bygg, ende-til-ende og tilgjengelighet. Uten slike tester merges bare GitHub Actions automatisk. |

- [ ] Sikkerhetsoppdateringer fra Dependabot følger de samme kriteriene, men venter ikke på `cooldown`.
- [ ] Arbeidsflyten slår på automatisk merge (`gh pr merge --auto`). Regelsettet avgjør når PR-en merges.
- [ ] I repoer som publiserer ved push til `main`, slås automatisk merge på med et token fra en GitHub-app. En merge
      med `GITHUB_TOKEN` starter ikke andre arbeidsflyter, så endringen ville ikke blitt publisert. Nøkkelen ligger
      som Dependabot-secret, fordi arbeidsflyter fra Dependabot ikke får vanlige secrets.
- [ ] Arbeidsflyten bruker `pull_request_target` og sjekker ikke ut koden, så den alltid kjører versjonen fra `main`
      og aldri kode fra PR-en.

## Hemmeligheter

- [ ] Ingen hemmeligheter i repoet. `.gitignore` stopper `.env` og nøkkelfiler.
- [ ] Hemmeligheter ligger som secrets, og bare jobbene som trenger dem, får dem.
- [ ] Endringer i produksjon (for eksempel apply av infrastruktur) kjører i et miljø som bare tillater `main`, med
      påkrevd godkjenning fra et team. Der planen ikke støtter godkjenning i miljøet (private repoer uten Enterprise),
      er merge av pull requesten, med planen synlig, godkjenningen.
- [ ] Tokenet som kan endre produksjon, ligger bare i miljøet. Jobber som kjører på branches og pull requests (for
      eksempel `plan`), bruker et token som bare kan lese.
- [ ] Tokens har minst mulig tilgang og utløpsdato, og en planlagt jobb varsler før de utløper.
- [ ] Tjenestetokens eies av organisasjonen eller kontoen, ikke av en person, der tjenesten støtter det.
