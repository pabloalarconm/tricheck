# Tricheck
## Triplets serializer and check filter to work with RDF data.  

from rdflib import Graph,URIRef, Literal
import sys
import re


def triplipy(s,p,o,g):
    
    #Subject:
    s.strip()
    ss=s.split()
    m_http=re.match(r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+',s)
    #Check if its a rdflib object:
    if str(type(s))== "<class 'rdflib.term.URIRef'>":
        pass
    #Check if its a URI or a Multiple words string:
    elif len(ss) == 1 and m_http:
        s=URIRef(str(s))
    elif len(ss) >1 and m_http:
        sys.exit("Multiple word string cant be a Subject")
    else:
        sys.exit("Not matchable Subject")
        
    #Predicate:
    p.strip()
    pp=p.split()
    m_http=re.match('http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+',p)
    #Check if its a rdflib object:
    if str(type(p))== "<class 'rdflib.term.URIRef'>":
        pass
    #Check if its a URI or a Multiple words string:
    elif len(pp) == 1 and m_http:
        p=URIRef(str(p))
    elif len(pp) >1 and m_http:
        sys.exit("Multiple word string cant be a Predicate")
    else:
        sys.exit("Not matchable Predicate")
        
        
    #Object:
    m_http=re.match('http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\), ]|(?:%[0-9a-fA-F][0-9a-fA-F]))+',o)
    m_date=re.match(r"[\d]{1,2}-[\d]{1,2}-[\d]{2}",o)
    m_float=re.match(r"[-+]?\d*\.\d+|\d+",o)
    m_init=re.match(r"[+-]?[0-9]+$",o)
    oo=o.split()
    #Check if its a rdflib object:
    if str(type(o))=="<class 'rdflib.term.URIRef'>":
        pass
    #Check if its a URI or a Multiple words string:
    elif len(oo) == 1 and m_http:
        o= URIRef(str(o))
    elif len(oo) > 1 and m_http:
        o = Literal(str(o),lang='en')
    elif m_date:
        o = Literal(str(o),datatype=XSD.date)
    elif m_float:
        o = Literal(str(o),datatype=XSD.float)
    elif m_init:
        o = Literal(str(o),datatype=XSD.init)
    else:
        o = Literal(str(o),lang='en')
        
    g.add([s,p,o])


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
