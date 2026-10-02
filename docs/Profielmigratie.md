---
layout: page-with-side-nav
title: Profielmigratie
---
# 5.4 Modelmigratie naar een ander MIM-profiel

Generiek stappenplan: migratie van een willekeurig profiel naar VNGR MIM 1-2 Grouping NL

## Doel en reikwijdte
Dit stappenplan beschrijft hoe een Enterprise Architect-model (of een package daarin) wordt gemigreerd van een willekeurig bronprofiel naar het doelprofiel **VNGR MIM 1-2 Grouping NL**. Het is afgeleid van de RSGB-migratie (VNGR_SIM_Grouping_NL → VNGR MIM 1.2 Grouping NL) en gebruikt dezelfde scripts:

- `Migratie_inventarisatie_VNGR_SIM_naar_VNGRMIM12.js` — inventarisatie en tag-opschoning (stap 0 en 1);
- `Migratie_eindopschoning.js` (v6.7 of later) — herkoppelen van stereotypes en tags (stap 3).

De scriptnamen en enkele variabelenamen (zoals `CONFIRMED_VNGR_SIM_REMOVED`) zijn historisch; ze werken voor elk bronprofiel zodra het configuratieblok hieronder is ingevuld en overgenomen in de scripts.

## 1. Configuratieblok (per migratie invullen)

**!!!!!!!!!!!! OPM. ROBERT: Persoonlijk zou ik deze configuratie in een bijlage plaatsen. Dus eerst het proces uitleggen en dan de procedure stap voor stap beschrijven en daarin verwijzen naar de bijlage !!!!!!!!!!!!!!!!**

De configuratie staat bovenin elk script. Hieronder per script de variabelen die per migratie moeten worden ingevuld of gecontroleerd. De twee scripts gebruiken voor de doelprofielnaam elk een eigen variabele (`TARGET_TECHNOLOGY_QUALIFIER_FOR_VERIFICATION` en `TARGET_PROFILE_NAME`); die moeten dezelfde waarde hebben. Paden staan in de voorbeelden zoals ze in het script worden geschreven (met dubbele backslashes).

### 1a. Inventarisatiescript (`Migratie_inventarisatie_VNGR_SIM_naar_VNGRMIM12.js`)

| Variabele | Betekenis | Waar vind je het | Voorbeeld (RSGB) |
|---|---|---|---|
| `SOURCE_TECHNOLOGY_ID` | Id van de brontechnology | MDG-bestand: `<MDG.Technology><Documentation id="…">`, of *Specialize > Technologies > Manage-Tech* **&lt;-- OPM. ROBERT: Dit laatste maakt het voor mij al niet duidelijk wat je dan moet gebruiken (laat staan voor iemand die wat minder diep in EA zit. Ik vermoed de vet gedrukte titel in het menu rechtsboven.** | `MIGNL` |
| `SOURCE_TECHNOLOGY_QUALIFIER_NAME` | Naam van het bron-UML-profiel, zoals die in de FQNames van het model staat **&lt;-- OPM. ROBERT: Is 'De namespace waarin de stereotypes en tagged values in het bron-UML-profiel staan, dus zoals die in de FQNames van het model (voor de dubbele punt) staat' niet duidelijker?** | MDG-bestand: `<UMLProfile><Documentation name="…">`, en controleren met de telling in §3.1 | `VNGR SIM+Grouping NL` |
| `SOURCE_PROFILE_XML_FALLBACK` | Pad naar het MDG-/profielbestand van de bron | eigen bestandsbeheer | `C:\\temp\\VNGR_SIM_Grouping_NL-1_0-1_67_1_ea-toolbox.xml` |
| `SOURCE_LABEL` | Korte naam voor rapporten en logs (alleen weergave) | vrij te kiezen | `VNGR_SIM_Grouping_NL` |
| `TARGET_TECHNOLOGY_ID` | Id van de doeltechnology (vast) | — | `VNGRMIM1-2NL` |
| `TARGET_TECHNOLOGY_QUALIFIER_FOR_VERIFICATION` | Naam van het doel-UML-profiel, het FQName-voorvoegsel (vast) **&lt;-- OPM. ROBERT: Is 'De namespace van de stereotypes en tagged values in het doel-UML-profiel en waarin deze terecht moeten komen, zoals die in de FQNames van het model (voor de dubbele punt) moet komen te staan' niet duidelijker?** | — | `VNGR MIM 1-2 Grouping NL` |
| `TARGET_PROFILE_XML_FALLBACK` | Pad naar het doel-MDG-bestand | het doel-MDG-bestand of een kopie daarvan | `C:\\temp\\VNGR_MIM_1.2_Grouping_NL_ea-toolbox.xml` |
| `TARGET_LABEL` | Korte naam voor rapporten (alleen weergave) | vrij te kiezen | `VNGR_MIM_1.2_Grouping_NL` |
| `ROOT_PACKAGE_GUID` | GUID van de package die gemigreerd wordt | eigenschappen van de package in EA | `{…}` **&lt;-- OPM. ROBERT: Mij is niet duidelijk wat er hier nu precies wordt verwacht maar misschien wordt dat verderop duidelijk. In dat geval beter om hier naar die stap te verwijzen.** |
| `OUTPUT_HTML_PATH` | Pad van het rapport; per migratie een eigen naam | vrij te kiezen | `C:\\temp\\Migratie_inventarisatie_VNGR_SIM_naar_VNGRMIM12.html` |
| `APPLY_CHANGES` | `false` = dry-run, `true` = wijzigingen doorvoeren | — | `false` |

### 1b. Eindopschoning (`Migratie_eindopschoning.js`)

| Variabele | Betekenis | Voorbeeld (RSGB) |
|---|---|---|
| `ROOT_PACKAGE_GUID` | GUID van de package **in het nieuwe project** (na stap 2) | `{…}` **&lt;-- OPM. ROBERT: Mij is niet duidelijk wat er hier nu precies wordt verwacht maar misschien wordt dat verderop duidelijk. In dat geval beter om hier naar die stap te verwijzen.** |
| `TARGET_TECHNOLOGY_ID` | Id van de doeltechnology (vast) | `VNGRMIM1-2NL` |
| `TARGET_PROFILE_NAME` | Naam van het doel-UML-profiel, het FQName-voorvoegsel (vast) **&lt;-- OPM. ROBERT: Is 'De namespace van de stereotypes en tagged values in het doel-UML-profiel en waarin deze terecht moeten komen, zoals die in de FQNames van het model (voor de dubbele punt) moet komen te staan' niet duidelijker?** | `VNGR MIM 1-2 Grouping NL` |
| `TARGET_PROFILE_XML_FALLBACK` | Pad naar het doel-MDG-bestand | `file:///C:/Users/…/MDGTechnologies/VNGR%20MIM%201.2%20Grouping%20NL.ea-toolbox.xml` **&lt;-- OPM. ROBERT: Klopt dit pad wel? Moeten er geen dubbele backslashes gebruikt worden?** |
| `TARGET_LABEL` | Korte naam voor rapporten (alleen weergave) | `VNGR_MIM_1.2_Grouping_NL` |
| `APPLY_CHANGES` | `false` = dry-run, `true` = wijzigingen doorvoeren | `false` |
| `CONFIRMED_VNGR_SIM_REMOVED` | `true` als het **bronprofiel** volledig verwijderd is (naam is historisch, geldt voor elk bronprofiel) | `true` |
| `STEREOTYPE_REMAP` | Stereotypes zonder equivalent → doelstereotype (gelijk aan die in het inventarisatiescript) | `{}` **&lt;-- OPM. ROBERT: Mij is niet duidelijk wat er hier nu precies wordt verwacht maar misschien wordt dat verderop duidelijk. In dat geval beter om hier naar die stap te verwijzen.** |
| `LINK_ENUM_LITERALS`, `ENUM_LITERAL_STEREOTYPE`, `ENUM_OWNER_STEREOTYPES` | Koppelen van enumeratiewaarden zonder stereotype | `true`, `"Enumeratiewaarde"`, `["Enumeratie"]` |

> **Let op:** EA gebruikt in FQNames de **profielnaam** (`VNGR MIM 1-2 Grouping NL`, met streepje), niet de technologynaam (`VNGR MIM 1.2 Grouping NL`, met punt) of het id. Het juiste formaat is `VNGR MIM 1-2 Grouping NL::Objecttype`. Een toekenning met `VNGR MIM 1.2 Grouping NL::…` is dus niet correct al zal EA het niet rapporteren.

### 1c. Gegevens voor de handmatige stap 2 (niet in de scripts)

Voor het vervangen van de FQNames in de XMI zijn nog drie gegevens nodig die in geen van de scripts staan:

- **de locatie van het geëxporteerde XMI-bestand** (bijv. `C:\temp\SIM_RSGB.xml`);
- **de codering** uit de eerste regel van dat bestand (`encoding="…"`, bij RSGB `windows-1252`) **&lt;-- OPM. ROBERT: Hoe ben jij er achter gekomen dat dit voor het RSGB de codering is en hoe komen anderen er achter welke encoding gebruikt moet worden?**;
- **overige FQName-voorvoegsels**: profielnamen naast `SOURCE_TECHNOLOGY_QUALIFIER_NAME` die in het model voorkomen en ook naar het doelprofiel moeten (restanten van eerdere pogingen of een tweede bronprofiel; bij RSGB `VNGR MIM 1.2 Grouping NL`). Deze volgen uit de telling in §3.1. **&lt;-- Checken of de uitleg daar meer duidelijkheid schept.**

Model-specifieke mappings (`VALUE_REMAP`, `TAG_REMAP`, `TAG_EXCLUDE_FROM_REMOVAL`, `CUSTOM_TAG_ACTIONS`, `STEREOTYPE_REMAP`) horen **per migratie opnieuw** te worden bepaald; neem de RSGB-inhoud niet ongezien over. **&lt;-- OPM. ROBERT: Ik neem aan dat dit verderop beter wordt uitgelegd.**

## 2. Voorwaarden

- Het doel-MDG-bestand staat in de MDGTechnologies-map en `VNGRMIM1-2NL` is in *Manage-Tech* aangevinkt.
- Het bronprofiel is (nog) beschikbaar in EA voor stap 0 en 1 maar is in *Manage-Tech* niet meer aangevinkt.
- Er is een backup van het project; bij versiebeheer is de uitgangsrevisie genoteerd. **&lt;-- OPM. ROBERT: Bedoel je niet dat er een backup is van het XMI bestand? Het project omvat n.m.m. de gehele '&lt;project> KING: SIM' folder.**
- Geen openstaande, niet-ingecheckte wijzigingen in de betrokken packages. **&lt;-- OPM. ROBERT: Je schrijft hier packages (in meervoud). Worden er naast het package dat we omzetten ook andere packages geraakt?**

## 3. Vooronderzoek (eenmalig per model)

### 3.1 Welke profielen staan er in het model?
Controleer welk profiel gebruikt is bij het opstellen van het model en welke Stereotypes daar bij horen. Controleer tevens welke tagged values er bij die stereotypes zijn gebruikt en of er "profielloze" tagged values zijn gebruikt, tagged values dus die handmatig zijn aangemaakt. **&lt;-- Dit zou nog wel eens een stap zijn die veel tijd vergt. Zeker als je veel modellen om te zetten hebt. Kunnen we dit makkelijker maken m.b.v. een script?**

Vergelijk de stereotypenamen (het deel na `::`) met de lijst in de bijlage. Voor elk stereotype dat in het doelprofiel **niet** bestaat, een doelstereotype kiezen en opnemen in `STEREOTYPE_REMAP` (in beide scripts) als die te remappen is. Let ook op het metatype: het doelstereotype moet gelden voor hetzelfde soort modelelement (zie kolom *Geldt voor* in de bijlage).

### 3.2 Enumeratiewaarden
Controleer of de waarden van enumeraties in het bronmodel een stereotype hebben. Zo niet, dan koppelt de eindopschoning ze aan `Enumeratiewaarde` (`LINK_ENUM_LITERALS = true`), mits de enumeratie zelf het stereotype `Enumeratie` krijgt. Heeft de enumeratie in de bron een ander stereotype, voeg dat toe aan `ENUM_OWNER_STEREOTYPES`.

## 4. Stappenplan

### Stap 0 - Profielen en scripts installeren in Enterprise Architect
1. Zorg dat het profiel [VNGR MIM 1-2 Grouping NL](./bestanden/VNGR%20MIM%201-2%20Grouping%20NL.ea-toolbox.xml) beschikbaar is (voor user) in het Enterpise Architect project. 
2. Zorg dat de scripts [Migratie-1 inventarisatie VNGR SIM naar MIM](./bestanden/Migratie-1%20inventarisatie%20VNGR%20SIM%20naar%20MIM) en [Migratie-3 eindopschoning Tagged values](./bestanden/Migratie-3%20eindopschoning%20Tagged%20values) in Enterprise Architect beschibaar zijn in de scripting module.

### Stap 1 — Check op oude schade
1. Configuratie invullen in het inventarisatiescript (§1a).
2. Inventarisatiescript draaien als **dry-run** (`APPLY_CHANGES = false`).
3. In het log zoeken naar "GECORRUMPEERDE STEREOTYPE-NAAM". Zulke elementen eerst handmatig via de UI rechtzetten; de tag-logica verwijdert anders ten onrechte alle tags van zo'n element.

### Stap 2 — Tags opschonen (bronprofiel staat nog aan)
1. Dry-run-rapport doornemen: vervallen tags, onbekende tags, ongeldige enumeratiewaarden, lege verplichte velden.
2. `VALUE_REMAP`, `TAG_REMAP`, `TAG_EXCLUDE_FROM_REMOVAL`, `CUSTOM_TAG_ACTIONS` en `STEREOTYPE_REMAP` voor dit model invullen.
3. Opnieuw dry-run tot het rapport klopt.
4. `APPLY_CHANGES = true` en draaien.
5. Resultaat controleren; bij versiebeheer inchecken.

### Stap 3 — FQNames handmatig omzetten in de XMI (bewust niet gescript)
Hiermee wordt de stereotype-koppeling van alle modelelementen in één keer van het bron- naar het doelprofiel verlegd. Importeer het resultaat **altijd in een nieuw, leeg project**: herimport in hetzelfde project kan elementen dupliceren (nieuwe GUID, ander package).

**3a. Voorbereiden**
1. Backup maken; uitgangsrevisie noteren.
2. Controleren dat `VNGRMIM1-2NL` aangevinkt is.

**3b. Exporteren**
3. De package (`ROOT_PACKAGE_GUID`) exporteren naar XMI 1.1.
4. Een ongewijzigde kopie bewaren (`…_origineel.xml`).
5. De codering noteren uit de eerste regel van het bestand (`encoding="…"`).

**3c. FQNames vervangen**
6. Per voorvoegsel tellen hoe vaak het voorkomt (script uit §3.1). Noteer de aantallen.
7. Vervangen, **altijd met `FQName=` ervoor en `::` erachter**, zodat vrije tekst (notities, definities) ongemoeid blijft:

   | Zoeken | Vervangen door |
   |---|---|
   | `FQName=<SOURCE_TECHNOLOGY_QUALIFIER_NAME>::` | `FQName=VNGR MIM 1-2 Grouping NL::` |
   | `FQName=<elk overig voorvoegsel uit §1c>::` | `FQName=VNGR MIM 1-2 Grouping NL::` |

   Veilige manier in PowerShell (behoudt de codering; schrijft naar een nieuw bestand):

   ```powershell
   $in  = 'C:\temp\model.xml'
   $uit = 'C:\temp\model_naar_VNGRMIM12.xml'
   $enc = [Text.Encoding]::GetEncoding(1252)   # gelijk aan de codering van de XMI
   $t   = [IO.File]::ReadAllText($in, $enc)
   $doel = 'FQName=VNGR MIM 1-2 Grouping NL::'
   foreach ($bron in @('VNGR SIM+Grouping NL', 'VNGR MIM 1.2 Grouping NL')) {   # SOURCE_TECHNOLOGY_QUALIFIER_NAME + overige voorvoegsels
       $zoek = "FQName=${bron}::"
       $n = ([regex]::Matches($t, [regex]::Escape($zoek))).Count
       Write-Output "$zoek -> $n keer vervangen"
       $t = $t.Replace($zoek, $doel)
   }
   [IO.File]::WriteAllText($uit, $t, $enc)
   ```

   Doe je het in een teksteditor (bijv. Notepad++): gewoon tekst-zoeken gebruiken, geen reguliere expressie (tekens als `+` en `.` hebben daar een speciale betekenis), en opslaan in dezelfde codering als het origineel. EA schrijft de XMI als één lange regel.
8. Controleren met de telling uit §3.1 op het nieuwe bestand:
   - geen FQNames met `SOURCE_TECHNOLOGY_QUALIFIER_NAME` of een overig voorvoegsel meer;
   - het aantal `VNGR MIM 1-2 Grouping NL` is gelijk aan de som van wat vervangen is (plus wat er eventueel al stond).

**3d. Bronprofiel verwijderen en importeren**
9. Het bronprofiel **volledig verwijderen** (Resources-boom > Delete Profile, of de technology uitschakelen/verwijderen in Manage-Tech), niet alleen deactiveren.
10. Een **nieuw, leeg project** aanmaken en controleren dat `VNGRMIM1-2NL` daar aangevinkt is.
11. De aangepaste XMI importeren.
12. Steekproef: bij enkele elementen, attributen en connectoren controleren dat het stereotype uit `VNGR MIM 1-2 Grouping NL` komt.

Na deze stap hangen de **stereotypes** aan het doelprofiel, maar de **tagged values** nog niet: XMI 1.1 neemt de koppeling tussen tag en profiel niet mee. Stereotypes zonder equivalent in het doelprofiel hebben nu een FQName die naar een niet-bestaand stereotype wijst; die worden in stap 3 via `STEREOTYPE_REMAP` rechtgezet.

### Stap 4 — Eindopschoning (in het nieuwe project)
1. Configuratie invullen (§1b), met `ROOT_PACKAGE_GUID` van de package **in het nieuwe project**, en `STEREOTYPE_REMAP` gelijk aan die uit stap 1.
2. **Dry-run** (`APPLY_CHANGES = false`). Controleer in het uitvoervenster:
   - Technology-id in de XML: `VNGRMIM1-2NL`;
   - Profielen in de technology-XML: bevat `VNGR MIM 1-2 Grouping NL`;
   - Controle technology: `enabled = true`;
   - aantal objecten per stereotype en aantal enumeratiewaarden zonder stereotype;
   - geen meldingen over objecten die aan een **ander** profiel hangen.
3. Testen op een **kleine subpackage** met `APPLY_CHANGES = true` en `CONFIRMED_VNGR_SIM_REMOVED = true`.
4. Rapport en model controleren (profieltabblad bij de tags, waarden teruggezet).
5. Draaien over de hele package.

Het script stopt de hele run zodra EA een stereotype niet overneemt; er blijven dan geen reeksen objecten zonder stereotype achter.

### Stap 5 — Nacontrole en afronding
1. Inventarisatiescript nogmaals draaien als dry-run: `reportTechnologyLinkStatus` moet 0 objecten aan het bronprofiel tonen.
2. Steekproef in EA op elk gebruikt stereotype.
3. Bij versiebeheer: de packages in het nieuwe project onder versiebeheer brengen en inchecken.
4. Rapporten (inventarisatie, eindopschoning) bewaren bij de migratiedocumentatie.

## 5. Bijzondere situaties

- **Meerdere bronprofielen of restanten van eerdere pogingen.** Alle voorvoegsels uit §3.1 behalve het doelprofiel als overig voorvoegsel (§1c) in stap 2c meenemen. Het inventarisatiescript kent maar één bronprofiel; tags van een tweede bronprofiel verschijnen daar als "onbekend".
- **Objecten met een stereotype zonder FQName** (niet aan een profiel gekoppeld). Die worden in stap 2 niet geraakt. Beoordeel in de dry-run van de eindopschoning hoe ze behandeld worden voordat je een echte run doet.
- **Stereotypes zonder equivalent.** Via `STEREOTYPE_REMAP` naar een bestaand doelstereotype met hetzelfde metatype. Controleer of het tussenstereotype dat de eindopschoning kiest voor dat metatype bestaat (voor attributen en associaties `Anoniem`, voor generalisaties `Static`).
- **Version-controlled packages.** Na een "Get Latest" komen tags via XMI 1.1 los van het profiel terug. Draai de eindopschoning dus pas nadat het nieuwe project zelf onder versiebeheer staat en ingecheckt is, of opnieuw na een herlaadactie.
- **Toolbox van het doelprofiel.** In het huidige MDG-bestand (Imvertor 4.4.0) verwijst de toolbox naar `VNGR MIM 1.2 Grouping NL::…` terwijl het profiel `VNGR MIM 1-2 Grouping NL` heet. Nieuwe elementen uit de toolbox krijgen daardoor een niet-bestaand profiel, totdat het MDG-bestand opnieuw is gegenereerd zonder punt in de naam.

## 6. Technische achtergrond (kort)
- Stereotype-koppeling zit in `t_xref` (`Name='Stereotypes'`); in XMI zichtbaar als `$ea_xref_property` met `@STEREO;Name=<Stereotype>;FQName=<Profielnaam>::<Stereotype>;@ENDSTEREO`.
- Een tagged value wordt alleen aan een profiel gekoppeld op het moment dat een stereotype wordt **toegekend**; losse `TaggedValues.AddNew()` levert altijd een los tag op. Daarom werkt de eindopschoning met de 4-stappen-methode (leeg → tussenstereotype → leeg → doelstereotype) en zet daarna de waarden terug.
- EA laat een `StereotypeEx`-toekenning met een onbekende profielnaam stilzwijgend vallen.
- `IsTechnologyLoaded(id)` en `GetTechnologyXML(id)` werken alleen voor technologies die in het model zijn geïmporteerd; voor een technology uit de MDGTechnologies-map is `IsTechnologyEnabled(id)` bepalend en wordt het profiel uit het bestand gelezen.

## Bijlage: stereotypes in VNGR MIM 1-2 Grouping NL

| Stereotype | Geldt voor | Aantal tags |
|---|---|---|
| Anoniem | Association, Attribute | 0 |
| Attribuutsoort | Attribute | 30 |
| Attribuutsoort_proxy | Attribute | 21 |
| Basismodel | Package | 22 |
| Codelijst | DataType | 13 |
| Componenten | Package | 0 |
| Data element | Attribute | 16 |
| Datatype | Attribute | 1 |
| Domein | Package | 18 |
| Enumeratie | Enumeration | 10 |
| Enumeratiewaarde | Attribute | 8 |
| Extern | Package | 18 |
| Externe koppeling | Association | 12 |
| Folder | Package | 0 |
| Gegevensgroep | Attribute | 14 |
| Gegevensgroep_proxy | Attribute | 13 |
| Gegevensgroeptype | Class | 11 |
| Gegevensgroeptype_proxy | Class | 13 |
| Generalisatie | Generalization | 4 |
| Gestructureerd datatype | DataType | 14 |
| Informatiemodel | Package | 24 |
| Interface | Class, DataType | 2 |
| Intern | Package | 9 |
| Isid | Association | 0 |
| Keuze | Association, Attribute, Class, DataType | 10 |
| Keuze attributen | Class | 1 |
| Keuze attribuut | Attribute | 1 |
| Keuze datatypen | Class, DataType | 1 |
| Keuze element | Attribute | 0 |
| Keuze relatie | Association | 1 |
| Keuze relaties | Class | 1 |
| Keuze zonder betekenis | Attribute | 1 |
| Koppelklasse | Class | 9 |
| Objecttype | Class | 13 |
| Objecttype_proxy | Class | 11 |
| Primitief datatype | DataType, PrimitiveType | 14 |
| Process | Class | 0 |
| Product | Class | 1 |
| Project | Package | 4 |
| Provided | Package | 0 |
| Prullenbak | Package | 0 |
| Referentie | Class | 0 |
| Referentie element | Attribute | 14 |
| Referentielijst | DataType | 12 |
| Relatieklasse | AssociationClass | 13 |
| Relatierol | Property | 15 |
| Relatiesoort | Association | 19 |
| Service | Class | 0 |
| Static | Generalization | 0 |
| Static liskov | Generalization | 0 |
| System | Package | 0 |
| System-reference-class | Class | 0 |
| System-reference-package | Package | 0 |
| Toepassing | Package | 21 |
| Trace | Class, DataType | 0 |
| View | Package | 17 |

*Bron: MDG-bestand `VNGR MIM 1.2 Grouping NL.ea-toolbox.xml` (technology-id `VNGRMIM1-2NL`, gegenereerd door Imvertor 4.4.0).*
