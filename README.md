Solr MLT
---------

``solr-mlt`` adds the ability to:

* boost MLT requests from ``MoreLikeThisHandler`` by arbitrary function queries. It relies on edismax, and  may not work with other ``QParser``s 

* sort MLT query results by using the ``sort`` parameter


Building
---------
You will need maven: ``mvn package`` . The resulting jar should be in your ``target/`` folder.

Configuration
--------------
In ``solrconfig.xml``, add the jar to your lib:
       <lib path="/path/to/solr-mlt-1.0-SNAPSHOT.jar" />
      
then configure your request handler:

       <requestHandler name="/mlt" class="org.dfdeshom.solr.mlt.MoreLikeThisHandler" />


Making requests:
----------------
you need to speciy  ``edismax`` as your query parser and you need to specify the default field (``df``) on which to apply this request, if it's not specified in your ``schema.xml``. Example request:

``curl http://localhost:8983/collection1/mlt?q={!boost%20b=recip(ms(NOW,pub_date),3.16e-11,1,1)%20defType=edismax}search term&mlt.fl=field&df=field``

To sort mlt results by a field, for example, ``pub_date``:
``curl http://localhost:8983/collection1/mlt?q=search term&mlt.fl=content&sort=pub_date desc``
