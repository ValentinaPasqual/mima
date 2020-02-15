#Query 1 

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
```
```hf.Carta_HF_I_44v_45r
```
```hf:HF_I_incipit_title-frag
```
```hf:HF_I_incipit_illustration
```
```hf:Carta_HF_IV_20v_21r
```
```hf:Illustrazione_X
```
