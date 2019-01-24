*********
Inleiding
*********

In dit onderdeel maken we kennis met MongoDB als voorbeeld van een NoSQL-database,
en met Linked Data.

Als voorbeeld voor beide gebruiken we een database met contacten en events,
zoals je deze bijvoorbeeld kunt gebruiken voor een contacten-toepassing
en een agenda-toepassing op een smartphone.

  Voor de praktische opdrachten gebruiken we Jupyter Notebooks.
  Hierin combineren we MongoDB met Python.

De nadruk bij het onderwerp "databases" ligt meestal op *relationele databases*.
Dit is voor een deel terecht: deze zijn nog steeds belangrijk voor een groot aantal toepassingen.

Maar net zoals er verschillende programmeertalen zijn en verschillende operating systems,
zijn er verschillende soorten databases.
Er zijn grote verschillen tussen de data en tussen de toepassingen die deze data gebruiken:
deze zijn niet met één enkele technologie op te lossen.

Het kennismaken met meerdere soorten databases helpt ook om elk van deze soorten beter te begrijpen.

Voorbeeld-model
===============

Zoals gezegd gebruiken we als voorbeeld *contacten* en *events*.
We beschrijven dit voorbeeld ook met een eenvoudig (E-R) model.

Dit voorbeeld heeft een aantal karakteristieken die het minder geschikt maken voor een relationele database,
en meer voor een document-database zoals MongoDB:

* elk contact heeft een eigen structuur: van het ene contact ken je alleen de naam en het email-adres,
  van een ander ken je ook een aantal telefoonnummers, het woonadres en misschien nog meer.
*

MongoDB
=======

We gebruiken MongoDB als voorbeeld van een NoSQL-database.
Een MongoDB-database bestaat uit een aantal *collections* (vgl. de tabellen van een relationele database).
Een *collection* bevat een aantal soortgelijke *documenten* (vgl. de rijen van een relationele database).
Elk document in een collection heeft zijn eigen structuur:
de namen en types van de velden.
MongoDB eist niet dat alle documenten in eenzelfde collection dezelfde structuur hebben.

Schema
======

De structuur van een document bestaat uit de namen en de types van de velden.

  In een relationele database beschrijft het *schema* van een database de namen van de tabellen en de structuur daarvan:
  de namen en de types van de kolommen.

Zoals we gezien hebben kunnen de documenten in een collection verschillende structuren hebben.
Maar als we queries willen formuleren, voor het selecteren van documenten,
en voor het samenvatten van een reeks geselecteerde documenten (aggregatie),
dan moeten deze documenten in elk geval voor een deel eenzelfde structuur hebben.
Deze gemeenschappelijke structuur van de documenten in een *collection* noemen we het (partiële) *schema*.

In MongoDB kunnen we via voor elke collection een *validator*-query definiëren,
waarmee we kunnen de gemeenschappelijke (deel)structuur van de documenten in een collection kunnen afdwingen.

Embedding en referencing
========================

Een MongoDB-document kan deel-documenten bevatten (*embedding*),
en naar andere documenten verwijzen (*referencing*, met behulp van de *key*).
Bij het opzetten van een MongoDB-database moet de voor elk (deel)document bepalen welke van deze twee mogelijkheden je gebruikt.

Hierbij spelen de volgende overwegingen een rol:

* onafhankelijke documenten: een document dat je afzonderlijk wilt kunnen gebruiken,
  onafhankelijk van andere documenten, dan maak je daarvan een apart document,
  waarnaar je vanuit andere documenten verwijst.
* normalisatie: als je wilt voorkomen dat dezelfde gegevens op meerdere plekken voorkomen,
  dan maak je daarvan een apart document, waarnaar je vanuit andere documenten verwijst.
* de-normalisatie: als je snel toegang wilt hebben tot bepaalde gegevens,
  dan is het soms handig om een document of een deel daarvan te embedden.
  Je krijgt dan een gede-normaliseerde model.

Normalisatie (of niet?)
-----------------------

Het voordeel van een genormaliseerd model is dat veranderingen (updates) eenvoudiger zijn,
omdat je gegevens maar op één plek hoeft te veranderen.
Een nadeel is dat je dan bij het opvragen van gegevens vaker een *join* nodig hebt,
waarin je de gegevens van meerdere documenten combineert.
Als snelle toegang belangrijker is dan eenvoud van updates en absolute consistentie van de database,
dan gebruik je eerder embedding, voor een niet-genormaliseerde vorm.

  In ons voorbeeld bevat een event-document de verwijzingen (references) naar de bijbehorende deelnemers.
  Als optimalisatie voor snelle toegang embedden we de namen van deze deelnemers in het event-document.

Join
====

Als je referencing gebruikt, dan heb je soms de gegevens uit meerdere documenten nodig.
je gebruikt een *join* om gegevens uit meerdere documenten te combineren.
Deze join moet in de toepassing geprogrammeerd worden:
MongoDB biedt daarvoor geen ondersteuning.

  In SQL zorgt het DBMS voor het uitvoeren van de join.

Linked Data
===========

De database-schema's die we hierboven genoemd hebben zijn beperkt tot de database.
Als je gegevens met andere databases wilt uitwisselen,
of als je gegevens uit verschillende databases wilt combineren,
dan heb je behoefte aan een *globaal schema*.
De Linked Data aanpak probeert dit te bereiken door elementen uit het web te gebruiken:
voor namen van types en velden en voor de identificatie van entiteiten ("keys") gebruik je URLs.
