**************
MongoDB schema
**************

.. admonition:: Samenvatting

  Met een *schema* beschrijf je de namen en types van de *fields* (velden) van de *documents* in een *collection*.
  Deze *consistentie in namen en types* vormt de basis voor het efficiënt zoeken in die collection.

  Een MongoDB-schema is gewoonlijk *partieel*: je legt de minimale eisen aan een document vast.
  Een document kan naast het gemeenschappelijke deel extra fields bevatten.
  Als database-ontwerper bepaal je dan zelf de grens tussen de gemeenschappelijke structuur (schema) en het document-eigen deel.

Zoals we gezien hebben heeft in MongoDB elk document in een collection zijn eigen structuur:
een document is een voorbeeld van *self-describing data*.
Dit geeft een grote flexibiliteit, maar heeft ook een aantal nadelen:

- een collection bevat "soortgelijke documenten", maar het is niet duidelijk wat "soortgelijk" is;
- zoeken gaat efficiënter en effectiever voor gestandaardiseerde veldnamen en types;
- een tikfout in een veldnaam wordt niet opgemerkt.

Met behulp van een schema voor een collection geef je aan MongoDB op je welke velden (namen en types) de documenten in die collection
*tenminste* moeten hebben.
Naast deze gemeenschappelijke structuur kan een document extra velden bevatten.

MongoDB heeft twee schema-notaties.
We gebruiken hier de notatie op basis van een *query*-document:
alleen documenten die aan deze query voldoen zijn toegelaten in de betreffende collection.

  De andere notatie is met behulp van `JSON-Schema <https://json-schema.org>`_ :
  hiermee beschrijf je de structuur van JSON-documenten.

Het onderstaande query-document beschrijft alle documenten die een `name` en een `email`- of een `telephone`-veld hebben (``$or``).
In de opdracht daarna koppelen we dit query-document als *validator* (schema) aan de collection ``contacts``.
Na deze koppeling kun je alleen documenten die aan dit schema voldoen aan de collection ``contacts`` toevoegen.

.. code-block:: python

  contact_schema = {"name": {"$type": "string"},
                    "$or": [{"email": {"$type": "string"}},
                            {"telephone": {"$type": "string"}}

                           ]}
  db.command("collMod", "contacts", validator=contact_schema)

Op basis van deze validator kennen we de minimale structuur van de documents in de collection ``contacts``.
Deze structuur kunnen we gebruiken voor het zoeken naar documents in de collection.
Bovendien kunnen we dit gebruiken als basis voor een *index* waarmee we efficiënt kunnen zoeken.

**Opmerking.**
De collection kan documents bevatten die niet aan dit schema voldoen,
als die eerder toegevoegd zijn.
Dit probleem kan bijvoorbeeld optreden als je het schema voor een collection aanpast.
Het is aan de databaseprogrammeur om dit probleem op te lossen.

Vergelijking met relationele databases
======================================

Het fysieke schema van een relationele database beschrijft de structuur van de database:
de tabellen, kolomnamen, kolom-types, en constraints.
Dit is een beschrijving in een formele notatie (SQL), die door het DBMS verwerkt kan worden.
Het schema beschrijft welke data in de database opgeslagen worden, en welke data voor de database toelaatbaar zijn.
Het DBMS bewaakt deze eisen aan de data.

  Eén van de eisen van Codd aan een relationeel database-systeem is dat een schema zelf ook in een database opgeslagen wordt:
  *Rule 4: Dynamic online catalog based on the relational model:
  The data base description ("schema", ed.) is represented at the logical level in the same way as ordinary data,
  so that authorized users can apply the same relational language to its interrogation as they apply to the regular data.*
  (https://en.wikipedia.org/wiki/Codd%27s_12_rules)

Het schema beschrijft niet wat de *betekenis* van de kolomnamen is.
Voor het DBMS speelt deze betekenis geen rol.
De betekenis voor de gebruiker (en voor de toepassingen) kun je beschrijven in de *data dictionary*.
