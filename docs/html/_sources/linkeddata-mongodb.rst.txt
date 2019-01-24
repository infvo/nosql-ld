**********************
Linked Data en MongoDB
**********************

.. admonition:: Nog uit te werken

  NB: het onderstaande moet nog verder uitgewerkt worden.

.. todo::

  Hoe gebruik je linked data in combinatie met MongoDB?

* context hoeft niet embedded te zijn: door reference *sharing* van gemeenschappelijke context
* NB: context beschrijft niet welke velden aanwezig (moeten) zijn; deze beschrijft alleen de interpretatie van de velden.
* dit is een verschil met een schema:; dat beschrijft welke velden aanwezig moeten zijn (en geeft niet de interpretatie).
* typering vindt zowel in context als in schema plaats.
    * bij typering in BSON/MongoDB gaat het vooral om de manier waarop de data opgeslagen worden (en opgezocht);
    * bij typering in Linked Data (JSON-LD) gaat het vooral over de interpretatie (meestal van strings).
* gebruik van ``@id`` ook voor MongoDB-ids? - zie https://www.slideshare.net/gkellogg1/jsonld-and-mongodb
    * ``@id`` is dan de MongoDB-key; dit gebruik je ook voor het opzoeken van documenten in MongoDB.
    * vaak: gebruik van aliases voor ``@id`` en ``@type``: vereenvoudigt het gebruik in JavaScript/web frameworks.
      (een context beschrijft een reeks aliases)
    * voor een key in MongoDB wordt ``_id`` gebruikt; is dit iets anders dan ``id`` in de genoemde presentatie?
* vraag: bevat Schema.org (als context) aliases voor id en type?
    * ja, zie: https://schema.org/docs/jsonldcontext.json
    * (deze json krijg je ook via context negotiation van https://schema.org)

Redefinition of terms in local context?

.. code-block:: JSON

  {"@context": ["http://schema.org", {
      "image": { "@id": "schema:image", "@type": "@id"}
    }],
    
  }
