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

NL: "Quali sono tutte le influenze (fisiche e concettuali) dell'illustrazione X?"

# Query 4 

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
