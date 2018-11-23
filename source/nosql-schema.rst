**************
MongoDB schema
**************

.. admonition:: Samenvatting

  Met een *schema* beschrijf je de namen en types van de eigenschappen (*properties*) van de documenten in een *collection*.
  Deze *consistentie in namen en types* vormt de basis voor het (efficiënt) zoeken in die collection.

  Een MongoDB-schema kan ook *partieel* zijn: dat beschrijft een deel van de eigenschappen van de documenten.
  Als database-ontwerper bepaal je dan zelf de grens tussen de gemeenschappelijke structuur (schema) en het document-eigen deel.

Zoals we gezien hebben heeft in MongoDB elk document in een collection zijn eigen structuur:
een document is een voorbeeld van *self-describing data*.
Dit geeft een grote flexibiliteit, maar het staat het efficiënt zoeken van documenten in de database in de weg.

Met behulp van een schema kunnen we beschrijven en afdwingen welke eigenschappen (properties) de documenten in een collection
*tenminste* moeten hebben.
Deze gemeenschappelijke structuur vormt de basis voor efficiënt zoeken in de collection.
Daarnaast kan een document altijd eigenschappen hebben die afwijken van deze gemeenschappelijke structuur.

In MongoDB kun je een dergelijke structuur (het *schema*) op verschillende manieren aangeven.
We gebruiken hier de aanpak met behulp van een *query*-structuur:
alleen documenten die aan deze query voldoen zijn toegelaten in de betreffende collection.

  De andere manier is met behulp van `JSON-Schema <https://json-schema.org>`_ :
  hiermee beschrijf je de structuur van JSON-documenten.

Het onderstaande query-document beschrijft alle documenten die een `naam` en een `email`- of een `tel`-eigenschap hebben (``$or``).
In de opdracht daarna koppelen we dit query-document als *validator* (schema) aan de collection ``contacts``.
Na deze koppeling kun je alleen documenten die aan dit schema voldoen aan de collection ``contacts`` toevoegen.

.. code-block:: python

  contact_schema = {"name": {"$type": "string"},
                    "$or": [{"email": {"$type": "string"}},
                            {"tel": {"$type": "string"}}

                           ]}
  db.command("collMod", "contacts", validator=contact_schema)

Op basis van deze validator kennen we de minimale structuur van de documenten in de collection ``contacts``.
Deze structuur kunnen we gebruiken voor het zoeken naar documenten in de collection.
Bovendien kunnen we dit gebruiken als basis voor een *index* waarmee we efficiënt kunnen zoeken.

**Opmerking.**
De collection kan documenten bevatten die niet aan dit schema voldoen,
als die eerder toegevoegd zijn.
Het is aan de database-programmeur om dit probleem op te lossen.
