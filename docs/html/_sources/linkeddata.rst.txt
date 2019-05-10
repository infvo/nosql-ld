***********
Linked Data
***********

.. admonition:: Samenvatting

  De namen van de kolommen of velden hebben alleen betekenis in de database zelf
  en in de bijbehorende toepassingsprogramma's.
  Buiten deze omgeving hebben de namen geen betekenis:
  dat maakt onder andere het koppelen van gegevens uit verschillende databases lastig.

  Door namen te gebruiken die wereldwijd dezelfde zijn, met een gestandaardiseerde betekenis,
  kunnen we data ook buiten de oorspronkelijke database betekenis geven.
  Deze aanpak van Linked Data is daarmee voor het koppelen van databases wat het internet is voor het koppelen van computernetwerken.

  Door de context voor de interpretatie van de namen van de velden expliciet te maken,
  maak je deze interpretatie onafhankelijk van de context waarin deze data gebruikt wordt.

Namen in Linked Data
====================

Met behulp van een *schema* beschrijf je de structuur van een database.
De namen van de kolommen (bij een relationele database) of van de velden (bij een document-database) kunnen we daarbij vrij kiezen.
Vanuit het database-systeem (DBMS) bezien is de enige eis dat we deze namen consistent gebruiken.

In plaats van de kolomnaam of veldnaam ``address`` kunnen we ``banaan`` gebruiken: voor de computer maakt dat niet uit,
als we deze naam maar consistent gebruiken.

  Een naam als ``address`` is bedoeld om de betekenis van het veld voor de gebruikers van de database duidelijk te maken.
  Daarnaast kunnen we aan het schema commentaar toevoegen, als extra documentatie.
  Ook deze is alleen bedoeld voor menselijke gebruik.

Deze lokale data-naamgeving in de database heeft de volgende beperkingen:

1. geen ondersteuning voor de uitwisseling van data, of voor het koppelen van bestanden;
2. geen interpretatie door computers mogelijk, om bijvoorbeeld een telefoonnummer in de database te koppelen aan de telefoon-applicatie.

Met de Linked Data aanpak die we hier beschrijven los je het eerste probleem op,
en maak je een belangrijke stap voor het oplossen van het tweede probleem.
(Op dat laatste gaan we hier niet verder in: je komt dan in de wereld van de Semantische Databases en het Semantische web.)

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

We gebruiken de *principes van het web* voor het maken van globale namen.
Voor naamgeving in het web gebruiken we URI's (uniform resource identifiers):
een URI identificeert een *resource*.
Een *resource* is een "ding": dit kan een concreet iets of iemand zijn,
maar ook een abstract begrip.
Een URL is een URI die je als webadres kunt gebruiken.
Via een browser kun je dan een document ophalen wat deze resource beschrijft.

  Een ISBN kun je ook als URI gebruiken, maar dat is geen URL.

  Bij Linked Data wordt vaak IRI gebruikt, in plaats van URI:
  dit heeft onder meer te maken met het gebruik van verschillende soorten talen,
  bijvoorbeeld talen die van rechts naar links gelezen worden.

Schema.org beschrijft op met behulp van URLs een groot aantal begrippen,
onder andere begrippen die we in de contacten-database kunnen gebruiken.
Bijvoorbeeld:

* https://schema.org/Person
* https://schema.org/familyName
* https://schema.org/givenName
* https://schema.org/name
* https://schema.org/telephone

We kunnen deze URLs gebruiken als veldnamen:

.. code-block:: JSON

  {"https://schema.org/name": "Hans de Jong",
   "https://schema.org/telephone": "06-1234 5599"}

Zo krijgen we gestandaardiseerde veldnamen, maar ook behoorlijk onleesbare documenten.
Bedenk dat we deze veldnamen ook gebruiken in queries e.d.:
zulke lange namen maken de toepassingsprogramma's voor deze database veel minder leesbaar.

Door middel van een *context* kunnen we onze eigen lokale namen koppelen aan deze globale namen:
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
kun je een compacte vorm omzetten in een uitgebreide vorm met URIs als namen van eigenschappen en omgekeerd.

.. Admonition:: 4 regels voor Linked Data

  Tim Berners-Lee, de uitvinder van het world-wide web, formuleerde de volgende 4 regels voor Linked Data:

  1. Use URIs as names for things.
  2. Use HTTP URIs (*URLs*) so that people can look up those names.
  3. When someone looks up a URI, provide useful information, using the standards (RDF*, SPARQL - *or JSON-LD*).
  4. Include links to other URIs, so that they can discover more things.


Types in Linked Data
====================

De Linked Data notatie maakt het ook mogelijk om de types van de velden te beschrijven.
Er zijn twee soorten types in JSON-LD: *object types*, zoals ``Person`` of ``Event``,
en *data types*, zoals ``integer``, ``boolean``, ``string``, maar ook ``date``, ``temperature``.
Ook voor types gebruik je weer URIs.

Met behulp van het ``@type``-veld in het document geef je het object-type van het document aan.
In het onderstaande voorbeeld geeft dit aan dat het document een "Person" beschrijft.

.. code-block:: JSON

  {"@context":
    {"name": "https://schema.org/name",
     "tel": "https://schema.org/telephone",
     "homepage": "http://xmlns.com/foaf/0.1/homepage"
    },
   "@type": "https://schema.org/Person",
   "name": "Hans de Jong",
   "tel": "06-1234 5599",
   "homepage": "https://hans.nl"
  }

Je kunt ook in de context aangeven wat de types zijn van de velden.
In dat geval heb je naast ``@type`` ook ``@id`` nodig, om de webnaam (URI of IRI) aan te geven:

.. code-block:: JSON

  {"@context":
    {"name": "https://schema.org/name",
     "tel": "https://schema.org/telephone",
     "age": {
       "@id": "http://xmlns.com/foaf/0.1/age",
       "@type": "xsd:integer"
     },
     "homepage": {
       "@id": "http://xmlns.com/foaf/0.1/homepage",
       "@type": "@id"
     }
    },
   "@type": "https://schema.org/Person",
   "name": "Hans de Jong",
   "tel": "06-1234 5599",
   "age": 35,
   "homepage": "https://hans.nl"
   }

**Opmerking:** in het algemeen is het niet verstandig om veranderlijke ("volatile") gegevens zoals  ``age`` op te slaan.
We gebruiken dit hier alleen als type-voorbeeld.

Gebruik van Linked Data
=======================

Waar en op welke manieren wordt Linked Data gebruikt?

In html-documenten (websites), om (semi-)gestructureerde data aan te bieden voor zoekmachines.
Zie: https://developers.google.com/search/docs/guides/intro-structured-data.
Zoekmachines kunnen dan bijvoorbeeld de contactgegevens en de openingstijden van een restaurant op een gestandaardiseerde manier weergeven.
Daarmee kun je zoeken naar restaurants die op een bepaald tijdstip open zijn.

In web-APIs, om de data in deze APIs te beschrijven.
(Zie het voorbeeld in XXX)

.. todo::

  web-API voorbeeld uitwerken.

In `Wikidata <https://www.wikidata.org>`_, voor de gestructureerde data rond Wikipedia e.d.
Zie ook: `Wikidata data access <https://www.wikidata.org/wiki/Wikidata:Data_access>`_.

Referenties en verdiepding
==========================

De vier regels voor Linked Data van Tim Berners-Lee (de uitvinder van het web):

1. Use URIs as names for things.
2. Use HTTP URIs (*URLs*) so that people can look up those names.
3. When someone looks up a URI, provide useful information, using the standards (RDF*, SPARQL - *or JSON-LD*).
4. Include links to other URIs, so that they can discover more things.

* `Tim Berners-Lee: Linked Data Principles <https://www.w3.org/DesignIssues/LinkedData.html>`_
* `linked data-best practices <https://json-ld.org/spec/latest/json-ld-api-best-practices/#bp-summary>`_
* https://schema.org
* https://json-ld.org
* https://moz.com/blog/json-ld-for-beginners
* https://datalanguage.com/news/publishing-json-ld-for-developers
* https://json-ld.org/primer/latest/#example-links-bike-shop
* https://www.youtube.com/watch?v=UmvWk_TQ30A
* https://developers.google.com/search/docs/guides/intro-structured-data
