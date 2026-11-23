# Infrastruktur som kode

Med OpenTofu. Cloudflare brukes som eksempel, men oppsettet passer for andre leverandører.

## Struktur

```
infra/
  modules/<module>/        gjenbrukbar modul, alt kommer inn som variabler
  <environment-or-zone>/   én mappe per sone eller miljø
    terraform.tfvars       verdiene, uten hemmeligheter
    versions.tf            provider, backend og kryptering
    main.tf                kaller modulen
    variables.tf
    .terraform.lock.hcl    sjekksummer, sjekkes inn
```

## Regler

- **OpenTofu**, med låst versjon i arbeidsflyten og låst provider i `versions.tf`.
- **State** ligger i en privat bøtte hos leverandøren (for eksempel R2), med låsing (`use_lockfile`) og kryptering
  i OpenTofu (`encryption` med `pbkdf2` og `aes_gcm`, `enforced = true`). State legges aldri i git.
- **Ingen hemmeligheter i koden.** Tokens og passfrase kommer fra secrets via miljøvariabler.
- **Nøkler lages manuelt.** Koden skal ikke kunne lage sine egne tokens, fordi et token som kan lage tokens, gir
  nesten full tilgang. Tokens for OpenTofu har minst mulig tilgang, er knyttet til én sone der det går, og har
  utløpsdato.
- **Eksisterende ressurser** kobles til koden med en midlertidig `importer.tf`. Første plan skal bare vise import.
- **Nye navn** på moduler og ressurser flyttes med `moved`-blokker. Planen skal bare vise flytting.
- **Endringer i dashbordet** er ikke tillatt. De oppdages av avvikssjekken og overskrives ved neste apply.

## Arbeidsflyt

| Jobb | Når | Hva |
|---|---|---|
| `check` | Alle endringer i `infra/` | `tofu fmt -check` og `tofu validate`, uten nøkler |
| `plan` | Pull requests, før apply og ukentlig | `tofu plan -detailed-exitcode` med et token som bare kan lese, med resultatet i oppsummeringen |
| `apply` | Merge til `main`, bare når planen har endringer | Kjører i et miljø som bare tillater `main`, med skrivetokenet. Krever godkjenning i miljøet der planen støtter det, ellers er merge med synlig plan godkjenningen |
| `drift` | Ukentlig | Oppretter én issue når planen viser endringer |

- Plan og apply kjører bare når en repovariabel (for eksempel `TOFU_ENABLED`) er satt, så repoet kan settes opp før
  nøklene finnes.
- `-parallelism` settes lavt nok til at leverandørens API ikke svarer med rate limiting.
- `set -o pipefail` når planen sendes videre med `tee`.
- Dependabot (`package-ecosystem: opentofu`) foreslår nye versjoner av provideren, og planen på pull requesten viser
  om en ny versjon endrer noe.

## Utenfor koden

Beskriv i repoet hva som med vilje står utenfor koden, og hvorfor. Vanlige grunner er at provideren ikke støtter
ressursen, at verdien er privat, eller at ressursen styres av et annet verktøy.
