## LEZIONE 1 ESERCIZI
![[Pasted image 20260311101813.png|400]]
>[!info]- soluzione
>
> ```scss
> INTERSECT(p1,p2)
> answer <- {}
> while p1 != NIL and p2 != NIL
> do if docID(p1)=docID(p2)
> 	p1<- next(p1)
> 	p2<- next(p2)
> 	else if docID(p1)<docID(p2)
> 			ADD(answer,docID(p1))
> 			p1<- next(p1)
> 		else p2<- next(p2)
>while p1 != NIL
> 	ADD(answer,docID(p1))
> return answer
> ```
> costi uguali
> per il 2 scorrerei tutti i docID e vedrei solo se non stanno in Caesar
> i costi cambiano


![[Pasted image 20260311101826.png|400]]
>[!info]- soluzione
>- la freq di countryman ci serve per determinare quanti elementi dobbiamo escludere dal totale
>	- prima controllo friends and roman e poi escludo countryman
>- in teoria no basta mettere una not con or per dover scorrere tutta la collezione

![[Pasted image 20260311103454.png|400]]
>[!info]- soluzione
>- farei l'unione dei due docID senza ripetizioni di kaleidoscope e eyes
>	- poi la metterei in merge con tangerine or trees formando una nuova lista risultante
>	- questa lista la metterei in and con kmarmelade or skies
>- farei prima kaleidoscope or eyes

![[Pasted image 20260311111004.png|400]]

>[!info]- soluzione
> ```scss
>  INTERSECT(p1,p2)
>  answer <- {}
>  while p1 != NIL and p2 != NIL
>  do if docID(p1)=docID(p2)
> 		 pos1 <- first(p1)
> 		 pos2 <- first(p2)
> 		 while pos1 != NIL and pos2 != NIL 
> 			do if pos1=pos2-1
> 					ADD(answer,docID(p1))
> 					break
> 				else if pos1< pos2 -1
> 						pos1 <- nextpos(pos1)
> 					else
> 						pos2 <- nextpos(pos2)
> 		 p1<- next(p1)
> 		 p2<- next(p2)
> 	else if docID(p1)< docID(p2)
> 			p1<- next(p1)
> 		 else
> 			 p2<-next(p2)
>  return answer
>  ```

## LEZIONE 3 ESERCIZI
![[Pasted image 20260321101333.png|400]]
slide 27
>[!info]- soluzione
> - esercizio 1
> Per $k=4$, i $400\,000$ termini vengono divisi in:
> - sapendo che contiene 400k termini
> $400\,000/4 = 100\,000 \text{ blocchi}$ ogni 4 parole
> Lo spazio totale si calcola sommando:
> - **term string**: $400\,000 \times 8 = 3.2 \ MB$
> - **document frequency**: $400\,000 \times 4 = 1.6 \ MB$
> - **posting pointer**: $400\,000 \times 4 = 1.6 \ MB$
> - **block pointer**: $100\,000 \times 3 = 0.3 \ MB$
> - **term length**: $400\,000 \times 1 = 0.4 \ MB$
> 	- li sommo e ottengo 3,2+1,6+0,3+0,4
> la formula
> $400,000⋅(8+4+4+1)+400,000​/k⋅3$
> 
> - esercizio 2
> la formula è $T_k​≈log_2​(\frac{M}{k}​)+\frac{k+1}{2}​$
> - M= numero termini
> - k= numero di blocchi
> - $\frac{k+1}{2}$ numero di confronti

