********************
MongoDB fysiek model
********************

Bij het omzetten van een conceptueel model naar een fysiek model in de vorm van een MongoDB-database heb je meer mogelijkheden dan bij een relationele database.

In de eerste plaats kun je in MongoDB samengestelde attributen (*composite attributes*) en
meerwaardige attributen (*multivalued attributes*) direct in een document weergeven.
Daarnaast kun je voor relaties kiezen tussen *embedding* en *referencing*.

Samengesteld attribuut
======================

Een voorbeeld van een samengesteld attribuut is een *address*:
dit bestaan uit de deel-attributen *street*, *city* en *postcode*.
In MongoDB kun je een samengesteld attribuut als deel-document in een document opnemen (*embedden*),
bijvoorbeeld:

.. code-block:: json

  {
    "name": "F.G. Schuitema",
    "email": "fg@schuitema.com",
    "address": {
      "street": "Eikenlaan 23",
      "city": "Amsterdam",
      "postcode": "1001 AB"
    }
  }

Meerwaardig attribuut
=====================

In het contacten-voorbeeld kan een *contact* meerdere email-adressen hebben.
``email`` is daar een *meerwaardig attribuut* (*multivalued attribute*).
In MongoDB geef je dit weer door middel van een *array*:


.. code-block:: json

  {"name": "F.G. Schuitema",
   "email": ["fg@schuitema.com", "fred.schuitema@gmail.com"],
   "address": {"street": "Eikenlaan 23",
               "city": "Amsterdam",
               "postcode": "1001 AB"
              }
  }

Soms wil je onderscheid maken tussen de verschillende soorten waarden,
zoals tussen een privé-emailadres en een zakelijk email-adres.
Dit modelleer je door van elk email-adres een samengesteld attribuut te maken,
met een extra attribuut om het type aan te geven.

.. todo::

  voorbeeld van meerwaardig attribuut met onderscheid.

..


Relatie: embedding of referencing
=================================

Voor een relatie van entiteit A naar entiteit B heb je twee mogelijkheden:

* *referencing*: verwijs vanuit het A-document naar het B-document, via de *foreign key* van B;
* *embedding*: neem het B-document op als deel-document van het A-document.

*Opmerking:* eigenlijk is er sprake van een relatie *tussen* A en B.
Afhankelijk van de aard van de relatie kun je dit weergeven als een relatie van A naar B,
van B naar A, of allebeide; zie verderop.

Als eerste voorbeeld gebruiken we de relatie ``attends`` tussen Event en Contact.
We willen deze weergeven als een relatie van Event naar Contact:
een Event bevat een lijst van ``attendee`` s.

Referencing
-----------

De eerste mogelijkheid is een lijst (array) *references* van Event naar Contact.
Als reference van een contact gebruiken we de key daarvan;
in een event-document is dat een *foreign key*.

Voorbeeld:

.. code-block::

  {'_id': ObjectId('5cd3304cd09dd5773bee4f04'),
    'subject': 'Beleidsplan',
    'time': datetime.datetime(2019, 3, 1, 0, 0),
    'duration': 60,
    'participants': [ObjectId('5cd3dfe0f58e1b79c042122a'),
                 ObjectId('5cd3dfe0f58e1b79c0421230')],
    'location': 'Seats2Meet Den Bosch'
  }

Dit event-document verwijst naar de volgende contact-documenten:

.. code-block::

  {'_id': ObjectId('5cd3dfe0f58e1b79c042122a'),
   'name': 'Hans de Boer',
   'email': 'hdb@example.com',
   'tel': '06-1290 8746'}

  {'_id': ObjectId('5cd3dfe0f58e1b79c0421230'),
   'name': 'Adrie Vierhuis',
   'email': 'a34huis@gmail.com',
   'address': {'street': 'Beukenweg 12',
               'city': 'Rotterdam',
               'postcode': '2001 BC'},
   'tel': '010-123 123 9'}

Met behulp van de key (``ObjectId(...)``) kun je het bijbehorende contact-document opzoeken.
Dit opzoeken moet je in MongoDB in de toepassing programmeren:
MongoDB heeft geen *join*-opdracht.

Voorbeeld: toevoegen van de namen van de deelnemers aan een afspraak.
We nemen hierbij aan dat de database *consistent* is:
de objecten waarnaar verwezen wordt bestaan in de database.
in MongoDB moet je er (via de toepassingen) zelf voor zorgen dat de database consistent blijft.

.. code-block:: python

  def addNames(agendaItem):
      participants = eventItem["participants"]
      for person in participants:
          pList = list(contacts.find_one({"_id": person["id"]})) ## (1) find
          person["name"] = pList[0]["name"]  ## (2) toevoegen van naam

Gedeeltelijke embedding
-----------------------

Bij het tonen van een afspraak in een agenda wil je ook de namen en mail-adressen van de deelnemers laten zien.
Het is omslachtig om hiervoor steeds de betreffende contact-documenten op te zoeken.
Een mogelijke optimalisatie is het *embedden* van deze velden in het event-document van de afspraak.
Je krijgt dan de volgende situatie:

.. code-block::

  {'_id': ObjectId('5cd3304cd09dd5773bee4f04'),
   'description': 'Beleidsplan',
   'time': datetime.datetime(2019, 3, 1, 0, 0),
   'duration': 60,
   'participants': [{'email': 'hdb@example.com',
                     'id': ObjectId('5cd3dfe0f58e1b79c042122a'),
                     'name': 'Hans de Boer'},
                    {'email': 'a34huis@gmail.com',
                     'id': ObjectId('5cd3dfe0f58e1b79c0421230'),
                     'name': 'Adrie Vierhuis'}],
   'location': 'Seats2Meet Den Bosch'}

Merk op dat deze vorm *niet genormaliseerd* is:
de namen en mail-adressen van deze contacten staan zowel in het event-document als in de bijbehorende contact-documenten.
Maar omdat deze gegevens niet veranderen vormt dat geen risico voor de consistentie.

Embedding
---------

.. todo::

  Voorbeeld(en) van volledige embedding (van een Entiteit).

  * (samengesteld attribuut: adres)
  * agenda-item (event) met contactgegevens: email-adres, tel.nr.; versus contact als zelfstandig
  * agenda-item met gedeeltelijke contactgegevens (partial embedding).
  * order met order-lines (mogelijk ook samengesteld attribuut)

Relatie-patronen
================

We geven hier een aantal handige patronen voor het weergeven van relaties in MongoDB.
Elk patroon heeft voor- en nadelen;
het hangt van de omstandigheden af of een bepaald patroon een geschikte keuze is.

In het conceptuele model zijn relaties symmetrisch, dat wil zeggen:
je spreekt over een relatie *tussen* A en B.
Je kunt deze in de database weergeven als een relatie van A naar B,
van B naar A, of beide.
Als je alleen de relatie van A naar B in de database opneemt,
heb je een zoekopdracht nodig om de relatie van B naar A te reconstrueren.

Voorbeeld: we geven de relatie *participates in* weer door de ``participants`` op te nemen in de event-documenten.
Als we willen weten wat de events waaraan een bepaald persoon deelneemt,
moeten we in de event-collection zoeken naar documenten met deze persoon als ``participant``.
Dit zoeken kunnen we versnellen door voor ``participants`` een index op te nemen.

* 1-1: je kunt hier zowel kiezen voor embedding als voor referencing;
* 1-N: in het algemeen: handiger om embedding/referencing te doen aan de N-kant,
  zodat je een embedding of referencing van een enkel document krijgt;
* N-M: (de oplossing voor relationele databases is om hier een extra tabel voor te maken).
  Als één van de N of M klein is (tientallen) en niet of nauwelijks groeit,
  dan kun je "de andere kant" gebruiken voor embedding of referencing.
  (NB: dit geval staat niet in de lijst van patterns van MongoDB.)

*Opmerking:* hoewel je in MongoDB arrays kunt opnemen in een document,
zijn grote arrays die veranderen en groeien minder efficiënt.
Je gebruikt dan bij voorkeur de referentie die klein is en/of weinig verandert.
*Voorbeeld:* het aantal deelnemers van een bijeenkomst (agenda-item) is klein, en verandert nauwelijks.
Het aantal bijeenkomsten per persoon in een agenda is groot, en verandert regelmatig.
Dan geef je de relatie tussen personen en agenda-items weer als verwijzing van agenda-item naar persoon.

Vergelijking met relationele databases
======================================

Bij relationele databases ligt de nadruk op *consistentie*,
bij MongoDB op *beschikbaarheid* (snelheid).
Een MongoDB database is *eventual consistent*:
de database wordt wel consistent, maar voor de gebruikers kan het even duren voordat veranderingen zichtbaar zijn.

Bij MongoDB ga je uit van kennis van de gebruikte toepassing(en):
je weet dan welke queries veel voorkomen, en je past het ontwerp van de database daarop aan.
Een relationele database is onafhankelijk van de toepassingen.

Of een entiteit een "zelfstandig document" is of niet, hangt af van het domein dat je modelleert.

In principe kun je een Entity-Relationship-model ook gebruiken voor een MongoDB-database.

(Soms wordt er -in een E-R-model- een onderscheid gemaakt tussen een *strong entity* en een *weak entity*.
Een weak entity is niet zelfstandig:
dat betekent dat je deze in een MongoDB-database zult embedden in het document van de bijbehorende strong entity.)

In een E-R model kan er sprake zijn van *multi-valued attributes*.
Deze kun je in MongoDB rechtstreeks in het document opnemen.
(In een relationele database moet je daarvoor een aparte tabel introduceren.)

(Voorbeeld: OrderItem als onderdeel van Order; of Participant als onderdeel van Event;
of Address als onderdeel van Contact?)

Zie ook: MongoDB modeling,

Normalisatie (of niet)
----------------------

Een relationele database probeer je zoveel mogelijk te normaliseren.
Dit betekent dat je elk gegeven maar één keer in de database opneemt.
Als dat gegeven verandert, hoef je dat maar één plaats in de database aan te passen.
Deze aanpak stelt daarmee de consistentie en integriteit van de database voorop.

Een nadeel van deze aanpak is dat je vaak gegevens uit meerdere tabellen moet combineren.
Dit gaat ten koste van de *beschikbaarheid* van de gegevens (snelheid van toegang).

In MongoDB staat in het algemeen de beschikbaarheid voorop;
onmiddellijke consistentie is minder van belang.
Door het embedden van eenzelfde gegeven in meerdere documenten krijg je een redundante vorm.
Het aanpassen van dit gegeven op een consistente manier is lastig, en kost meer tijd;
maar voor typische MongoDB-toepassingen is dat minder belangrijk.

Voor gegevens die op meerdere plaatsen gebruikt worden kun je in MongoDB kiezen voor een genormaliseerde vorm,
door het gebruik van *referencing*;
of voor een niet-genormaliseerde vorm, door het gebruik van *embedding*.

Samengesteld attribuut
----------------------

In een relationele database kun je een samengesteld attribuut *embedden* door het "plat te slaan".
Je combineert bijvoorbeeld de namen van het attribuut en van de deelattributen tot nieuwe kolomnamen,
zoals ``address_street``, ``address_city``, ``address_postcode``.
Het oorspronkelijke deel-document vind je dan niet meer terug.

Meerwaardig attribuut
---------------------

In een relationele database kun je meerwaardige attributen niet direct opnemen.
Je moet daarvoor een aparte tabel introduceren.

Relaties tussen entiteiten
--------------------------
Bij een relationele database heeft een Entiteit altijd een eigen tabel.
Een relatie geef je dan weer via een *foreign key*-verwijzing.

Join
----

In SQL gebruik je de join-opdracht voor het combineren van gegevens uit verschillende tabellen.
Dit is een operatie van het database-systeem, deze hoef je niet in de toepassing uit te werken.

Consistentie
------------

Een relationeel database-systeem (RDBMS) bewaakt de *constraints* van de database,
zoals de aanwezigheid van records (rijen) waarvoor elders een verwijzing (via een foreign key) bestaat.
De consistentie van de database hangt dan niet af van de gebruikers of de gebruikerstoepassingen.
