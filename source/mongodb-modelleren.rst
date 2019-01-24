******************
MongoDB modelleren
******************

Bij het ontwerp van een MongoDB database heb je voor deel-documenten de keuze tussen *embedding* en *referencing*.
*Embedding* betekent dat je het deel-document opneemt in een omvattend document;
het deel-document heeft dan geen eigen key (`_id`).
Bij *referencing* is het deel-document een zelfstandig document, met een eigen key (`_id`).

Wat zijn de overwegingen bij het kiezen tussen deze alternatieven?

* als het deel-document een zelfstandige entiteit betreft, die je ook los van het omvattende document kunt gebruiken,
  dan kies je voor *referencing*.
* als het deel-document in
    * verschillende omvattende documenten kunnen naar hetzelfde deel-document verwijzen,
      en zo gegevens delen;
    * voor onderdelen van het deel-document kun je *embedding* gebruiken,
      als *caching*.
* als het deel-document alleen relevant is als onderdeel van het omvattende document,
  dan gebruik je *embedding*.

Je kunt voor *referencing* kiezen in combinatie met embedding van enkele onderdelen.

<<fig embedding>>

<<fig referencing>>

Voorbeeld: in het geval van de agenda-(event)database gebruiken we *referencing* voor de documenten van de deelnemers.
Maar voor enkele onderdelen van deze deelnemer-documenten gebruiken we *embedding*:
we nemen bijvoorbeeld de namen van de deelnemers op in het agenda-(event)-document.
De overwegingen hierbij zijn:
(i) bij het gebruik van een event-document hebben we vrijwel altijd de namen van de deelnemers nodig;
(ii) de naam van een persoon in het persoon-document verandert niet.

  We hebben hier mogelijk een probleem als de naam van een persoon eventueel wel verandert.
  Maar: (i) we kunnen altijd stellen dat het de naam van de persoon ten tijde van het maken van de afspraak was;
  (ii) of, we kunnen met een lage-prioriteit proces dergelijke veranderingen in de rest van de database doorvoeren.

Vergelijking met relationele databases
======================================

Bij relationele databases ligt de nadruk op *consistentie*,
bij MongoDB op *beschikbaarheid* (snelheid).
Een MongoDB database is *eventual consistent*:
de database wordt wel consistent, maar voor de gebruikers kan het even duren voordat veranderingen zichtbaar zijn.

Bij MongoDB ga je uit van kennis van de gebruikte toepassing(en):
je weet dan welke queries veel voorkomen, en je past het ontwerp van de database daarop aan.
Een relationele database is veel minder gericht op specifieke toepassingen,
of op bepaalde queries.

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
