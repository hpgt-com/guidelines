# Retningslinjer

Felles regler og sjekklister for repoene i hpgt-com, for både mennesker og kodeagenter. Retningslinjene er skrevet
generelt, uten navn på bestemte repoer, domener eller tjenester. Det som gjelder ett repo, står i det repoet.

| Retningslinje | Gjelder |
|---|---|
| [naming.md](naming.md) | Navn på filer, mapper, kode, arbeidsflyter, tokens og ressurser |
| [architecture.md](architecture.md) | Prinsipper, beskrivelse og gjennomgang av arkitekturen |
| [testing.md](testing.md) | Testlag, regler og gjennomgang av tester |
| [documentation.md](documentation.md) | Hvordan dokumentasjon skrives og holdes oppdatert |
| [github-security.md](github-security.md) | Tilganger, regelsett, GitHub Actions, avhengigheter og hemmeligheter |
| [web-security.md](web-security.md) | Sikkerhetshoder, CSP, Trusted Types, endepunkter, skjemaer og domener |
| [operations.md](operations.md) | Drift som kode: innstillinger, ukentlig kontroll, sertifikater, utløpsdatoer og gjenoppbygging |
| [infrastructure.md](infrastructure.md) | Infrastruktur som kode med OpenTofu, med Cloudflare som eksempel |
| [accessibility-ux.md](accessibility-ux.md) | Tilgjengelighet, visning, ytelse og visuelle tester |
| [coding-agents.md](coding-agents.md) | Oppsett for kodeagenter: `AGENTS.md`, `CLAUDE.md` og lokale retningslinjer |

## Ta i bruk i et repo

1. Kopier [templates/AGENTS.md](templates/AGENTS.md) til roten av repoet og fyll inn prosjektet og kommandoene.
2. Legg til `CLAUDE.md` med én linje: `@AGENTS.md`.
3. Legg regler som bare gjelder repoet, i `docs/guidelines/` i repoet. Lenk til retningslinjene her for resten, i
   stedet for å kopiere dem.
4. Følg [github-security.md](github-security.md) for tilganger, regelsett og arbeidsflyter.

Nye repoer lages fra organisasjonens mal (et template-repo som følger retningslinjene) av en eier.

## Grunnlaget

Retningslinjene er en praktisk utgave av kjente fremgangsmåter, ikke regler fra én organisasjon eller ett prosjekt:

| Fremgangsmåte | Hva det betyr her | Retningslinjer |
|---|---|---|
| Infrastruktur som kode (IaC) | DNS, soneinnstillinger og regler står som kode, og endres bare der | [infrastructure.md](infrastructure.md) |
| GitOps | Git er fasiten. Alt endres via pull request med grønne sjekker, og avvik fra koden meldes | [github-security.md](github-security.md), [operations.md](operations.md) |
| DevSecOps | Sikkerhet er en del av hver endring: låste avhengigheter, minste tilgang, kodeskanning og strenge hoder | [github-security.md](github-security.md), [web-security.md](web-security.md) |
| Policy som kode | Regelsett, påkrevde sjekker og en jevnlig kontroll håndhever reglene | [github-security.md](github-security.md), [operations.md](operations.md) |
| Dokumentasjon som kode | Dokumentasjonen ligger i repoet, oppdateres i samme pull request og kontrolleres | [documentation.md](documentation.md) |
| Testing og tilgjengelighet fra start | Tester, ytelsesbudsjett og tilgjengelighet sjekkes før merge | [testing.md](testing.md), [accessibility-ux.md](accessibility-ux.md) |

## Prinsipper

- **Uavhengig av verktøy.** Retningslinjene er vanlig Markdown. Verktøyspesifikke filer er bare tynne pekere hit.
- **Generelt her, spesielt lokalt.** Det som gjelder ett repo (stier, domener, tjenester), står i repoet.
- **Avvik står i repoet.** Et repo som med vilje ikke følger en regel, beskriver avviket og grunnen i sin egen
  dokumentasjon. Passer en regel dårlig for flere repoer, endres regelen her.
- **Kontrollerbart.** Der det går, håndheves reglene av regelsett, arbeidsflyter eller tester, ikke bare av tekst.

## Endringer

Lenkesjekken (`.github/workflows/link-check.yml`) stopper pull requests med brutte lenker eller ankere mellom
retningslinjene. Eksterne lenker sjekkes hver mandag.

Endringer går via pull request. Repoer som lenker hit, følger alltid siste versjon på `main`. En endring som krever
tiltak i repoene, beskrives i pull requesten.
