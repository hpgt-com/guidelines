# Dokumentasjon

Dokumentasjonen beskriver hvordan ting er. Den oppdateres i samme pull request som endringen den gjelder.

## Struktur i et repo

| Fil | Innhold |
|---|---|
| `README.md` | Hva repoet er, kom i gang, kommandoer, mappestruktur og lenker videre |
| `AGENTS.md` | Instruksjoner for kodeagenter (se [coding-agents.md](coding-agents.md)) |
| `SECURITY.md` | Rapportering av sårbarheter |
| `docs/architecture.md` | Komponenter, dataflyt og beslutninger (se [architecture.md](architecture.md)) |
| `docs/decisions/` | Beslutningsnotater, når det trengs |
| `docs/security.md` | Sikkerhetsmodell, tilganger og nøkler |
| `docs/operations.md` | Publisering, arbeidsflyter, overvåking og fremgangsmåter |
| `docs/guidelines/` | Regler som bare gjelder repoet |

Mindre repoer kan samle dette i `README.md`.

## Fremgangsmåte

1. Se hva som er endret i diffen.
2. Finn dokumentene som berøres, og les dem i sin helhet.
3. Kontroller hver påstand mot koden eller innstillingene, ikke mot hukommelsen.
4. Oppdater teksten etter skrivereglene under.
5. Kjør kontrollen av dokumentasjonen, hvis repoet har en.

## Skriveregler

- Beskriv hvordan det er, ikke hva som er gjort. Ingen datoer for når noe ble gjort, og ingen «nå», «nylig» eller
  «gjenstår». Historikken ligger i git.
- Nøytral form: «Kjør …», «Tokenet lages i …», ikke «du må» eller «jeg har». Bruk «eier» eller rollen om personen
  som har tilgangene.
- Ingen private opplysninger: private e-postadresser, lokale filstier, navn på personer utenfor prosjektet,
  tokenverdier eller hemmeligheter.
- Ingen omtale av tidligere løsninger eller leverandører som ikke lenger er i bruk.
- Stier, kommandoer, filnavn og verdier i `kode`.
- Korte avsnitt og tabeller. Én ting beskrives ett sted, og andre steder lenker dit.
- Diagrammer skrives i Mermaid (` ```mermaid `), som GitHub viser direkte. Bruk `flowchart` for dataflyt og
  `sequenceDiagram` for forløp. Etiketter på organisasjonens språk, node-id på engelsk. Under hvert diagram står en
  kort tekst som sier det samme, for steder der Mermaid ikke vises.

## Automatisk kontroll

Repoer med mye dokumentasjon bør ha en kontroll i bygget som stopper når dokumentasjonen nevner en sti, lenke,
overskrift, kommando eller arbeidsflyt som ikke finnes, og når en arbeidsflyt ikke er dokumentert. Kontrollen kan
ikke se om en beskrivelse er feil. Det dekkes av fremgangsmåten over.
