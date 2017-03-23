Publicatie statistische datasets
================================

Voor het publiceren van statistische datasets volgen we een vast stappenplan om te garanderen dat we dezelfde statistiek in de toekomst kunnen reproduceren. Daarvoor dienen we aandacht te besteden aan versiebeheer van de bronnen, code (syntax, documentatie, schema’s) en de gevulde database. 

Randvoorwaarden
---------------

Voor het doorlopen van dit document wordt uitgegaan van een aantal randvoorwaarden:

- Het is duidelijk welke statistiek(en) er gepubliceerd worden. Als het gaat over jaarboektabellen over de loop van de bevolking over een kalenderjaar, dan dient deze procedure gevolgd te worden voor geboorte, sterfte, vestiging, verhuizing en vertrek.
- Van elke relevante repository is vastgesteld welke versie van de code gebruikt gaat worden.

Logbestand
----------

Bij het statistiek product plaatsen we een log bestand met dezelfde naam als de bestandsnaam van het statistische product maar met de extensie rst. Dit logbestand wordt opgemaakt in ResturcturedText zodat we deze gegevens ook online kunnen publiceren, en in een later stadium kunnen indexeren.

Voorbeeld

- stifpsn18k1.sas7bdat
- stifpsn18k1.rst

In het log bestand wordt in platte tekst meta-informatie gezet over de totstandkoming van het specifieke product. Aan het eind van dit document staat hier een voorbeeld van.

Code bevriezen
--------------

Als er voor de publicatie code uit de development branch wordt gebruikt, dan wordt deze code op gitlab overgezet van de development branch naar de staging branch. Zo kan er op de development branch doorontwikkeld worden. De code die we gaan gebruiken voor een publicatie is onder voorbehoud bevroren in staging. En afhankelijk van dat voorbehoud is de master branch schoon gehouden. Houd er hierbij rekening mee dat dit niet alleen gaat over de repositories die direct verband houden met een specifiek product, maar bijvoorbeeld ook over die van de documentatie van definities. Deze kan in de loop van de tijd ook veranderen. Als we geen nieuwe ontwikkelde code willen gebruiken, dan gebruiken we voor onze publicatie de laatste versie die in master staat.

Bronnen
-------

Nadat helder is met welke code we gaan werken beginnen we met het verzamelen van de bronnen. Uiteraard alleen als de bronnen nog niet eerder zijn verzameld. De verzamelde bronnen slaan we op in een beveiligde map onder bronnen via de volgende structuur:

- bronnen
   - [organisatie] (e.g. OIS / BI)
      - [type bron] (e.g. bag / brp / brk)
         - [historie of stand]
            - [(begindatum –) einddatum]

In het log bestand wordt een verwijzing opgenomen naar de gebruikte bronnen op basis van het type en de datums. Als het een stand betreft dan hoeft de laatste map maar één datum te bevatten. Als het gaat om een mutatiebestand, dan heeft de laatste map de begindatum en einddatum waarover de mutaties zijn geleverd. Als de volledige historie is geleverd dan is de begindatum: 0

Database
--------

De volgende stap is het vanaf de basis opbouwen van het database model op de productieomgeving aan de hand van de bevroren code in de staging repository. Er dient een database aangemaakt te worden met een logische naam die past bij publicatie. De gegevens die in de database komen te staan moeten een logische samenhang met elkaar hebben. Het is dus niet zinvol om voor geboorte, sterfte, vestiging, verhuizing en vertrek telkens een aparte database te vullen. Als het bijvoorbeeld gaat over de loop van de bevolking 2018 dan zou deze naam van de database loopbevolking2018k1 kunnen zijn. Zo is duidelijk dat het de data huisvest voor de loop van de bevolking in de periode van het eerste kwartaal van 2018. Wanneer er bijvoorbeeld ook vastgoed statistieken in een database zijn opgenomen is een generieke naam zoals basisstatistiek2018k1 wellicht beter.
In het log bestand wordt gedocumenteerd welke database is gebruikt voor het publiceren van de dataset(s).

Handleidingen volgen
--------------------

Elk statistisch product bevat een handleiding die gevolgd kan worden om tot een publicatie te komen. In de handleidingen staan veelal de stappen beschreven om tot een specifiek eindproduct te komen. Het is echter handig om bepaalde bronnen zo volledig mogelijk te importeren, met het oog op mogelijk uitgebreider gebruik in de toekomst.n.

Validatie van de datasets
-------------------------

Op een gegeven moment zijn alle handleidingen gevolgd en de dataset(s) gemaakt. Deze datasets dienen dan nog wel gecontroleerd te worden door het team basisstatistiek als geheel. Afhankelijk van het oordeel van het team volgen hierop twee verschillende acties:

- Afgekeurd
   #. Er dienen in de development wijzigingen aangebracht te worden om het product op orde te krijgen.
   #. Alle wijzigingen worden weer verplaatst naar staging om opnieuw te beginnen met ``logbestand``.
   #. Dit proces wordt herhaald tot dat in de validatie van de dataset(s) goedkeuring wordt gegeven.

- Goedgekeurd
   #. Alle stappen vanaf hoofdstuk 2 worden herhaald door een tweede persoon. Daarbij wordt gebruik gemaakt van het bijgehouden logbestand en alles wat in staging staat.
   #. Vervolgens zijn er weer twee uitkomsten mogelijk. De dataset(s) zijn:
       #. Volledig reproduceerbaar: Er kan verder gegaan worden naar het volgende hoofdstuk.
       #. Niet volledig reproduceerbaar. Afhankelijk van de aard van de verschillen is het noteren van de verschillen in het log bestand voldoende, anders dienen dezelfde stappen gevolgd te worden als wanneer het eindproduct zou zijn afgekeurd.

Code publiceren
-------------------------------------------

Als alle dataset(s) zijn goedgekeurd en reproduceerbaar zijn gebleken, is de nieuwe publicatie wat betreft het eindproduct al gerealiseerd. De bijbehorende code moet echter nog wel geformaliseerd worden. Dit doen we door master bij te werken met de code uit staging. Vervolgens taggen we in master de laatste commit met een label die overeenkomt met de database naam.

Database formaliseren
---------------------

Zodra de dataset(s) zijn goedgekeurd wordt het logbestand dat is aangemaakt bij de dataset(s) overgenomen in de daarvoor bestemde database tabel zodat ook in de database zelf bekend is hoe hij tot stand is gekomen [#]_. Op de productieserver mag vanaf nu niks meer aan de database gewijzigd worden.

.. [#] Deze tabel is nog niet ontwikkeld.

Vullen van longitudinale database
---------------------------------

Voor het analyseren van tijdreeksen houden we een specifieke longitudinale database bij. Deze database bevat niet dezelfde data als de databases die worden gebruikt voor de dataset(s), maar bevat alleen de database versies van de dataset(s).
In de longitudinale database dient een nieuw schema aangemaakt te worden met dezelfde naam als de database waar vanuit de dataset(s) zijn overgenomen. Alle dataset(s) (vaak in de vorm van een materialized view) dienen als normale tabellen geïmporteerd te worden in het betreffende schema.

Publicatie op github
--------------------

Zodra alle publicaties zijn afgerond en alle code is doorgezet naar master (wanneer nodig), dan dienen de master branches van de gekoppelde github repositories ook bijgewerkt te worden.

Format logbestand
-----------------

.. code::

   publicaties
   -----------
   jaarboek 2018
   kerncijfers 2018

   gemaakt op
   ----------
   01-05-2018

   bronnen
   -------
   BAG DIVA exports:
   - Pad: BI / BAG / historie
   - Begindatum: 0
   - Einddatum:  01-02-2018
   Stand personen:
   - Pad: OIS / personen / stand
   - Begindatum: 01-01-2017
   - Einddatum:  01-01-2018
   StUFCSV:
   - OIS / personen / stand
   - Begindatum: 01-02-2017
   - Eindatum: 01-02-2018
   BAG UVA2:
   - Pad: BI / BAG / historie
   - Begindatum: 0
   - Einddatum: 12-31-2016
   Vestiging Extra:
   - Pad: BI / BRP / historie
   - Begindatum: 01-01-2017
   - Einddatum: 31-12-2017

   database
   --------
   loopvandebevolking2018k1

   tag
   ---
   loopvandebevolking2018k1

   parameters
   ----------
   Kern: geboorte('2017-02-01', '2018-02-01', '2016-01-01', '2017-01-01');
   Aangehaakt: geboorte('2017-01-01', '2018-02-01', '2017-01-01', '2018-01-01');

   gemaakt door
   ------------
   Aafke Elbrecht
   Maurice Hendriks

   gevalideerd door
   ----------------
   Hans de Waal

   datasets
   --------
   geboorte2018k1.sas7bdat
   sterfte2018k1.sas7bdat
   vestiging2018k1.sas7bdat
   vertrek2018k1.sas7bdat
   verhuizing2018k1.sas7bdat
   stif2018k1.sas7bdat
   woningvoorraad2018k1.sas7bdat
   transacties2018k1.sas7bdat

   Commentaar
   ---------
   Hier komt commentaar te staan toegevoegd door de maker(s) of reviewer(s)

*De eerste persoon onder de kop 'door' is de persoon die de eerste uitdraai heeft gedaan. De tweede persoon is de degene die gekeken heeft naar de reproduceerbaarheid van de producten*
