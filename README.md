# eFORSK dokumentasjon

Her følger dokumentasjon for utvikling av verktøyet. Brukes som kravspesifikasjon ved utvikling, dokumentasjon og funksjonalitetsoversikt for (potensielle) brukere av verktøyet. Tekst i *skråskrift* i lenkede dokumenter er ennå ikke planlagt implementert i kommende versjon.

## Terminologi og definisjoner

Skal brukes for konsistens i dokumentasjon, kode og grensesnitt. Engelsk brukes i programmeringskode.

* **Register** (Registry) - En egen database i løsningen, der et register eller forskningsprosjekt kan registrere sine data
* **Tilgangsenhet** (AccessUnit) - En egen tilgang innad et register. Tilgangsenhetene er definert i et tilgangstre, som bestemmer hvilke data som kan sees
* **Skjematype** (FormType) - Definisjonen bak et skjema som kan fylles ut i et register. Skjematypene opprettes av registeransvarlige i hvert enkelt register.
* **Skjemaversjon** (FormVersion) - En versjon av skjematypen
* **Felt** (Field) - En felt som kan besvares i et skjema
	* **Kodenavn** (CodeName) - Et felts kodenavn ved datauttrekk (datadump o.l.)
	* **Visningsnavn** (DisplayName) - Overskrift for feltet ved utfyllelse i grensesnitt (brukervennlig forklaring av feltet)
* **Person** - data/skjemaer legges på en person (i et vanlig registers tilfelle: pasient) i registeret. det er valgt å kalle dette person da det nødvendigvis ikke trenger å være en pasient. VURDER DETTE!
* **Mitt skjema** - brukeren som oppretter et skjema er den som eier skjemaet

## Dokumenter:
* [Overordnet arkitektur](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Overordnet%20arkitektur.md)
* [Databasestruktur](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Databasestruktur.md)
* [Roller, tilgangstre og samtykke](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Roller%20og%20tilgangstre.md)
* [Logging og innsyn](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Logging%20og%20innsyn.md)
* [Informasjonsarkitektur & funksjonalitet](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Grensesnitt.md)
* [Skjemasystemet](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Skjemasystemet.md)
* [Datadump & kodebok](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Datadump%20og%20kodebok.md)
* [PROMS integrasjon](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/PROMS%20integrasjon.md)
* [Randomisering](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Randomisering.md) - her trenger vi mer input!
* [Kodemiljø](https://github.com/HemitSystemutvikling/dokumentasjon-forskningsapp/blob/master/Kodemiljø.md) - prosjektoppsett for utviklere

## Prioriteringsliste utvikling

Minimumsliste må-gjøre MRS for forskning
- App skall (Fungerendes applikasjon som kommuniserer med service-lag. Ingen funksjonalitet)
	- Angular front-end
	- WCF
- Databasestruktur
	- Videreutviklet MRS modell
	- Dapper?
- Mulighet for å definere et skjema
	- mulighet for å lage nye versjoner av et skjema
- Mulighet for å fylle ut skjema
- Integrasjon mot PROMS (inkl manuell bestilling,  mottak)
- Datadump (excel, csv)
- Kodebok
- Logging og innsyn
- Randomisering (enkel knapp, tildelt gruppe  A,B,C...)
- Samtykkehåndtering

Bør gjøre
- Optimalt databaselag for løsningen (dapper, dbup, databasetool)
- Tilgangsstyring innad register
- Koblede skjematyper
- Enkel bruksstatistikk
- Standard for utvidelser per enkelt register (rapporter)
- Dokumenthåndtering (proms papir/signerte dokument)
- Administrasjonsgrensesnitt 
	- Opprette nye register
	- Status for registrene, debug info, statistikk

Kjekt å ha
- Notifikasjonssystem
- Async datadump/rapporter
- Datadump spss format
- Import (fihr format?, xml, csv, excel)
- Massebestilling proms
- Administrasjonsgrensesnitt: kopier register (import/eksport definisjoner)
- Administrasjonsgrensesnitt: arkiver/slette register
- Listehåndtering (gjenbruk og felles vedlikehold av lister innad et register, eks.: ja, nei, ukjent. samt standardiserte lister globalt tilgjengelig)
