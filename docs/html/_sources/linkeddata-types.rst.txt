********************
Types in Linked Data
********************

Eigenlijk moet je naast de namen van de eigenschappen ook de bijbehorende types beschrijven.
De Linked Data aanpak maakt ook dit mogelijk.
We kunnen dan ook verschillende soorten strings onderscheiden, bijvoorbeeld een string die een datum beschrijft,
of een XXX.

Met behulp van ``@type`` kun je aangeven wat het type is van het document (object).
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

Je kunt ook in de context aangeven wat de types zijn van de genoemde eigenschappen.
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
   "@type": "https://schema.org/Person"
   "name": "Hans de Jong",
   "tel": "06-1234 5599",
   "age": 35,
   "homepage": "https://hans.nl"
   }

**Opmerking:** in het algemeen is het niet verstandig om veranderlijke ("volatile") gegevens zoals  ``age`` op te slaan.
We gebruiken dit hier alleen als type-voorbeeld.
