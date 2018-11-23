********************
MongoDB factorisatie
********************

.. todo::

  * factorisatie/normalisatie MongoDB uitwerken

* keuze: embedden vs. referenties; wanneer kies je waarvoor?
* belangrijke overweging: snelheid van toegang (embedding) vs. eenvoud van update (referenties)
* factorisatie van gemeenschappelijke data, door gebruik van *referenties* in plaats van *embedding*.
    * je brengt de gemeenschappelijke data "buiten haakjes")
* (vgl. normalisatie van een relationele database)
* doel: consistentie in de *data* van de database
