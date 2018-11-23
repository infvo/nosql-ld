***********
Linked Data
***********

.. admonition:: Samenvatting

  De namen van de kolommen of eigenschappen hebben gewoonlijk alleen betekenis in de database zelf,
  en in de bijbehorende programma's.
  Buiten deze omgeving hebben de namen geen betekenis:
  dat maakt onder andere het koppelen van gegevens uit verschillende databases lastig.

  Door namen te gebruiken die wereldwijd dezelfde zijn, bij voorkeur met een duidelijke en gestandaardiseerde betekenis,
  kunnen we data ook buiten de oorspronkelijke database betekenis geven.
  Deze aanpak van Linked Data is daarmee voor het koppelen van databases wat het internet is voor computernetwerken.

  Door de context voor de interpretatie van de namen van de eigenschappen expliciet te maken,
  maak je deze interpretatie onafhankelijk van de context waarin deze data gebruikt wordt.

Met behulp van een *schema* kunnen we beschrijven hoe de data in een database eruit ziet.
De namen van de kolommen (bij een relationele database) of van de eigenschappen (bij een document-database) kunnen we daarbij vrij kiezen.
Vanuit het database-systeem (DBMS) bezien is de enige eis dat we deze namen consistent gebruiken.

In plaats van de kolomnaam of eigenschap ``address`` kunnen we ``banaan`` gebruiken: voor de computer maakt dat niet uit,
als we dit deze naam maar consistent gebruiken.

Een naam als ``address`` is in eerste instantie bedoeld om de betekenis van de eigenschap voor de menselijke gebruikers van de database duidelijk te maken.
Daarnaast hebben we vaak nog de mogelijkheid om aan het schema commentaar toe te voegen, als extra documentatie.
Ook deze is alleen bedoeld voor interpretatie door mensen.

Deze lokale standaardisatie van de data in de database heeft twee beperkingen:

1. deze biedt geen ondersteuning voor de uitwisseling van data, of voor het koppelen van bestanden;
2. er is geen interpretatie door computers mogelijk, om bijvoorbeeld een telefoonnummer in de database te koppelen aan de telefoon-applicatie.

Met de Linked Data aanpak die we hier beschrijven los je het eerste probleem op,
en maak je een belangrijke stap voor het oplossen van het tweede probleem.
(Hier gaan we op dat laatste niet verder in: je komt dan in de wereld van de Semantische Databases en het Semantische web.)

Linked Data is een middel om de namen en types van velden te standaardiseren zodat data ook buiten de oorspronkelijke database betekenis heeft.
Hierbij gebruik je de principes van het web voor het beschrijven van data.

We gebruiken het voorbeeld van de contacten-database om e.e.a. te verduidelijken.

Een voorbeeld van enkele documenten uit deze database:

.. code-block:: JSON

  {"name": "Hans de Jong",
   "tel": "06-1234 5599"}

  {"name": "Anna Kanis",
   "address": {"street": "Brink 23",
               "city": "Assen"}}

Zoals gezegd hebben de namen die we hier gekozen hebben, zoals ``tel`` en ``name``,
alleen lokale betekenis, binnen deze database en de bijbehorende toepassingsprogramma's.

We gebruiken de principes van het web om globale namen te maken.
Een URI (uniform resource identifier) is een naam in het web:
deze zorgt voor de identificatie van een *resource*.
Een *resource* is een "ding": dit kan een concreet iets of iemand zijn,
maar ook een abstract begrip.
Een URL is een URI met de extra eigenschap dat je deze als webadres kunt gebruiken.
Via een browser kun je dan een document ophalen wat deze resource beschrijft.

  Bij Linked Data wordt vaak IRI gebruikt, in plaats van URI:
  dit heeft onder meer te maken met het gebruik van verschillende soorten talen,
  bijvoorbeeld talen die van rechts naar links gelezen worden.

Schema.org beschrijft op deze manier een groot aantal begrippen,
onder andere begrippen die we in de contacten-database kunnen gebruiken.
Bijvoorbeeld:

* https://schema.org/Person
* https://schema.org/familyName
* https://schema.org/givenName
* https://schema.org/name
* https://schema.org/telephone

We zouden nu deze URLs kunnen gebruiken als de namen van de velden:

.. code-block:: JSON

  {"https://schema.org/name": "Hans de Jong",
   "https://schema.org/telephone": "06-1234 5599"}

We krijgen op deze manier gestandaardiseerde veldnamen, maar ook behoorlijk onleesbare documenten.
Bedenk bovendien dat we deze veldnamen ook gebruiken in queries e.d.:
deze lange namen maken de programma's voor deze database veel minder leesbaar.

Door middel van een *context* kunnen we aangeven hoe onze eigen lokale namen gekoppeld zijn aan deze globale namen:
(Zie https://json-ld.org)

.. code-block:: JSON

  {"@context":
    {"name": "https://schema.org/name",
     "tel": "https://schema.org/telephone"
    },
   "name": "Hans de Jong",
   "tel": "06-1234 5599"
   }

Met behulp van speciale functies, in Python ``jsonld.expand(doc)`` en ``jsonld.compact(expandedDoc, context)``,
kun je een compacte vorm omzetten in een uitgebreide vorm (met URIs als namen van eigenschappen) en omgekeerd.



Hoe wordt Linked Data gebruikt?
===============================

Waar en op welke manieren wordt Linked Data nu gebruikt?

In websites, om gestructureerde data (in het html-document) aan te bieden voor zoekmachines.
Zie: https://developers.google.com/search/docs/guides/intro-structured-data.
Zoekmachines kunnen dan bijvoorbeeld de contactgegevens en de openingstijden van een restaurant op een gestandaardiseerde manier weergeven.
En je zou dan kunnen zoeken naar restaurants die op een bepaald tijdstip open zijn.

In web-APIs, om de data in deze APIs te beschrijven.
(Zie het voorbeeld in )

In `Wikidata <https://www.wikidata.org>`_, voor de gestructureerde data rond Wikipedia e.d.
Zie ook: `Wikidata data access <https://www.wikidata.org/wiki/Wikidata:Data_access>`_.

Referenties
===========

De vier regels voor Linked Data van Tim Berners-Lee (de uitvinder van het web):

1. Use URIs as names for things.
2. Use HTTP URIs (*URLs*) so that people can look up those names.
3. When someone looks up a URI, provide useful information, using the standards (RDF*, SPARQL - *or JSON-LD*).
4. Include links to other URIs. so that they can discover more things.

* `Tim Berners-Lee: Linked Data Principles <https://www.w3.org/DesignIssues/LinkedData.html>`_
* `linked data-best practices <https://json-ld.org/spec/latest/json-ld-api-best-practices/#bp-summary>`_
* https://schema.org
* https://json-ld.org
* https://moz.com/blog/json-ld-for-beginners
* https://datalanguage.com/news/publishing-json-ld-for-developers
* https://json-ld.org/primer/latest/#example-links-bike-shop
* https://www.youtube.com/watch?v=UmvWk_TQ30A
* https://developers.google.com/search/docs/guides/intro-structured-data
