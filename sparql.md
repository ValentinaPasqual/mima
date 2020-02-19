# How to run these queries 

```
filename = "toyset.ttl" #replace with the dataset 

import rdflib
import rdfextras
rdfextras.registerplugins() # so we can Graph.query()

g=rdflib.ConjunctiveGraph()
g.parse(filename, format="ttl")
results = g.query(""" Place here your query """)

for row in results:
    print (row)
```


# Queries 

## Query 1 

NL: "Which are all the fragments (both visual and textual) present in the dataset?"

```SELECT DISTINCT ?fragment
WHERE {
    ?manuscript a crm:E22_Man-Made_Object ;
        crm:P46_is_composed_of ?page . 
    ?page ?property ?recto_or_verso .
    ?recto_or_verso ?property2 ?fragment 
} 
```

Results: 

```hf:Carta_HF_I_12v_13r
hf.Carta_HF_I_44v_45r
hf:HF_I_incipit_title-frag
hf:HF_I_incipit_illustration
hf:Carta_HF_IV_20v_21r
hf:Illustrazione_X
```

## Query 2 

NL: "Quali sono i frammenti visuali che sono stati realizzati su più di un folio?"

``` SELECT DISTINCT ?illustration
WHERE {
    ?illustration a vir:IC1_Iconographic_Atom . 
    ?recto crm:P56_bears_feature ?illustration . 
    ?verso crm:P56_bears_feature ?illustration .    
}
```

Results: 

```hf:Carta_HF_I_12v_13r
hf:Carta_HF_IV_20v_21r
hf:Carta_HF_I_44v_45r
```

## Level 1

## Query 3 

NL: "Quali sono tutte le influenze (fisiche e concettuali) dell'illustrazione X appartenente alle HF?"
NB: mostra tutte le illustrazioni con tutte le influenze

```SELECT ?icon_atom ?influence ?source
WHERE { 
    
    ?manuscript crm:P46_is_composed_of ?icon_atom ; 
        dcterms:title ?HF . 
    ?icon_atom a vir:IC1_Iconographic_Atom .
    
    {  
    ?icon_atom vir:K1_denotes ?representation . 
    ?representation a vir:IC9_Representation .
    ?creation crm:P94_has_created ?representation . 
    ?creation crm:P15_was_influenced_by  ?influence . }
    
    UNION 
    
    { ?icon_atom a oaentry:CopyorDerivation ; 
        oaentry:isConceivedByMeansOf ?source . }
        
FILTER regex(?HF, "historiae ferrariae", "i")

}
```

## Query 4 

NL: "tutti gli eventi di produzione in cui PP è coinvolto"

``` SELECT ?event ?artefact 
WHERE {
    ?prisciani a crm:E21_Person. 
    ?event crm:P94_has_created | crm:P108_has_produced ?artefact .  
    ?event crm:P14_carried_out_by ?prisciani . 
    ?prisciani rdfs:label ?string .  
FILTER regex(?string, "pellegrino prisciani", "i")
}
``` 

## Query 5 (cs5)

NL: "Quali sono i temi rappresentati nelle illustrazioni delle HF o degli attributi delle illustrazioni?"

``` 
SELECT DISTINCT ?theme 
WHERE { 
    
    
    ?manuscript crm:P46_is_composed_of ?icon_atom ;
        dcterms:title ?HF . 
    ?icon_atom a vir:IC1_Iconographical_Atom ;
        vir:K1_denotes ?repr .
    
    {?repr vir:K17_has_attribute ?attr . 
    ?attr vir:K14_symbolize ?theme}
    
    UNION 
    
    {?repr crm:P138_represents ?theme . 
    ?theme a crm:E90_Symbolic_Object }
    
FILTER regex(?HF, "historiae ferrariae", "i")
}

"""
``` 

## Query 6 

NL: "Quali illustrazioni hanno il valore simbolico X?"

## Query 7 

NL: "Tutti i temi e simboli trattati nelle hf (che sono in comune con un'altra opera??)"
