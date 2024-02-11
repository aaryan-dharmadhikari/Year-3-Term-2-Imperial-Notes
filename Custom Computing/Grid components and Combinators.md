## Reminder:
![[Pasted image 20240124100526.png]]
- Geometric, directional interpretation in all circuits used
- This allows us to effectively express a 2D circuit in text
## Beside and below
![[Pasted image 20240124100909.png]]
- Beside: there is some `s` logically connecting Q and R, they can then be merged via `<->` combinator
- Similar for below: 
	```
	<<a, b>, c>  Q <|> R <d, <e, f>> 
					<=> 
	(<a,x> R <d,e>) & (<b,c> S <x,f>) for some x
	```
## Grid structures
![[Pasted image 20240124101653.png]]
- **Inverter** is a not; $<a, b>$ go in, $<\neg a, b>$ come out.
## Laws
![[Pasted image 20240124102540.png]]
- Similar to relationship between series and parallel `[(P ; Q ), (R ; S)] = [P, R] ; [Q, S].`
## Transposed conjugate
![[Pasted image 20240124103357.png]]
- The transpose conjugate combinator is a grid variant of the conjugate combinator. Note that:
	$R\backslash[P,Q]=[P,Q]^{-1} ;R;[P,Q],=[P^{-1} ,Q^{-1}];R;[P.Q].$
## Useful  definitions
![[Pasted image 20240124103650.png]]
## Recap: repeated parallel composition
![[Pasted image 20240124103826.png]]
## Row: repeated beside
![[Pasted image 20240124104329.png]]

![[Pasted image 20240124105510.png]]
- Top block is an insertion circuit; x is input, ys are current list
- Then the bottom is a (slow) insertion sort implementation (bubble sort)