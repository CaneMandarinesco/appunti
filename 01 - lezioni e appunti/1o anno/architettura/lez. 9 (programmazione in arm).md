# algoritmi
un algoritmo e' un procedimento astratto che puo' essere tradotto in un programma.  piu l'algoritmo e' astratto e piu e' ampia la scelta di linguaggi da utilizzare per la traduzione.  
e' possibile imporre delle condizioni al nostro algoritmo.  
gli algoritmi possono essere progettati usando i diagrammi di flusso che sono poco efficienti (se l'algoritmo e' grande puo' essere difficile leggere il diagramma), quindi noi usiamo i DNS.

### Diagrammi Nassi-Shneidermann (DNS)

sono disponibili i seguenti blocchi di selezione:
* singola (`if-then/if-else`)
* binaria (`if-then-else`)
* multipla (`switch`)

i cicli di iterazione:
* for
* while
* do-while

posso semplificare i blocchi di selezione introducendo delle variabili logiche, e la combinazione dei valori di queste determina se l'espressione e' vera o falsa

![[bubble-sort.png]]

dove della funzione $F$, posso creare una tavola della verita che mi descrive quando vale 0 e quando vale 1, ed eventualmente con la mappa di karnaugh, ricavare la sua formula.

# scriviamo codice arm diocane
## selezione singola

### es 1
![[1.png]]
```arm
        CMP R0,R1   ; aggiorna CPSR facendo R0-R1
        BLE else    ; Branch to else
                    ; LE: signed less than or equal
                    ; vai a else se R0<=R1 altrimenti continua
    
then    ...         ; BLOCCO THEN se R0>R1
        ...

else NOP
```

### es 2
![[binaria.png]]
```arm
        CPM R0, R1      ; aggiorna i flag facendo R0-R1
        BLE else        ; se R0 <= R1 vai ad else altrimenti vai avanti

        CMP R0, R2      ; aggiorna i flag facendo RO-R2
        BLE else        ; se R0 <= R2 vai ad else

        B then          ; vai a then! caso: R0>R1 AND R0>R2

else    ...
        ...
        
then    NOP

```

## selezione binaria
i due diagrammi di seguito sono equivalenti, implemento il secondo che e' piu semplice
![[Pasted image 20240417220359.png]]
![[Pasted image 20240417220521.png]]
```asm
        CMP     R0, R1      ; fai R0-R1
        BGT     then        ; vai a then se R0>R1
        CMP     RO, R2      ; fai R0-R2
        BGT     then        ; vai a then se RO>R2
else    ...
        ...
        B end               ; altrimenti then verrebbe eseguito
then    ...
        
        ...
end     NOP
```

## selezione multipla
![[Pasted image 20240417221105.png]]

```arm
        CMP     R0, R1
        BEQ     blocco1     ; R0=R1
        CMP     R0, R2      
        BEQ     blocco2     ; R0=R2
default ...
        ...
        ...
        B end
blocco1 ...
        ...
        B end
blocco2 ...
        ...
        B end
end     NOP
```

## for
![[Pasted image 20240417221420.png]]

```arm
        MOV     R0, #0
for     CMP     RO, R1
        BGE     end     ; se R0 >= R1 vai alla fine
        ADD     R0, R0, #0x1
blocco  ...             ; se R0 < R1
        ...
        B for           ; torna a for
end     NOP
            
```

## while
![[Pasted image 20240417221915.png]]
```arm
while   CMP R0, R1
        BGE end     ; R0 >= R1
blocco  ...
        ...
        B while
end     NOP
```

### do while
![[scemo chi legge]]

```arm
do      ...
blocco  ...
        CMP R0, R1
        BGE end
        B do
    
end NOP
```

# esercizi guidati
## es 1: divisione intera
> calcolare il quoziente della divisione tra X,Y, basta vedere quante volte Y sta in X

```arm
X EQU #0x0F ; dico al compilatore che X=0x0F
Y EQU #0x05 ; Y = 0X05

        MOV     R0, #0x0F   ; RO=X
        MOV     R1, #0x05   ; R1=Y
        MOV     R2, #0      ; R2=0
while   CMP     R0, R1      ; RO-R1
        BLT     END         ; R0 < R1? se si
					        ; vuol dire che ho trovato il quoz.
        SUB     R0, R0, R1  ; X = X-Y
        ADD     R2, R2, #1  ; R2 += 1
        B       while       ; vai a while
end     NOP                 ; l'algoritmo ha terminato, il 
                            ; risultato e' dentro R2
```