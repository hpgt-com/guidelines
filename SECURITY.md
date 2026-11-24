# Sikkerhet

## Rapportere et sikkerhetsproblem

Ikke opprett en offentlig issue. Bruk **Report a vulnerability** under fanen **Security** i dette repoet, så
bare vedlikeholderne ser rapporten. Beskriv hva du fant, hvordan det kan gjenskapes, og hva det påvirker. Vi svarer
så fort vi kan, men dette er et frivillig klubbprosjekt.

Rapporten kan skrives på norsk eller engelsk.

## Dette er på plass

- Alle endringer på `main` går via pull request (regelsettet «Main protection»). Force push og sletting av `main` er
  blokkert.
- Påkrevde sjekker: `dependency-review` og `workflow-security` (zizmor), pluss kodeskanning med CodeQL av
  arbeidsflytene (high eller høyere stopper merge).
- Actions er låst til commit-SHA og oppdateres månedlig av Dependabot, med en ventetid på 7 dager (30 for hovedversjoner
  der økosystemet støtter det). Patch- og minor-oppdateringer av Actions merges automatisk når alle påkrevde sjekker er
  grønne (`dependabot-automerge.yml`). Andre oppdateringer merges for hånd.
- Arbeidsflyter har bare lesetilgang som standard, med `persist-credentials: false` på hver checkout.
- Repoet inneholder bare Markdown. Det har ingen hemmeligheter, miljøer eller publisering.
