# Kodeagenter

Instruksjoner til kodeagenter skal virke uansett verktøy.

| Fil | Innhold | Leses av |
|---|---|---|
| `AGENTS.md` | Prosjektet, kommandoer og regler, med lenker til retningslinjene | Codex, GitHub Copilot, Cursor, Gemini CLI og flere |
| `CLAUDE.md` | Én linje: `@AGENTS.md` | Claude Code |
| `docs/guidelines/` | Regler som bare gjelder repoet | Mennesker og alle verktøy, via lenker i `AGENTS.md` |

Mal: [templates/AGENTS.md](templates/AGENTS.md).

## Regler

- `AGENTS.md` holdes kort: hva repoet er, de viktigste kommandoene og reglene som alltid gjelder. Detaljer står i
  retningslinjene den lenker til.
- Kunnskap legges ikke i verktøyspesifikke mapper (for eksempel `.claude/`, `.cursor/`). Slike mapper er lokale og
  står i `.gitignore`.
- Ingen andre filer i repoet bør hete `agents.md` med små bokstaver. Filsystemer som ikke skiller mellom store og små
  bokstaver (Windows, macOS), blander dem sammen med `AGENTS.md`.
- Agenter følger de samme reglene som mennesker: pull request, grønne sjekker, ingen hemmeligheter, og dokumentasjon
  oppdatert i samme endring.
- Agenter får ikke tilgang til produksjon utover det arbeidsflytene gir. Endringer i infrastruktur går via plan og
  godkjent apply.
