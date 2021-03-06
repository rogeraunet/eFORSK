# Skjemasystemet

Skjemasystemet definerer hvordan skjemaer kan lages.
Dette er en videreutvikling av hvordan skjemaer løst i PROMS og MRS (spesielt fleksible felter). Mulighetene kan ikke overgå hva PROMS tilbyr, da skjemaene må støtte oversettelse til PROMS-format.

## Definisjon av skjemaer

### Skjematyper (FormType)
Hvert register kan opprette så mange skjematyper dem ønsker.
*En skjematype kan definere å være tilknyttet en annen skjematype. Et skjema av denne skjematypen sies da å være et tilknyttet skjema av dens hovedskjema. Et hovedskjema må være tilstede for at et tilknyttet skjematype skal kunne opprettes*

**Tabellstruktur**
* Id (int)
* ParentId (int?)
* Name
* Delete (bool)

### Skjemaversjon (FormVersion)
En skjematype har alltid en versjon. Disse gies løpende numre. En skjematype kan kun ha en versjon i status kladd samtidig. Denne må publiseres, og eventuelt tilbaketrekkes, før man kan opprette en ny kladd. En ny versjon er alltid basert på sin forrige versjon. Man kan ha flere samtidige aktive versjoner (ved utfyllelse av et skjema må man da velge hvilken versjon man vil fylle ut). Man har mulighet til å slette en versjon i kladd. En versjon som er publisert kan aldri slettes, men kan trekkes tilbake (avpubliseres).

En skjemaversjon kommer med en skjemadesigner hvor man kan flytte rundt på felter, legge til nye felt, og deprekere eksisterende felt. Man har et felt for å plassere "skjulte felter", hvor metadata ligger som standard.

**Tabellstruktur**
* FormTypeId (int)
* VersionNumber  (int)
* Name
* Status (draft,published,withdrawn,deleted) <- Versjoner kan slettes hvis den er den nyeste versjonen i kladd
* FormDesignJson (nvarchar(max))  <- parent/child og rekkefølger av felter 
* PromsActivated (bool)
* PromsConfigJson (nvarchar(max))
  * PromsPaperActivated (bool) - manuell proms
  * PromsId (guid?)
	* (..)
  
### Felt (FormField)
Et skjemafelt kan kun opprettes **og endres** i en versjon med status kladd. Så fort versjonen er publisert, kan man i etterfølgende versjoner kun velge og deprekere (deaktivere) feltet (for godt). Brukeren kan selv lage et kodenavn (kolonneoverskrift i datadump) for feltet, dette må være unikt for skjematypen. Merk at man aldri kan ta i bruk kodenavnet igjen på andre felt i skjematypen selv om det på et tidspunkt blir deprekert.

**Tabellstruktur**
* UniqueName (nvarchar(16)) <- generert unikt, brukes i kode/database som nøkkel og aldri eksponert for brukeren
* CodeName (nvarchar(50)) <- brukerdefinert kodenavn, kan ikke endres etter at versjonen er ferdigstilt
* DisplayName (nvarchar(255)) <- visningsnavnet for feltet ved utfylling av skjema
* HelpText
* FormTypeId (int)
* FromVersionId (int)
* ToVersionId (int?)
* FieldTypeId (int)
* AutoField (nvarchar(255)) <- om feltet skal tilegnes verdi automatisk ved lagring. feltet inneholder referanse til autofeltets ID, f.eks. PatientAge. Feltet blir da automatisk readonly ved utfyllelse.
* SortIndex (int) <- kun for datadump, feltet har sin egen rekkefølge i FormVersion sin FormDesign. Denne må vurderes.
* Deleted (bool) <- Properties can be deleted from the version they are created on when version is not active yet
* ConfigurationJson (nvarchar(max)) <- spesifikk konfigurasjon for felttypen
	* ..
	
### Skjemaregel (FormRule)
Skjemaregler omfavner både validerings- og vis/skjul-regler. Skjemaregler kan på lik linje med felter opprettes på en versjon, og deprekeres i senere versjoner. 

Regler som støttes er:
* Vis felt hvis _
* Skjul felt hvis _
* Tall: Mindre enn _
* todo..

**Tabellstruktur**
* UniqueName (nvarchar(16)) <- generert unik 
* FormTypeId (int)
* FromVersionId (int)
* ToVersionId (int?)
* RuleType (show,hide,validation-warning,*validation-dismissable*,validation-required)
* FormFieldId1 (int?)
* FormFieldId2 (int?)
* RuleData (nvarchar(max)) <- json med regel-data

### FeltType (FieldType)
En felttype må velges når man oppretter et felt. 

Felttyper som støttes er:
* Tall
* Valgfelt (radio eller nedtrekssliste)
* Avkrysning
* Dato/*tid* - tid støttes ikke i PROMS
* *Tekst* - fases ut fra PROMS?
* *Tekstområde* - fases ut fra PROMS?

**Objektstruktur**
* Id (int)
* Navn
* todo..

### AutoFelt
*Foreløbig på idestadiet.*
Tanken er at man kan legge på felt som automatisk fylles ut/kalkuleres ved opprettelse og lagring, som ikke kan fylles ut eller endres av brukeren, eksempel på dette er:
* Skjema-metadata: Id, MainFormId, FormTypeId, PersonId, FormVersionId, UnitId, Status... (legges til som felt på skjematypen automatisk, men skjult fra grensesntitt ved utfyllelse)
* Pasientalder
* Kjønn
* Postnummer, *Poststed*, Bydelsnr, kommunenr, *kommunenavn* (i forhold til skjemadato), gjeldende kommunenr
* Enhetsnavn, Helseenhetsnavn, Helseenhets-kortnavn, Sykehusnavn, HF, RHF (i forhold til UnitId)
* Randomisert-gruppe
* *Felter fra hovedskjema* (må vurdere hva som gjøres når feltene i hovedskjemaet oppdateres)
* todo..

Autofelt blir tilgjengeliggjort for en bestemt felttype. For eksempel blir pasientalder tilgjengelig for tallfelt.

## Utfyllelse av skjemaer

### Skjemadata (FormData)
Dette er et utfylt skjema

**Tabellstruktur**
* Id (guid)
* MainFormId (guid?)
* IsTest (bool) - om skjemaet tilhører testmodus
* FormTypeId (int)
* PersonId (guid?)
* FormVersionId (int)
* UnitId (int)
* ImportSessionId (int?)
* Status (draft,closed,deleted,returned)
* Created
* CreatedBy
* Updated
* UpdatedBy
* FormDate
* PropertyDataJson (nvarchar(max))
	* key-value liste av UniqueName - verdi

## Testmodus
Man kan i grensesnittet hoppe mellom testmodus og produksjonsmodus av registeret når man vil. Dette for å teste skjemaer, opplæring, demonstrering eller øve seg på utfyllelse. Man vil i testmodus ikke se skjemaer fra prodmodus, og omvendt. Pasientene i testmodus hentes fra testfolkeregisteret.
