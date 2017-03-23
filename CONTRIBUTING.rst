Bijdrage aan basisstatistiek
============================

Definition of Done
------------------

Algemeen
~~~~~~~~

-  Alle documentatie is waar mogelijk in ``plaintext`` opgeslagen in het `reStructuredText <http://docutils.sourceforge.net/docs/ref/rst/directives.html>`_ formaat.

Statistische producten
~~~~~~~~~~~~~~~~~~~~~~

Bij statistische producten moeten de volgende elementen aanwezig zijn

-  Definitie
-  Selectiecrtieria *Sommige producten leveren een geaggregeerd resultaat in verschillende groepen op. De beschrijving van deze groepen kan dan op zichzelf gezien worden als selectiecriteria.*
-  Logisch model (+ documentatie)
-  Bij meerdere mogelijke bronnen een beschrijving waarom deze bron is gebruikt (met cijfers onderbouwd)
-  Technische model + documentatie
-  Datatransformatie + schematische weergave van deze transformatie
-  Statische syntax met commentaar
-  Beschrijving eindproduct
-  Beschrijving beperkingen van het eindproduct (uitgesplitst naar beperkingen van de bron en van de huidige programmatuur)
-  Handleiding met stappen om het product te maken (+ aangehaakte ggevens)
-  Alles is tenminste één keer volledig gereviewed door een ander
-  Alles staat op gitlab

Code stijl
----------

Algemeen
~~~~~~~~

-  Nieuwe regels worden aangeduid met ``newlines`` en niet met ``carriage returns``.
-  Haal alle overbodige spaties en tabs aan het eind van elke regel weg.

SQL stijl
~~~~~~~~~

Algemeen
^^^^^^^^

-  Gebruik overal kleine letters.
-  Gebruik geen onnodige aanhalingstekens.

Inspringen
^^^^^^^^^^

**SQL statements**

.. code:: sql

    select
        persoon_id
    from
        persoon
    where
        woonplaats = 'Amsterdam'

Zet de SQL ``select``, ``from``, ``where``, ``having``, ``group by``, ``order by``, ``join`` allemaal onder elkaar voor het betreffende logische block.

**Joins**

.. code:: sql

    select
        a.persoon_id,
        b.naam as woonplaats
    from
        persoon as a
    inner join
        gemeente as b
    on
        a.gemeente_id = b.id

Joins worden op hetzelfde nivaeu gezet als de andere selectie blokken.

**Inline view**

.. code:: sql

    select
        persoon_id
    from
        (select
            a.persoon_id,
            b.naam as woonplaats
        from
            persoon as a
        inner join
            gemeente as b
        on
            a.gemeente_id = b.id
        ) as a
    where
        woonplaats = 'Amsterdam'

Zorg ervoor dat iedere \`inline view één stap naar rechts is ingesprongen ten opzichte van de bovenliggende query.

**Condities**

.. code:: sql

    select
        persoon_id
    from
       persoon
    where
        (
            woonplaats = 'Amsterdam'
        or
            (
                woonplaats = 'Diemen'
            and
                leeftijd > 18
            )
        )

Zorg er in je where statement voor dat het duidelijk is hoe de afhankelijkheden van je condities zijn. Kolommen die samen in een ``and`` of ``or`` conditie betrokken zijn dienen gegroepeerd te worden door haakjes en op hetzelfde niveau ingesprongen te zijn.

**Common Table Expressions**

.. code:: sql

    with persoon as (
        select
            persoon_id
        from
            persoon
    )
    select
        *
    from
       persoon
