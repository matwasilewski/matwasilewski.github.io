---
title : Uploading biomedical ontologies into neo4j database
notetype : feed
date : 26-04-2022
---
 
Biomedical ontologies have a natural graph-like structure and can be uploaded into a graph database easily. This tutorial explains this process, using SPARQL queries and neosemantics.

# Glossary
Subject, object, predicate: An RDF graph is a set of tiples or statements (subject,predicate,object) where both the subject and the predicate are resources and the object can be either another resource or a literal. The only particularity about literals is that they cannot be the subject of other statements. In a tree structure we would call them leaf nodes. Also keep in mind that resources are uniquely identified by URIs. - from https://jbarrasa.com/2016/06/07/importing-rdf-data-into-neo4j/

**Rule1:** Subjects of triples are mapped to nodes  in Neo4j. A node in Neo4j representing an RDF resource will be labeled `:Resource` and have a property `uri` with the resource’s URI.

```
(S,P,O) => (:Resource {uri:S})...
```

**Rule2a:** Predicates of triples are mapped to node properties in Neo4j **if the object of the triple is a literal**

```
(S,P,O) && isLiteral(O) => (:Resource {uri:S, P:O})
```

**Rule 2b:** Predicates of triples are mapped to relationships in Neo4j **if the object of the triple is a resource**

```
(S,P,O) && !isLiteral(O) => (:Resource {uri:S})-[:P]->(:Resource {uri:O})
```

**rule3:** The `rdf:type` statements are mapped to categories in Neo4j.

```
(Something ,rdf:type, Category)
```

The rule basically maps the way individual resources (data) are linked to classes (metadata) in RDF through the rdf:type predicate to the way you categorise nodes in Neo4j i.e. by using labels.





# Setup
First of all, initialize graph configuration:

```cypher
CALL n10s.graphconfig.init(); 
```

## Uniqueness constraints
Secondly, create a uniqueness contstraing on resource.
> Is r:Resource a necessary label?
> is the uniqueness constraint required?

```cypher
CREATE CONSTRAINT n10s_unique_uri ON (r:Resource) 
ASSERT r.uri IS UNIQUE;
```

## Configs

Due to the `applyNeo4jNaming` config option being set to `true`, Neosemantics is converting the relationship types to uppercase. In most cases this will be fine, but you may also prefer to create specific mappings for schema elements.

These graph model elements can be overridden by using the following configuration params:
-   **classLabel**: Label to be used for Ontology Classes (categories). Default is `Class`.
-   **subClassOfRel**: Relationship to be used for `rdfs:subClassOf` hierarchies. Default is `SCO`.
-   **dataTypePropertyLabel**: Label to be used for DataType properties in the Ontology. Default is `Property`.
-   **objectPropertyLabel**: Label to be used for Object properties in the Ontology. Default is `Relationship`.
-   **subPropertyOfRel**: Relationship to be used for `rdfs:subPropertyOf` hierarchies. Default is `SPO`.
-   **domainRel**: Relationship to be used for `rdfs:domain`. Default is `DOMAIN`.
-   **rangeRel**: Relationship to be used for `rdfs:range`. Default is `RANGE`.

# Quick introduction to RDFs
## Resource, Class and others
1. What is a resource?
2. What is a class?
3. Axioms?
4. Properties?
5. Argument?
6. What's with exotirc relation types such as has_synonym type
7. members, annotatedSource, head, inSubset...?

## Ontologies?
Relations between resources and classes in ontologies are also defined in other ontologies - Relationship Ontology (RO) and IO. Therefore, every relation is actually mapped to a PURL of ontology term. 

# Importing ontologies
## Different ways of importing ontologies
Neosemantics offers two ways of importing ontologies - a generic RDF import tool and one designed specifically for ontologies.

### Ontology tool
```cypher
CALL n10s.onto.import.fetch("https://github.com/neo4j-labs/neosemantics/raw/3.5/docs/rdf/vw.owl","Turtle");
```

However, it should be mentioned that this tool - albeit useful - allows the import of rudimentary elements of ontologies only. Therefore, to include XREFs and synonyms we will have to stick to the RDF one.

### SPARQL RDF tool
What should be done aftet the sparql query?
1. Map subclass_of onto relations that are relevant to the onto
2. Map any other RO relations that are relevant
3. Extract names and synonyms into separate nodes
	1. `hasExactSynonym` 
	2. `hasRelatedSynonym` 
	3. label vs synonym? what is the difference there
	4. `hasNarrowSynonym`
4. Create links with those nodes accordingly
5. Use `hasDbXref` to add new nodes from different ontologies
6. What to do with IAO terms? What on the good earth are those?
7. How does .owl SubclassOf relate to obo's is_a or part_of? How are those types of relations related to obo? How can I capture them in .owl?
8. Should I map Class / Resource labels on Terms in any way? Probably yes, but how exactly?
9. `exactMatch` is mapping onto MeSH
10. `consider` links to another term in HPO, eg HP:0001122 to HP:0000610
11. `hasDbXref` links to dBXref
13. `hasNarrowSynonym`
14. `hasExactSynonym`
15. `hasBroadSynonym`
16. `hasRelatedSynonym`
17. 
## Importing ontology
To access ontologies, we need their PURL (Permenanent URLs). There is a number of ontology serialization formats to choose from - Turtle, RDF/XML etc.
> what is the difference between .obo and .owl?
> What is the difference between RDF/XML and OWL?
> What is RDF and OWL?

Have a look at neosemantics documentation describing allowed ontology types.

Previewing ontology:

```cypher
CALL n10s.rdf.stream.fetch("http://purl.obolibrary.org/obo/hp.owl","RDF/XML");
```

> Add sample data here - what does it mean?

Importing ontology:
```cypher
CALL n10s.onto.import.fetch("http://purl.obolibrary.org/obo/hp.owl","RDF/XML")
```


