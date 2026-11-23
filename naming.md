# Navngivning

Identifikatorer er på engelsk. Det som leses av brukere og bidragsytere, er på organisasjonens språk.

| Engelsk | Organisasjonens språk |
|---|---|
| Mapper og filnavn | Innhold som vises for brukere |
| Kode: variabler, funksjoner, typer, komponenter, CSS-klasser, `data-*`-attributter, felt i datamodeller | Dokumentasjon og kommentarer i koden |
| npm-skript, arbeidsflyter, jobbnavn, step-id, input og output | Navn på steg i arbeidsflyter |
| Branch-navn | Commit-meldinger, pull requests og issues |
| Tokens, secrets, variabler, miljøer, bøtter, IaC-moduler og -ressurser | Beskrivelser og kommentarer som sendes til tjenester |

## Unntak

- **Filer som bestemmer offentlige adresser** (for eksempel sider i et rammeverk med filbasert ruting) følger
  språket i adressen.
- **Navn lagret i kryptert eller ekstern tilstand** (for eksempel nøkkelleverandører i OpenTofu-kryptering) beholdes
  til de kan migreres trygt.

## Form

| Hva | Form | Eksempel |
|---|---|---|
| Filer, mapper, npm-skript, arbeidsflyter, jobber, branch-navn | kebab-case | `link-check.yml`, `check-docs.mjs`, `test:e2e` |
| Komponenter og typer | PascalCase | `StatusCard`, `FlightStats` |
| Variabler og funksjoner (JavaScript, TypeScript) | camelCase | `searchPanel`, `formatDate` |
| Variabler og ressurser i OpenTofu | snake_case | `rate_limit`, `dns_records` |
| Secrets og miljøvariabler | SCREAMING_SNAKE_CASE | `CLOUDFLARE_API_TOKEN` |
| Tokens og bøtter hos tjenester | kebab-case med prosjektet først | `<prosjekt>-tofu`, `<prosjekt>-tofu-state` |
| Branch-navn | `<type>/<kort-beskrivelse>` | `docs/guidelines`, `infra/english-names` |

Branch-typer: `feat`, `fix`, `docs`, `ci`, `infra`, `refactor`, `test`, `deps`.
