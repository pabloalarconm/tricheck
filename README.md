# Tricheck
## Triplets serializer and check filter to work with RDF data.  



## Short Mock for trials:

```
from rdflib import Graph,URIRef, Literal,Namespace
import sys
import re

rdfs = Namespace("http://www.w3.org/2000/01/rdf-schema#")
trial = URIRef("https://github.com/pabloalarconm/triplipy")

g = Graph()

triplipy(trial,rdfs.label,"This is a label",g)
g.serialize(format='turtle')

```
