# Arkitektur

Regler for hvordan løsninger settes sammen, og hvordan arkitekturen holdes beskrevet og etterprøvbar.

## Prinsipper

- **Enklest mulig drift.** Statiske filer og serverløse funksjoner fremfor servere. Ingen database, innlogging eller
  adminpanel før det trengs.
- **Minst mulig angrepsflate.** Hver komponent har bare de tilgangene og adressene den trenger.
- **Alt som kode.** Applikasjon, oppsett, sikkerhetshoder, infrastruktur og arbeidsflyter ligger i repoet. Det som
  ligger i `main`, er det som er i drift.
- **Uavhengig av tredjeparter der det går.** Skrift, ikoner og skript ligger i repoet. Tredjeparter lastes først når
  brukeren ber om det.
- **Personvern som standard.** Ingen sporing, informasjonskapsler eller personopplysninger uten at det er nødvendig og
  beskrevet.
- **Fungerer uten JavaScript.** Kjernen er HTML. Skript forbedrer, men er ikke påkrevd.
- **Utskiftbare deler.** Tjenester bak egne grensesnitt (for eksempel et eget endepunkt foran en ekstern API), så en
  leverandør kan byttes uten å endre resten.

## Beskrivelse

Hvert repo har `docs/architecture.md` med:

| Del | Innhold |
|---|---|
| Oversikt | Tabell med område, valg og begrunnelse |
| Dataflyt | Diagram i Mermaid (`flowchart`): hvem snakker med hvem, og hva som går hvor. Ett diagram for trafikk og ett for endringer (PR, CI, publisering). |
| Komponenter | Hver del med ansvar, adresser og tilganger |
| Beslutninger | Viktige valg med alternativer som ble vurdert, og hvorfor de ble valgt bort |

Større beslutninger kan skrives som egne beslutningsnotater i `docs/decisions/` med filnavn
`NNNN-short-title.md`: kontekst, beslutning, alternativer og konsekvenser. Et notat endres ikke etter at det er
vedtatt. Det erstattes av et nytt som viser til det gamle.

## Gjennomgang

Gå gjennom punktene ved endringer som legger til, fjerner eller bytter en komponent, tjeneste, avhengighet eller
dataflyt.

- [ ] Endringen følger prinsippene over, eller avviket er begrunnet i beslutningene.
- [ ] `docs/architecture.md` er oppdatert: oversikt, dataflyt, komponenter og eventuelt ny beslutning.
- [ ] Nye tjenester og tredjeparter er beskrevet i sikkerhetsdokumentasjonen og personvernerklæringen.
- [ ] Nye endepunkter følger [web-security.md](web-security.md).
- [ ] Ny infrastruktur er kode og følger [infrastructure.md](infrastructure.md).
- [ ] Nye avhengigheter er nødvendige, vedlikeholdte, har kjent opphav og passer i ytelsesbudsjettet.
- [ ] Det finnes tester for det nye (se [testing.md](testing.md)).
- [ ] Det er klart hvordan endringen rulles tilbake.
