Title: OSLO - Richtlijnen voor het Mappen van data
Template: MultiMarkdown

<section id='abstract'>
</section>

Released under the [ISA Open Metadata v1.1](https://joinup.ec.europa.eu/category/licence/isa-open-metadata-licence-v11) license.

<section>

# Inleiding

Dit document beschrijft de mapping van het OSLO conceptueel model naar RDF en XML

</section>

<section>

# URI Strategie

De belangrijkste motivatie om te kiezen voor een URI-strategie is dat toepassingen en catalogi op een eenduidige manier kunnen verwijzen naar services die bestaan bij de verschillende overheden en gebruikers. Dit zal het hergebruik van de services maximaliseren en de impact bij wijzigingen voor implementaties minimaliseren. Dat kan aangezien alle dergelijke services op dezelfde manier hun resources toegankelijk maken.

Het basispatroon dat deze strategie vervult is (in lijn met http://www.opendataforum.info/Docs/URI_strategie_versie2):

	http://{domein}/{type}/{concept}/{referentie}

<section>

## Domein

Het domein bestaat uit het internet-domein en een pad. Het is best zo dat het domein van de URI een referentie bevat naar de bron waar het vandaan komt. 

Om het domein herkenbaar te houden voor ontwikkelaars en gebruikers, is het belangrijk om een generiek domein te voorzien in het belang van verschillende diensten die gecentraliseerd zijn rond een bepaald domein of sector.

Bvb. {service}.data.localgov.be/ of  data.localgov.be/{service}/ maar dit kan eventueel uitgebreid worden naar {eigendomein}/{eigenpad}/{service} of {service}.{eigendomein}/{eigenpad}.

</section>

<section>

## Type

Het type duidt op het soort service waarop aanspraak gemaakt wordt en dit heeft impact op hoe de referentie kan gebruikt worden.

Er zijn 2 types die minimaal dienen ondersteunt te worden:

- id: de referentie verwijst naar een concrete instantie van het concept
- lijst: een referentie is niet nodig, de lijst van concepten of van types concepten (indien geen concept wordt opgegeven) in het domein wordt weergegeven.

Op een lijst kunnen geavanceerdere operaties uitgevoerd worden zoals een custom filter dat kan toegepast worden een bepaald type concept of op het gehele domein of zoeken op tekstwaarde van een bepaald veld in het domein (al dan niet beperkt binnen een bepaald concept). Deze actie wordt meegegeven in de request (bvb. via de url parameters).

Daarnaast zijn er ook nog andere types mogelijk zoals een concretisatie van id (rijksregisternummer "rrk" of ondernemingsnummer "kbo".

</section>

<section>

## Concept

Het type duidt aan wat voor soort identiteit het gaat waarnaar verwezen wordt met de identifier. In het bijzonder zijn hier de geldige concepten (met Nederlandstalige terminologie):

- Basisconcepten
	- persoon en persoonsrelatie 
	- organisatie en organisatierelatie
	- hoedanigheid
	- contactinformatie
	- locatie (omvat ook verblijfsobjecten)
	- adres
	- dienstverlening
	- product

- Uitgebreide concepten
	- kanaal
 	- beleidsveld
	- bezigheid
	- openingsuren

</section>

<section>

## Resources

Om all attributen en relaties van een specifiek concept te krijgen, gekenmerkt aan de hand van zijn identifier gebruiken we volgende uri:

	http://{domein}/id/{concept}/{referentie}

Het resultaat van deze vraag is een document beschreven volgens OSLO (RDF of OSLO-XML) dat de beoogde instantie van het concept beschrijft, of derefereert.

Voorbeeld:

Het bevragen van een resource, een ‘READ’ operatie, vertaalt zich bij een REST Service naar een GET request.

	http://example.com/id/organisatie

Vaak zal in de praktijk een resource gekenmerkt worden door een specifiek type 'id' zoals bijvoorbeeld KBO-nummer. Dit is zeker het geval indien meerdere mogelijkheden tot mapping zijn:

	http://example.com/kbo/organisatie

</section>

</section>

<section>

# Namespaces

De namespace of the OSLO vocabulary is:

	http://purl.org/oslo/ns/ 
	http://purl.org/oslo/ns/vocabulary (XML Schema)

<section>

## XML

Aan de basis ligt een Identificatie klasse, die is gebaseerd op de gelijknamige UN/CEFACT klasse [UN/CEFACT] en is gedefinieerd in de ADMS namespace [ADMS]. In de de OSLO XML specificatie worden componenten uit de Core Vocabularies XML hergebruikt. Het idee hierachter is dat de OSLO XML specificatie zo maximaal gealigneerd is op de Core Vocabularies XML. Aan de basis van de Core Vocabularies XML is een bibliotheek van veelgebruikte informatie elementen die voorzien is door de “Universal Bussiness Language” bibliotheek (UBL) [OASIS]. Met dit design zorgt de Core Vocabularies voor optimale herbruikbaarheid van elementen gedefinieerd door de “Core Component Technical Specification” [CCTS] van UN/CEFACT (de basis van UBL).
Voor de contactinformatie te modelleren wordt gebruik gemaakt van VCard componenten zoals gespecifieerd in [RFC6350] door het IETF. Uit de beschikbare componenten werd een subset geselecteerd die gealinieerd is met het OSLO Conceptueel model.

|   Namespace                                                                   |  Prefix  |         Beheer:|   Domein                    |
|-------------------------------------------------------------------------------|----------|----------------|-----------------------------|
|   urn:un:unece:uncefact:data:specification:UnqualifiedDataTypesSchemaModule:2 |     udt  |   [UN/CEFACT](http://www.unece.org/cefact.html) |   Datatypes                 |
|   http://www.w3.org/ns/corevocabulary/BasicComponents                         |     cvb  |         [ISA](http://joinup.ec.europa.eu/asset/core_business/asset_release/core-business-vocabulary-100) |   Basis componenten         |         
|   http://www.w3.org/ns/corevocabulary/AggregateComponents                     |     cva  |            ISA |   Geaggregeerde componenten |
|   urn:oslo:names:specification:schema:xsd:CommonBasicComponents-1             |   ovc    |   OSLO         |   OSLO Basis componenten    |
|   urn:oslo:names:specification:schema:xsd:AggregateComponents-1               |   ova    |   OSLO         |   OSLO Geaggregeerde componenten |


- [OASIS](http://docs.oasis-open.org/ubl/UBL-2.1.pdf) Universal Business Language v2.1.
- [CCTS](http://www.unece.org/fileadmin/DAM/cefact/codesfortrade/CCTS/CCTS-Version3.pdf) United Nations Centre for Trade Facilitation and Electronic Business. Core Components Technical Specification Version 3.0
- [UN/CEFACT](http://www.unece.org/cefact/) UN Centre for Trade Facilitation and e-Business
- [ADMS](http://joinup.ec.europa.eu/asset/adms/description) The Asset Description Metadata Standard,
- [RFC6350](http://tools.ietf.org/html/rfc6350)

</section>

<section>

## RDF

Het RDF schema maak optimaal gebruik van bestaande schema’s en vocabularies. Dit is belangrijk zodat toepassingen en andere afgeleide schema’s zo makkelijk gelinkt kunnen worden aan het OSLO RDF-schema zonder dat daarvoor eerst complexe mappings gemaakt moeten worden tussen vocabularies voor courant gebruikte object, types en kenmerken in de beschrijving van de uitgewisselde gegevens. Bovendien garandeert hergebruik van bestaande schema’s compatibiliteit en herkenbaarheid op langere termijn van het OSLO RDF-schema. Een bijkomend voordeel is dat de set eigen definities in het OSLO RDF-schema beperkt blijft zodat deze makkelijk te ondersteunen blijft.

|   Namespace                       |   Prefix  |          Beheer:|   Domein                   |
|-----------------------------------|-----------|-----------------|----------------------------|
|   http://www.w3.org/ns/org        |     org   |      [W3C GLD](http://www.w3.org/2011/gld/wiki/Main_Page) |   Organisaties             |
|   http://www.w3.org/ns/regorg     |     rov   |         W3C GLD |   Geregistreerde           |
|                                   |           |                 |   Organisaties             |
|   http://www.w3.org/ns/person     |    person |          [ISA](http://joinup.ec.europa.eu/asset/core_person/description) |   Personen                 |
|   http://www.w3.org/ns/locn       |   locn    |   ISA(http://joinup.ec.europa.eu/asset/core_location/home)        |   Locaties                 |
|   http://purl.org/vocab/cpsv      |   cpsv    |   [ISA](http://joinup.ec.europa.eu/asset/core_public_service/home)       |   Publieke dienstverlening |
|   http://purl.org/dc/terms/       |   dcterms |   [DCMA](http://dublincore.org/about/organization/)       |   Metadata                 |
|   http://schema.org/              |   schema  |   [schema.org](http://schema.org/docs/schemas.html) |   Markup, Metadata         |
|   http://www.w3.org/2006/vcard/ns |   vcard   |   W3C           |   Contactgegevens          |
|   http://www.w3.org/ns/adms       |   adms    |   [WRC GLD](https://dvcs.w3.org/hg/gld/raw-file/default/adms/index.html)    |   Standaarden, Codelijsten |
|                                   |           |                 |   en Taxonomien            |
|   http://www.w3.org/2006/time     |   time    |   [W3C](http://www.w3.org/TR/owl-time/)       |   Tijdsaanduiding          |
|   http://www.w3.org/ns/prov       |   prov    |   [W3C](http://www.w3.org/TR/prov-o/)        |   Herkomst                 |

</section>

<section>

## Voorbeeld

Eenzelfde persoon kan volgens RDF en het OSLO XML Schema worden uitgewisseld.

Indien een dienstverlener de persoonsgegevens ter beschikking wil stellen dan kan dit bijvoorbeeld via een url:

GET http://dienstverlener.be/oslo/personen/123456789

Afhankelijk van de context van de gebruiker en de toepassing zal het opvragen van deze url de gegevens van de persoon teruggeven in RDF of geformatteerd volgens het OSLO XML Schema.

Het antwoord in RDF/XML:

	<rdf:RDF 
		xmlns:person="http://www.w3.org/ns/person#"
		xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
		<person:Person rdf:nodeID="123456789">
		<person:familyName>Brickley</person:familyName>
		<person:givenName>Dan</person:familyName>
		<!-- etc... -->
		</person:Person>
	</rdf:RDF>

Hetzelfde antwoord in OSLO XML:

	<operson:person
			xmlns:xsi=”http://www.w3.org/2001/XMLSchema-instance”
			xmlns:operson="http://oslo.v-ict-or.be/vocabulary/person"
			xmlns:cva="http://www.w3.org/ns/corevocabulary/AggregateComponents"
			xmlns:cvb="http://www.w3.org/ns/corevocabulary/BasicComponents"
			xmlns:cbc="urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2">
		<cbc:FamilyName>Brickley</cbc:FamilyName>
		<cvb:GivenName>Dan</cvb:GivenName>
		<!-- etc... -->
	</operson:person>	
	
</section>

</section>

<section>

# Entiteiten

<section>

## Basiscomponenten

|   Conceptueel                         |   XML Schema                 |   RDF                       |
|---------------------------------------|------------------------------|-----------------------------|
|   Persoon                             |   person                     |   person:Person             |
|                                       |   (OvpersonType)             |                             |
|   Organisatie                         |   organization               |   oslo:Organization         |
|                                       |   (OvorganizationType)       |                             |
|   Hoedanigheid                        |   membership                 |   oslo:Membership           |
|                                       |   (OvmembershipType)         |                             |
|   Persoonsrelatie                     |   PersonRelation             |   oslo:PersonRelation       |
|                                       |   (PersonRelationType)       |                             |
|   Organisatierelatie                  |   OrganizationRelation       |   oslo:OrganizationRelation |
|                                       |   (OrganizationRelationType) |                             |
|   Agent                               |   (abstract)                 |   dcterms:Agent             |
|                                       |   OvAgentType                |                             |
|   Locatie                             |   Location                   |   locn:Location             |
|                                       |   (CvLocationType)           |                             |
|   Verblijfsobject                     |   ResidenceObject            |   oslo:ResidenceObject      |
|                                       |   (OvresidenceobjectType)    |                             |
|   Gebouw                              |   building                   |   oslo:Building             |
|                                       |   (OsloBuildingType)         |                             |
|   Perceel                             |   parcel                     |   oslo:Parcel               |
|                                       |   (OsloParcelType)           |                             |
|   Basis Adres                         |   address                    |   oslo:BaseAddress          |
|                                       |   (OsloBaseAddressType)      |                             |
|   Dienstverlening                     |   PublicService              |   cpsv:PublicService        |
|                                       |   (OvPublicServiceType)      |                             |
|   Initiatie                           |   input                      |   cpsv:Input                |
|                                       |   (OvDocumentType)           |                             |
|   Product                             |   output                     |   cpsv:output               |
|                                       |   (OvProductType)            |                             |
| Kanaal       |   | oslo:Channel     |
| Bezigheid    |   | oslo:Activity    |
| Beleidsthema |   | oslo:PolicyTheme |
| Product      |   | oslo:Product     |
| Openingsuren |   | TBD              |

</section>

<section>

## Secundaire componenten

|   Conceptueel             |   XML Schema         |   RDF                       |
|---------------------------|----------------------|-----------------------------|
|   Identificatie           |   identifier         |   adms:identifier           |
|   (voor alle objecten)    |   (OvidentifierType) |   (adms:Identifier)         |
|   Code                    |   code               |   oslo:Code                 |
|   (alle types)            |   (OvcodeType)       |                             |
|   Herkomst                |   provenance         |   prov:entity (prov:Entity) |
|                           |   (OvprovenanceType) |                             |
|   Contact                 |   contact            |   oslo:contact              |
|                           |   (OvcontactType)    |   (vcard:VCard)             |
|   Geometrie               |   geometry           |   locn:geometry             |
|                           |   (cvb:GeometryType) |                             |
|   Uitgebreid              |   extendedAddress    |   oslo:ExtendedAddress      |
|   Adres (bij Basis Adres) |   (OsloAddressType)  |                             |
|   Rol                     |   Role               |   oslo:Role                 |
|                           |   (OvRoleType)       |                             |
|   Juridisch               |   Rule               |   cpsv:Rule                 |
|   kader                   |   (OvRuleType)       |                             |
|   Status                  |   Status             |   oslo:Status               |
|   Machtiging              |   permission         |   oslo:Permission           |
|                           |   (OvPermissionType) |                             |

</section>

</section>

<section>

# Metadata

<section>

## Identifier

|   Conceptueel     |   XML Schema             |   RDF                |
|-------------------|--------------------------|----------------------|
|   Sleutel         |   cvb:Identifier         |   dcterms:identifier |
|   Type            |   cvb:IdentifierType     |   dcterms:type       |
|   Bron            |   cvb:IssuingAuthority,  |   adms:schemeAgency  |
|                   |   cvb:IssuingAuthorityID |   dcterms:publisher  |
|   Wijzigingsdatum |   cbc:IssueDate          |   dcterms:issued     |
|   Revisie         |   ovc:revisionOf         |   prov:wasRevisionOf |
|   Actief          |   ovc:isActive           |   oslo:isActive	  |

</section>

<section>

## Code (all types)

|   Conceptueel  |   XML Schema      |   RDF                 |
|----------------|-------------------|-----------------------|
|   Code         |   ovc:code        |   dcterms:identifier  |
|   Label        |   ovc:Label       |   rdfs:label          |
|   Omschrijving |   cvb:Content     |   dcterms:description |
|   Lijst        |   cvb:List        |   dcterms:isPartOf    |
|   (Bron)       |   cvb:ListAgency  |   adms:schemeAgency   |
|   (Versie)     |   cvb:ListVersion |   dcterms:issued      |

</section>

</section>

<section>

# Contactinformatie

<section>

## Persoon

TODO: Opsplitsen Naam volgens nieuwe wetgeving (eerste en tweede familienaam)

TODO: Opsplitsen Rang

|   Conceptueel |   XML Schema         |   RDF                   |
|---------------|----------------------|-------------------------|
|   Naam        |   cvb:FamilyName     |   person:familyName     |
|   Voornaam    |   cvb:GivenName      |   person:givenName      |
|   Andere      |   cvb:PatronomycName |   person:patronymicName |
|   voornamen   |                      |                         |
|   Geslacht    |   cvb:GenderCode     |   schema:gender         |
|   Adellijke   |   oslo:nobelityTitle |   oslo:nobelityTitle    |
|   titel       |                      |                         |
|   (Versie)    |   cvb:ListVersion    |   adms:version          |
|   Geboortedatum     |   cvb:BirthDate       |   schema:birthDate       |
|   Geboorteplaats    |   ovc:placeOfBirth    |   person:placeOfBirth    |
|   Datum             |   cvb:DeathDate       |   schema:deathDate       |
|   overlijden        |                       |                          |
|   Plaats overlijden |   ovc:placeOfDeath    |   person:placeOfDeath    |
|   Wettelijk         |   legalCohabetation   |   oslo:legalCohabetation |
|   samenwonend       |   (ovc:IndicatorType) |                          |
|   Burgerlijke       |   civilClass          |   oslo:civilClass        |
|   staat             |   (ovc:OvcodeType)    |                          |
|   Juridische        |   legalIncompetence   |   oslo:legalIncompetence |
|   onbewkwaamheid    |   (ovc:OvcodeType)    |                          |
|   Is                |   domicile            |   oslo:domicile          |
|   gedomicilieerd op |                       |                          |
|   Verblijft         |   residence           |   oslo:residence         |

</section>

<section>

## Organisatie

|   Conceptueel       |   XML Schema                      |   RDF                    |
|---------------------|-----------------------------------|--------------------------|
|   Maatschappelijke  |   cvb:LegalName                   |   rov:legalName          |
|   naam              |                                   |                          |
|   Commerciële       |   cvb:AlternativeName             |   dcterms:alternative    |
|   naam              |                                   |                          |
|   Afgekorte naam    |   cvb:AlternativeName             |   dcterms:alternative    |
|   Type              |   ovc:companyTypeOvbusinessCode   |   rov:orgType            |
|                     |   (ovc:OvcodeType)                |                          |
|   Vestiging         |   isBranch                        |   rdf:type org:Site      |
|                     |   (ovc:IndicatorType)             |   org:hasSite, invers org:siteOf |
|   Oprichtingsdatum  |   establishmentDate               |   oslo:establishmentDate |
|                     |   (ovc:OsloDateType)              |                          |
|   Datum             |   shutdownDate                    |   oslo:shutDownDate      |
|   stopzetting       |   (ovc:OsloDateType)              |                          |
|   KBO soort         |   kind                            |   oslo:kind              |
|                     |   (ovc:IndicatorType)             |                          |
|   KBO rechtsvorm    |   ovc:legalFormCode               |   oslo:legalFormCode     |
|   KBO               |   ovc:legalStatusCode             |   rov:companyStatus      |
|   rechtstoestand    |   (ovc:legalStateEnumType)        |                          |
|   KBO reden         |   reasonShutdown                  |   oslo:reasonShutDown    |
|   stopzetting       |   (ovc:ReasonShutDownEnumType)    |                          |
|   KBO               |   ovc:FunctionPhase               |   oslo:functionPhase     |
|   Hoedanigheidsfase |                                   |                          |
|   KBO               |   ovc:Function                    |   oslo:function          |
|   Hoedanigheid      |                                   |                          |
|   KBO Status        |   ovc:companyStatusOvbusinessCode |   oslo:companyStatus     |
|                     |   (ovc:OvcodeType)                |                          |
|   activiteit        |   companyActivityOvbusinessCode   |   rov:companyActivity    |
|                     |   (ovc:OvcodeType)                |                          |
|   facturatielocatie |   invoiceLocation                 |   oslo:invoiceLocation   |
|                     |   (OvresidenceobjectType)         |                          |
|   KBO nummer        |   KBONumber                       |   rov:registration       |
|                     |                                   |                          |
|   KBO locatie       |   authenticLocation               |   oslo:authenticLocation |
|                     |   (OvresidenceobjectType)         |                          |
|   leveringslocatie  |   deliveryLocation                |   oslo:deliveryLocation  |
|                     |   (OvresidenceobjectType)         |                          |
|   post locatie      |   mailingLocation                 |   oslo:mailingLocation   |
|                     |   (OvresidenceobjectType)         |                          |

</section>

<section>

## Organisatierelatie

|   Conceptueel |   XML Schema           |   RDF                   |
|---------------|------------------------|-------------------------|
|   behoort tot |   from                 |   oslo:fromOrganization |
|               |   (OvorganizationType) |                         |
|   groepeert   |   to                   |   oslo:toOrganization   |
|               |   (OvorganizationType) |                         |
|   geldigheid  |   ovc:validityPeriod   |   oslo:validityPeriod   |

</section>

<section>

## Hoedanigheid

|   Conceptueel        |   XML Schema              |   RDF                     |
|----------------------|---------------------------|---------------------------|
|   functie            |   ovc:membershipFunction  |   oslo:membershipFunction |
|   geldigheidsperiode |   ovc:validityPeriod      |   org:memberDuring        |
|   type               |   ovc:membershipType      |   oslo:membershipType     |
|                      |   (OvcodeType)            |                           |
|   ontvangt post      |   mailingLocation         |   oslo:mailingLocation    |
|   op                 |   (OvresidenceobjectType) |                           |
|   persoon            |   person                  |   org:member              |
|                      |   (OvpersonType)          |                           |
|   organisatie        |   organisation            |   org:organization        |
|                      |   (OvorganizationType)    |                           |
|   beschikbaar op     |   availableAt             |   oslo:availableAt        |
|                      |   (OvresidenceobjectType) |                           |

</section>

</section>

<section>

# Lokalisatie

<section>

## Verblijfsobject

|   Conceptueel   |   XML Schema         |   RDF                 |
|-----------------|----------------------|-----------------------|
|   geldigheid    |   ovc:validityPeriod |   oslo:validityPeriod |
|   heeft gebouw  |   ovc:parcel         |   oslo:parcel         |
|   heeft perceel |   ovc:building       |   oslo:building       |
|   adresseert    |   ovc:address        |   oslo:address        |

</section>

<section>

## Locatie

|   Conceptueel        |   XML Schema         |   RDF                 |
|----------------------|----------------------|-----------------------|
|   geldigheidsperiode |   ovc:validityPeriod |   oslo:validityPeriod |
|   Heeft              |   geometry           |   locn:geometry       |
|   geometrie          |   (cvb:GeometryType) |                       |

</section>

<section>

## Adres

|   Conceptueel    |   XML Schema              |   RDF                    |
|------------------|---------------------------|--------------------------|
|   land           |   cvb:AdminunitFirstline  |   oslo:country           |
|   gemeente       |   cvb:PostName            |   locn:postName          |
|   postcode       |   cvb:PostCode            |   locn:postCode          |
|   straatnaam     |   cvb:Thoroughfare        |   locn:thoroughfare      |
|   huisnummer     |   cvb:LocatorDesignator   |   oslo:locatorDesignator |
|   locator        |   LocatorDesignator       |   oslo:locatorDesignator |
|   provincie      |   cvb:AdminunitSecondline |   oslo:province          |
|   deelgemeente   |   cvb:CvaddressArea       |   oslo:addressArea       |
|   postbus        |   cvb:PoBox               |   oslo:poBox             |
|   locatienaam    |   cvb:LocatorName         |   oslo:locatorName       |
|   volledig adres |   cvb:FullCvaddress       |   oslo:fullAddress       |

Het basisadres is uitgebreid met nog twee extra eigenschappen.

|   Conceptueel       |   XML Schema          |   RDF                   |
|---------------------|-----------------------|-------------------------|
|   uitgebreid        |   ovc:extendedAddress |   oslo:extended_address |
|   adres             |   (OsloAddressType)   |                         |
|   bakent            |   ovc:adminUnit       |   oslo:adminUnit        |
|   administratief af |   (OvcodeType)        |                         |

</section>

</section>

<section>

# Dienstverlening

<section>

## Dienstverlening

|   Conceptueel   |   XML Schema            |   RDF              |
|-----------------|-------------------------|--------------------|
|   Streefdatum   |   ovc:targetDate        |   oslo:targetDate  |
|   Type          |   type                  |   dcterms:type     |
|                 |   (OvcodeType)          |                    |
|   Vereist       |   hasInput              |   cpsv:hasInput    |
|                 |   (OvDocumentType)      |                    |
|   Levert        |   produces              |   cpsv:produces    |
|                 |   (OvProductType)       |                    |
|   Bevindt zich  |   status                |   oslo:status      |
|   in            |   (OvStatusType)        |                    |
|   Resulteert in |   result                |   oslo:result      |
|                 |   (OvcodeType)          |                    |
|   Volgt         |   ovc:Rule              |   cpsv:follows     |
|   Voorziet      |   ova:Role              |   oslo:hasRole     |
|   behoort tot   |   requires              |   dcterms:requires |
|                 |   (OvPublicServiceType) |                    |

</section>

<section>

## Rol

|   Conceptueel |   XML Schema         |   RDF                |
|---------------|----------------------|----------------------|
|   Type        |   roleType           |   dcterms:type       |
|               |   (OvcodeType)       |                      |
|   bestaat uit |   permission         |   oslo:hasPermission |
|               |   (OvPermissionType) |                      |
|   vervult     |   agent              |   oslo:hasActor      |
|               |   (ovc:OvagentType)  |                      |

</section>

<section>

## Machtiging

|   Conceptueel        |   XML Schema         |   RDF                 |
|----------------------|----------------------|-----------------------|
|   geldigheidsperiode |   ovc:validityPeriod |   oslo:validityPeriod |

</section>

<section>

## Status

|   Conceptueel      |   XML Schema       |   RDF                 |
|--------------------|--------------------|-----------------------|
|   Type             |   statusType       |   oslo:statusType     |
|                    |   (OvcodeType)     |                       |
|   Heeft statuscode |   statusCode       |   oslo:statusCode     |
|                    |   (OvcodeType)     |                       |
|   Toelichting      |   description      |   dcterms:description |
|   Wijzigingsdatum  |   modificationDate |   dcterms:modified    |

</section>

<section>

## Initatie

|   Conceptueel |   XML Schema            |   RDF                 |
|---------------|-------------------------|-----------------------|
|   Naam        |   name                  |   dcterms:title       |
|   Inhoud      |   content               |   dcterms:description |
|               |   (OvContentsType)      |                       |
|   Lokaliseert |   location              |   dcterms:spatial     |
|               |   (locn:CvlocationType) |                       |

</section>

<section>

## Product

|   Conceptueel        |   XML Schema            |   RDF                 |
|----------------------|-------------------------|-----------------------|
|   Type               |   productType           |   oslo:productType    |
|                      |   (OvcodeType)          |                       |
|   Naam               |   name                  |   dcterms:title       |
|   Inhoud             |   content               |   dcterms:description |
|                      |   (OvContentsType)      |                       |
|   Geldigheidsperiode |   ovc:validityPeriod    |   oslo:validityPeriod |
|   Heeft              |   coverage              |   dcterms:coverage    |
|   toepassingsgebied  |   (locn:CvlocationType) |                       |

</section>

</section>

<section>

# Gedeelde Catalogus

<section>

## Klassen

| Domeinmodel  | RDF class        | XML class |
|--------------|------------------|-----------|
| Kanaal       | oslo:Channel     | TBD       |
| Bezigheid    | oslo:Activity    | TBD       |
| Beleidsthema | oslo:PolicyTheme | TBD       |
| Product      | oslo:Product     | TBD       |
| Openingsuren | TBD              | TBD       |

</section>

<section>

## Eigenschappen

| Entiteit             | Relatie        | Object       |
|---------------------------|------------------------|------------------|
| __Bezigheid__             | RDF relation           | RDF object       |
| id                    | adms:identifier     | adms:Identifier  |
| roltype               | oslo:roleType          | oslo:Code        |
|                       |                        |                  |
| __Beleidsveld__          | RDF relation           | RDF object       |
| id                    | adms:identifier     | adms:Identifier  |
| type                  | dcterms:type           | oslo:Code        |
| omschrijving          | dcterms:description    | literal          |
|                       |                        |                  |
| __Product__               | RDF relation           | RDF object       |
| id                    | adms:identifier     | adms:Identifier  |
| behoort tot           | oslo:belongsTo         | oslo:PolicyTheme |
| toepassingsgebied     | oslo:scope             | dcterms:Location |
| gerelateerd aan       | oslo:relatesTo         | oslo:Activity |
|                       |                        |                  |
| __Kanaal__                | RDF relation           | RDF object       |
| id                    | adms:identifier     | adms:Identifier  |
| geeft toegang tot     | oslo:providesAccesTo   | oslo:Product     |
| availableAt           | oslo:availableAt       | dcterms:Location |
| te bereiken op        | oslo:contact           | oslo:Contact     |
| type                  | dcterms:type           | oslo:Code        |
| heeft beschikbaarheid | TBD                    |                  |
|                       |                        |                  |
| __Organisatie__           | RDF relation           | RDF object       |
| heeftBezigheid        | oslo:hasRoleInActivity | oslo:Activity    |
| heeftKanaal           | oslo:hasChannel        | oslo:Channel     |
|                       |                        |                  |
| __Person__                | RDF relation           | RDF object       |
| heeftBezigheid        | oslo:hasRoleInActivity | oslo:Activity    |

</section>

</section>

<section>

# Tools

<section>

## Own Domain Model to OSLO Model

TODO: Explain helper Table

| OSLO                  |                             |                                |      | Eigen Model |                 |
|-----------------------|-----------------------------|--------------------------------|------|-------------|-----------------|
| Entiteiten            | Type                        | Codelijsten (dictionaries)     |      |             |                 |
| Identificatie         | OSLO Entiteit               | productType                    | IPDC |             |                 |
| Code                  | OSLO Entiteit               | rolType                        | ?    |             |                 |
| Product               | OSLO Entiteit               | functieType                    | ?    |             |                 |
| Kanaal                | Gedeelde Catalogus Entiteit | beleidsveld                    | BBC  |             |                 |
| Beleidsveld           | OSLO Code (BBC)             | kanaalType                     | ?    |             |                 |
| Bezigheid             | Gedeelde Catalogus Entiteit |                                |      |             |                 |
| Persoon               | OSLO Entiteit               |                                |      |             |                 |
| Organisatie           | OSLO Entiteit               |                                |      |             |                 |
| Organisatierelatie    | OSLO Entiteit               |                                |      |             |                 |
| Hoedanigheid          | OSLO Entiteit               |                                |      |             |                 |
| Contactinformatie     | VCARD                       |                                |      |             |                 |
| Locatie               | OSLO Entiteit               |                                |      |             |                 |

| Mappings              | Type                        | Opmerking                      |      | Aanwezig?   | Waar te vinden? |
|-----------------------|-----------------------------|--------------------------------|------|-------------|-----------------|
| __Identificatie__     |                             |                                |      |             |                 |
| id                    | Tekst                       |                                |      |             |                 |
| bron                  | Tekst                       |                                |      |             |                 |
| versie                | Tekst                       |                                |      |             |                 |
| wijzigingsdatum       | Datum                       |                                |      |             |                 |
| __Code__              |                             |                                |      |             |                 |
| id                    | Tekst                       |                                |      |             |                 |
| label                 | Tekst                       |                                |      |             |                 |
| omschrijving          | Tekst                       |                                |      |             |                 |
| bron                  | Tekst                       |                                |      |             |                 |
| versie                | Tekst                       |                                |      |             |                 |
| __Product__           |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| naam                  | Tekst                       |                                |      |             |                 |
| inhoud                | Tekst                       |                                |      |             |                 |
| geldigheid            | Tijdsperiode                |                                |      |             |                 |
| type                  | Code                        |                                |      |             |                 |
| heeftKanaal           | Kanaal                      |                                |      |             |                 |
| __Kanaal__            |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| locatie               | Locatie                     |                                |      |             |                 |
| contactinformatie     | Contactinformatie           |                                |      |             |                 |
| geeftToegangTot       | Product                     |                                |      |             |                 |
| type                  | Code                        |                                |      |             |                 |
| __Persoon__           |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| naam                  | Tekst                       |                                |      |             |                 |
| voornaam              | Tekst                       |                                |      |             |                 |
| andere voornamen      | Tekst                       |                                |      |             |                 |
| adellijke titel       | Tekst                       |                                |      |             |                 |
| geslacht              | Tekst (Lijst)               | ISO/IEC 5218                   |      |             |                 |
| Geboortedatum         | Datum/tijd                  |                                |      |             |                 |
| Nationaliteit         | Tekst (Lijst)               | via ISO-3166-1 Alpha-3 of NIS  |      |             |                 |
| Datum overlijden      | Datum/tijd                  |                                |      |             |                 |
| Burgerlijke staat     | Tekst                       | Rijksregister                  |      |             |                 |
| Geboorteplaats        | Tekst                       | NIS                            |      |             |                 |
| Plaats van overlijden | Tekst                       | NIS                            |      |             |                 |
| Wettelijk Samenwonend | Bool                        |                                |      |             |                 |
| contactinformatie     | Contactinformatie           |                                |      |             |                 |
| __Organisatie__       |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| Maatschappelijke naam | Tekst                       |                                |      |             |                 |
| Commerciële naam      | Tekst                       |                                |      |             |                 |
| Afgekorte naam        | Tekst                       |                                |      |             |                 |
| Vestiging             | Bool                        | 0 = Onderneming, 1 = Vestiging |      |             |                 |
| Oprichtingsdatum      | Datum/tijd                  |                                |      |             |                 |
| __Organisatierelatie__ |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| behoort tot           | Organisatie                 |                                |      |             |                 |
| groepeert             | Organisatie                 |                                |      |             |                 |
| __Hoedanigheid__      |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| functietype           | Code                        |                                |      |             |                 |
| functieomschrijving   | Tekst                       |                                |      |             |                 |
| Persoon               | Persoon                     |                                |      |             |                 |
| Organisatie           | Organisatie                 |                                |      |             |                 |
| Contactinformatie     | Contactinformatie           |                                |      |             |                 |
| __Bezigheid__         |                             |                                |      |             |                 |
| id                    | Identificatie               |                                |      |             |                 |
| rolType               | Code                        |                                |      |             |                 |
| persoon               | Persoon                     |                                |      |             |                 |
| Organisatie           | Organisatie                 |                                |      |             |                 |

</section>

<section>

## OSLO Model to OSLO RDF or OSLO XML

</section>

<section>

## OSLO XML to RDF

TODO: Describe (RML mapping file + script?)

</section>

<section>

## OSLO RDF 

TODO: Describe (RML mapping file + script?)

</section>

</section>

<section class='appendix'>

Acknowledgements
================

Speciale dank aan ...

</section>

<!--- Info about the scripts below: http://www.w3.org/respec/ref.html -->
<script src='respec-w3c-common.js'
		async class='remove'>
</script>

<script class='remove'>
  var respecConfig = {
	  // specification status (e.g. WD, LCWD, WG-NOTE, etc.). If in doubt use ED.
	  specStatus:           "unofficial",
	  
	  // the specification's short name, as in http://www.w3.org/TR/short-name/
	  shortName:            "oslo-service-spec",
	  
	  // licensing
	  additionalCopyrightHolders: "OSLO",

	  // if your specification has a subtitle that goes below the main
	  // formal title, define it here
	  // subtitle   :  "an excellent document",

	  // if you wish the publication date to be other than the last modification, set this
	  publishDate:  "2014-04-24",

	  // if the specification's copyright date is a range of years, specify
	  // the start date here:
	  copyrightStart: "2013",

	  // if there is a previously published draft, uncomment this and set its YYYY-MM-DD date
	  // and its maturity status
	  // previousPublishDate:  "1977-03-15",
	  // previousMaturity:  "WD",

	  // if there a publicly available Editor's Draft, this is the link
	  // edDraftURI:           "http://berjon.com/",

	  // if this is a LCWD, uncomment and set the end of its review period
	  // lcEnd: "2009-08-05",

	  // editors, add as many as you like
	  // only "name" is required
	  editors:  [
		  {
			  name:       "Laurens De Vocht"
		  ,   mailto:     "laurens.devocht@ugent.be"
		  ,   company:    "Ghent University - iMinds"
		  ,   companyURL: "http://iminds.be"
		  },
	  ],
	  
	  // name of the WG
	  // wg:           "OSLO Werkgroep",
	  
	  // URI of the public WG page
	  // wgURI:        "http://example.org/really-cool-wg",
	  
	  // name (without the @w3c.org) of the public mailing to which comments are due
	  // wgPublicList: "spec-writers-anonymous",
	  
	  // URI of the patent status for this WG, for Rec-track documents
	  // !!!! IMPORTANT !!!!
	  // This is important for Rec-track documents, do not copy a patent URI from a random
	  // document unless you know what you're doing. If in doubt ask your friendly neighbourhood
	  // Team Contact.
	  // wgPatentURI:  "",
	  // !!!! IMPORTANT !!!! MAKE THE ABOVE BLINK IN YOUR HEAD
  };
</script>
