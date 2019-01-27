********************
Linked Data: vervolg
********************

De eerste stap van Linked Data is het gebruik van globale namen en schema's.
Dit maakt de uitwisseling van gegevens en het koppelen van databases mogelijk.

Maar dat is nog maar het begin:

* met Linked Data kun je betekenis toevoegen aan gegevens, op zo'n manier dat computers hiermee kunnen "rekenen".
  Denk bijvoorbeeld aan een bestand met familierelaties: je kunt dan ook queries vragen over relaties die niet direct in de database opgeslagen zijn.
* met Linked Data kun je factorisatie (normalisatie) over databases heen uitvoeren:
  je kunt ervoor zorgen dat gegevens maar 1 maal opgeslagen worden,
  en dat andere databases automatisch deze gegevens gebruikt,
  waardoor een lokale update niet nodig is.
  (Denk bijvoorbeeld hoeveel databases jouw adresgegevens hebben opgeslagen;
  als je verhuist, moeten op de een of andere manier al die databases bijgewerkt worden.)
* met Linked Data kun je ervoor zorgen dan jouw data van jou blijft:
  anderen kunnen jouw data raadplegen als dat nodig is, en als dat van jou mag -
  zonder dat ze een lokale kopie hiervan hoeven bij te houden.
  (In het kader van de privacywetgeving is het bewaren van persoonlijke gegevens veel lastiger geworden:
  met Linked Data hoeft dat niet meer, dan kunnen deze gegevens opgevraagd worden als het nodig is.)
  (*Opmerking*: het is wel handig als je hierbij gebruik maakt van efficiënte caching en synchronisatie.)
* een mogelijke infrastructuur hiervoor is de "persoonlijke Pod",
  zoals ontwikkeld door Solid(MIT)/Inrupt.
* in het web wordt Linked Data gebruikt om het zoekproces te verbeteren:


Globale normalisatie
====================

Met *normalisatie* bedoelen we hier dat je gemeenschappelijke gegevens "buiten haakjes haalt",
en deelt met meerdere databases.
Het belangrijkste doel hiervan is dat je deze gemeenschappelijke gegevens dan maar op één plaats hoeft aan te passen,
als deze veranderen.

Denk bijvoorbeeld aan het adres van een persoon:
Als dit in allerlei databases opgeslagen is, dan moeten deze bij een verhuizing allemaal aangepast worden.
Als deze databases gebruik kunnen maken van dezelfde "bron" voor deze gegevens,
dan hoeft deze verandering alleen maar bij deze bron plaats te vinden.

  Een ander argument is dat deze gegevens dan minder ruimte in beslag nemen,
  omdat je allerlei duplicaten voorkomt.
  Dit argument is tegenwoordig, met de sterk gedaalde kosten van gegensopslag,
  minder relevant.

Gegevens met betekenis
======================

Bij een "normale" database speelt voor de computer de betekenis van de data geen rol:
je kunt gegevens opslaan in een bepaalde structuur, en je kunt ze terugzoeken volgens deze structuur.
De namen die je gebruikt in de structuur: de namen voor de kolommen (SQL) of voor de eigenschappen van documenten (MongoDB),
hebben voor de computer geen betekenis.
Dit zijn voornamelijk aanwijzingen voor menselijke gebruikers (applicatieprogrammeurs) over de betekenis van de bijbehorende gegevens.

Linked Data vormt een opstapje naar Semantische Databases.
In zo'n database beschrijf je de betekenis van de gegevens op zo'n manier dat deze ook door computers geïnterpreteerd kan worden.

Voorbeeld: in een database met familierelaties beschrijf je gewoonlijk de directe relaties, zoals "zoon van" of "ouder van".
Dit betekent dat je alleen deze relaties in queries kunt gebruiken.
Je kunt wel een complexere query maken om de grootouders van een bepaald persoon op te zoeken,
maar zo'n query is onderdeel van een toepassing, geen onderdeel van de database.

In een semantische database kun je indirecte relaties, zoals "grootouder van", "kleinkind van", "oom van" e.d. beschrijven met behulp van regels.
In een query kun je deze relaties gebruiken alsof deze deel uitmaken van de database:
alsof dit kolomnamen zijn, of eigenschappen van documenten.

Het afleiden van dergelijke indirecte relaties, op grond van de gegevens in de database en een reeks regels voor relaties,
heet ook wel *inferencing* ("afleiden").

Linked Data in het web
======================

Als je een restaurant hebt met een webpagina, dan wil je graag dat zoekmachines handig omgaan met gegevens als locatie, openingstijden en telefoonnummers.
Als je deze alleen in HTML beschrijft, op een manier die voor mensen leesbaar is, dan is het voor een zoekmachine niet duidelijk wat deze gegevens voorstellen.
Met behulp van Linked Data, in verschillende vormen (microdata, RDF, JSON-LD, enz.),

Zie: https://developers.google.com/search/docs/guides/intro-structured-data
