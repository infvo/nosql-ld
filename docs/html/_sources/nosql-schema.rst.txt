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
Dit geeft een grote flexibiliteit, maar het staat het efficiënt zoeken van documenten in de database in de weg.

Met behulp van een schema beschrijf je welke fields (namen en types) de documents in een collection
*tenminste* moeten hebben.
Deze gemeenschappelijke structuur vormt de basis voor efficiënt zoeken in de collection.
Naast deze gemeenschappelijke structuur kan een document extra fields bevatten.

MongoDB heeft twee schema-notaties.
We gebruiken hier de notatie op basis van een *query*-document:
alleen documents die aan deze query voldoen zijn toegelaten in de betreffende collection.

  De andere notatie is met behulp van `JSON-Schema <https://json-schema.org>`_ :
  hiermee beschrijf je de structuur van JSON-documenten.

Het onderstaande query-document beschrijft alle documents die een `name` en een `email`- of een `tel`-field hebben (``$or``).
In de opdracht daarna koppelen we dit query-document als *validator* (schema) aan de collection ``contacts``.
Na deze koppeling kun je alleen documents die aan dit schema voldoen aan de collection ``contacts`` toevoegen.

.. code-block:: python

  contact_schema = {"name": {"$type": "string"},
                    "$or": [{"email": {"$type": "string"}},
                            {"tel": {"$type": "string"}}

                           ]}
  db.command("collMod", "contacts", validator=contact_schema)

Op basis van deze validator kennen we de minimale structuur van de documents in de collection ``contacts``.
Deze structuur kunnen we gebruiken voor het zoeken naar documents in de collection.
Bovendien kunnen we dit gebruiken als basis voor een *index* waarmee we efficiënt kunnen zoeken.

**Opmerking.**
De collection kan documents bevatten die niet aan dit schema voldoen,
als die eerder toegevoegd zijn.
Het is aan de databaseprogrammeur om dit probleem op te lossen.
