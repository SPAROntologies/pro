## Competency Questions

PRO can be used for answering several questions related to the various roles of an agent in the publication process.
In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    PREFIX pro: <http://purl.org/spar/pro/>
    PREFIX tvc: <http://www.essepuntato.it/2012/04/tvc/>
    PREFIX ti: <http://www.ontologydesignpatterns.org/cp/owl/timeinterval.owl#>

### CQ1

Which roles in time and role types does a person hold?

    SELECT ?person ?personName ?roleInTime ?role
    WHERE {
        ?person a foaf:Person ;
            pro:holdsRoleInTime ?roleInTime .
        ?roleInTime a pro:RoleInTime ;
            pro:withRole ?role .
        OPTIONAL { ?person foaf:name ?personName . }
    }

### CQ2

Which documents are associated with a person's role in time?

    SELECT ?person ?role ?document
    WHERE {
        ?person a foaf:Person ;
            pro:holdsRoleInTime ?roleInTime .
        ?roleInTime pro:withRole ?role ;
            pro:relatesToDocument ?document .
    }

### CQ3

Which organization is a person affiliated with in relation to a specific document?

    SELECT ?person ?organization ?document
    WHERE {
        ?person pro:holdsRoleInTime ?roleInTime .
        ?roleInTime pro:relatesToOrganization ?organization ;
            pro:relatesToDocument ?document .
    }

### CQ4 

What is the time interval (start and end dates) of an affiliation or role held by a person?

    SELECT ?person ?role ?organization ?startDate ?endDate
    WHERE {
        ?person pro:holdsRoleInTime ?roleInTime .
        ?roleInTime pro:withRole ?role ;
            tvc:atTime ?timeInterval .
        OPTIONAL { ?roleInTime pro:relatesToOrganization ?organization . }
        OPTIONAL { ?timeInterval ti:hasIntervalStartDate ?startDate . }
        OPTIONAL { ?timeInterval ti:hasIntervalEndDate ?endDate . }
    }

### CQ5

What are the full affiliation details (role, target document, organization, and timeframe) for a person?

    SELECT ?person ?role ?document ?organization ?startDate ?endDate
    WHERE {
        ?person a foaf:Person ;
            pro:holdsRoleInTime ?roleInTime .
        ?roleInTime pro:withRole ?role .
        OPTIONAL { ?roleInTime pro:relatesToDocument ?document . }
        OPTIONAL { ?roleInTime pro:relatesToOrganization ?organization . }
        OPTIONAL {
            ?roleInTime tvc:atTime ?timeInterval .
            OPTIONAL { ?timeInterval ti:hasIntervalStartDate ?startDate . }
            OPTIONAL { ?timeInterval ti:hasIntervalEndDate ?endDate . }
        }
    }