# Tricheck
## Triplets serializer and check filter to work with RDF data.  

```

def tricheck(s,p,o,g):
    
    
    if str(type(s)) == "rdflib.term.URIRef":
        pass
    else: 
        s_uri=re.match(r"http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+",s)
        if s_uri:
            s_uri = URIRef(str(s))
            s = s_uri
        else:
            sys.exit("Error:Subject {} must be a URI-compatible".format(str(s)))


    if str(type(p)) == "rdflib.term.URIRef":
        pass
    else: 
        p_uri=re.match(r"http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+",p)
        if p_uri:
            p_uri = URIRef(str(p))
            p = p_uri
        else:
            sys.exit("Error:Predicate {} must be a URI-compatible".format(str(p)))
    


    if str(type(o)) == "rdflib.term.URIRef":
        pass
    else: 
        o_uri=re.match(r"http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+",o)
        o_date=re.match(r"[\d]{1,2}-[\d]{1,2}-[\d]{2}",o)
        o_float=re.match(r"[-+]?\d*\.\d+|\d+",o)
        o_init=re.match(r"[+-]?[0-9]+$",o)

        if o_uri:
            o_uri = URIRef(str(o))
            o = o_uri

        elif o_date:
            o_date = Literal(str(o),datatype=XSD.date)
            o = o_date

        elif o_float:
            o_float = Literal(str(o),datatype=XSD.float)
            o = o_float

        elif o_init:
            o_init = Literal(str(o),datatype=XSD.init)
            o = o_init

        else:
            o = Literal(str(o),lang='en')

    g.add([s,p,o])
    
```
## Short Mock for trials:

```
import rdflib
import re
import sys

rdfs = Namespace("http://www.w3.org/2000/01/rdf-schema#")
trial = URIRef("https://github.com/pabloalarconm/tricheck")

g = Graph()

tricheck(trial,rdfs.label,"This is a label",g)
print(g.serialize(format='turtle'))

```
