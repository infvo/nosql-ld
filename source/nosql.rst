**************
NoSQL: MongoDB
**************

In dit hoofdstuk behandelen we MongoDB als  voorbeeld van een NoSQL database.
NoSQL betekent dat we niet met een klassieke relationele database te maken hebben.
Als vraagtaal wordt soms wel een variant van SQL gebruikt: NoSQL betekent dan "not only SQL".

We geven eerst een samenvatting van een aantal belangrijke eigenschappen van relationele databases.
Daarna geven we een aantal eigenschappen van MongoDB, om het verschil duidelijk te maken.

Als voorbeeld gebruiken we een database met *contacten*, bijvoorbeeld personen en organisaties.
Een dergelijke database is eenvoudiger te maken met MongoDB dan met een relationele database.

(In een ander hoofdstuk beschrijven we Linked Data; we geven aan hoe je dat in combinatie met MongoDB kunt gebruiken.)

Relationele databases
=====================

.. rubric:: Stuctuur van de data

Een relationele database bestaat uit een aantal tabellen.
Een tabel bestaat uit rijen (records) en kolommen (de velden van de records).
Alle rijen in een tabel hebben dezelfde structuur (dezelfde velden):
de rijen zijn *gelijkvormig*.
Sommige velden in een rij kunnen een lege waarde bevatten (NULL);
een lege waarde kun je zien als een "gat" in de rij.

.. rubric:: Beschrijving van de data

Het *fysieke schema* van een relationele database beschrijft voor elke tabel:
de naam van de tabel, en de namen en types van de kolommen.

Dit kun je vergelijken met *statische typering* in programmeertalen als Pascal en Java.
Je geeft hiermee voor het DBMS aan welke de waarden in de tabellen kunnen voorkomen.
Bovendien vormt dit de documentatie voor de toepassingsprogrammeurs en andere gebruikers van de database.

MongoDB
=======

.. rubric:: Stuctuur van de data

Een MongoDB-database bestaat uit een aantal *collections*.
Een collection bevat *gelijksoortige documenten*.
Deze documenten hoeven niet gelijkvormig te zijn:
ze kunnen een verschillende structuur hebben.

  Dit is een belangrijk verschil met een relationele ("SQL") database,
  waarbij alle rijen in een tabel dezelfde structuur hebben.

.. rubric:: Beschrijving van de data

Elk document in de database bevat zijn eigen beschrijving: de namen van de velden (properties) en het type van de waarde.
Dit noemen we ook wel "self describing data".

Voorbeeld van enkele documenten:

.. code-block:: JSON

  {"name": "Adrie Vierhuis",
   "address": {"street": "Beukenweg 12",
               "city": "Rotterdam",
               "postcode": "2001 BC"
              },
    "tel": "010-123 123 9"
  }

.. code-block:: JSON

  {"name": "Anna Verschuur",
   "email": "anna@hotmail.com",
   "address": {"street": "Noorderkade 102",
               "city": "Amsterdam",
               "postcode": "1000 AA"
              }
  }

Dit kunnen documenten in eenzelfde *collection* zijn:
elk document heeft een eigen structuur,
met meestal de nodige overlap tussen de structuur van de verschillende documenten in eenzelfde *collection*.
Later komen we daarop terug.

Elke waarde heeft een eigen type; er is geen vaste koppeling tussen een veld(naam) en een type.
Dit kun je vergelijken met de dynamische typering in programmeertalen als Python en JavaScript.

.. todo::

  * voorbeelden van andere waarden in het voorbeeld-document opnemen.

Andere voorbeelden van "self describing data" zijn JSON en XML.

JSON en BSON
============

JSON (JavaScript Object Notation) is een tekstformaat voor het uitwisselen van objecten tussen verschillende programma's.
Veel programmeertalen bieden de mogelijkheid om objecten in JSON-formaat te schrijven en in te lezen.
JSON wordt onder andere gebruikt voor de uitwisseling van data tussen de browser (web client) en de web server,
in AJAX-interacties.

JSON heeft een aantal beperkingen:

* een JSON-object kan *geen functies* bevatten (alleen data);
* een JSON-object kan geen cykels bevatten (het moet een "plat" object zijn).

Er is geen standaardnotatie voor verwijzingen tussen JSON-objecten:
dat moet je als programmeur zelf oplossen.

Enkele voorbeelden van objecten in JSON, JavaScript en Python (dictionary):

.. code-block:: JSON

.. code-block:: Python

.. code-block:: JavaScript

.. todo::

  * voorbeelden van JSON/JS/Python-objecten

Zoals je ziet is er een grote overeenkomst tussen de JSON-notatie en de notatie van een Python dictionary.

MongoDB gebruikt een variant van JSON: BSON, voor de notatie van documenten in de database.
BSON bevat meer types dan JSON, en heeft een standaardnotatie voor verwijzingen naar andere documenten.

Collections
===========

Vergelijking
============

+--------------+--------------+
| SQL          | MongoDB      |
+--------------+--------------+
| tabel        | collection   |
+--------------+--------------+
| row (record) | document     |
+--------------+--------------+
