﻿# 2 - Kompresovani niz

| Autor | Tekst i test primeri | Analiza rеšenja | Testiranje |
|:-:|:-:|:-:|:-:|
| Aleksa Milisavljević | Aleksa Milisavljević | Aleksa Milisavljević | Vladimir Milenković |

## Rešenje kada $N \leq 10$

U ovom podzadatku možemo testirati svaki mogući niz primene operacija. Za dato $K$ imamo $N-K+1 = (N - (K-1))$ opcija za primenu prve operacije. Posle prvog koraka, broj elemenata se smanji za $K-1$, pa u drugom koraku imamo $N-2\cdot(K-1)$ opcija. Ukupno, postoji $(N-(K-1)) \cdot (N-2\cdot(K-1)) \cdot ... \cdot ( (K-1) + (N\mod(K-1)))$ različitih sekvenci operacija koje rezultuju nizom dužine manje od $K$. Prethodni proizvod je ograničen sa $(N-1)!$, pa je složenost ovog rešenja $O(N!)$.

## Rešenje kada $K \leq 3$ i  $N \leq 100.000$

Za naredne podzadatke moramo malo detaljnije analizirati kako primene operacija utiču na niz. Za ovaj podzadatak, dovoljno je primetiti da su elementi krajnjeg niza zapravo kompresovani blokovi uzastopnih elemenata u početnom nizu. Ti blokovi uzastopnih elemenata imaju dužinu $1 \mod (K-1)$, što je lako pokazati. Naime, u početnom nizu blokovi imaju dužinu $1$. Svakom daljom kompresijom se $K$ takvih blokova spaja u jedan blok, koji po modulu $K-1$ ima dužinu $K \cdot 1 = K = 1 \mod (K-1)$. Konačno, za ovaj podzadatak možemo da analiziramo dva slučaja:

 * $K=2$ - tada krajnji niz ima dužinu $1$, a vrednost tog elementa je $\textbf{or}$ vrednosti svih elemenata u početnom nizu
 * $K=3$ - tada krajnji niz ima dužinu $1$ ukoliko je $N$ neparno i $2$, ukoliko je $N$ parno. Ukoliko je $N$ neparno, tada je vrednost jedinog elementa $\textbf{or}$ vrednosti svih elemenata u početnom nizu. Konačno, ukoliko je $N$ parno, možemo da iteriramo po završetku prvog bloka, koji se završava na nekom neparnom indeksu u početnom nizu i da posmatramo prefiksni i sufiksni $\textbf{or}$ elemenata početnog niza. Rešenje zadatka je minimum njihove sume.

## Rešenje kada $N \le 3.000$

Ovaj i naredne podzadatke rešavamo dinamičkim programiranjem. Vrednost $dp[i]$ će predstavljati minimalnu vrednost sume ukoliko se ograničimo isključivo na prvih $i$ elemenata niza. Prvo moramo primetiti da je uslov da se operacije primenjuju sve dok niz ima dovoljno elemenata suštinski nevažan. Naime, primenom operacije se suma elemenata niza ne može povećati, te je svakako uvek bolje primeniti što više operacija. Za svaku poziciju $i$, iteriramo po završnoj poziciji prethodnog bloka. Rekurentna formula za naše dinamičko programiranje je $\min_j (dp[j] + A[j+1] \textbf{ or } A[j+2] \textbf{ or } ... \textbf{ or } A[i])$, gde je $i - j = 1 \mod (K-1)$ (jer prema opservaciji iz prethodnog podzadatak važi da je dužina svakog bloka $1 \mod (K-1)$). Ukoliko održavamo vrednost $A[j+1] \textbf{ or } A[j+2] \textbf{ or } ... \textbf{ or } A[i]$, dok iteriramo po $j$, dobijamo rešenje složenosti $O(N^2)$.

## Rešenje kada su sve vrednosti u nizu $A$ su $1$ ili $2$ i $N \le 100.000$

I u ovom podzadatku ponovo posmatramo blokove. Svaki blok ima vrednost $1$, $2$ ili $3$. Međutim, blok koji se završava na nekoj poziciji $i$ može da ima samo dve vrednosti, a to su $A[i]$ i $3$ (npr. ukoliko $i$-ti element ima vrednost $1$, blok koji se završava na poziciji $i$ nikako ne može da ima vrednost $2$ i obrnuto). Za svaku krajnju poziciju, fiksiramo jednu od te dve moguće vrednosti poslednjeg bloka i posmatramo sve moguće startne pozicije trenutnog bloka. Za njih možemo da održavamo minimum koristeći neku strukturu podataka, na primer `set`.

## Rešenje kada $N \le 100.000$

Rešenje ovog podzadatka predstavlja nadogradnju na rešenje podzadatka u kojem su sve vrednosti u nizu $A$ jednake $1$ ili $2$. Možemo da primetimo da postoji najviše $\log_2 \max_i A[i] \leq 30$ mogućih različitih vrednosti bloka koji se završava na poziciji $i$ za svako $i$. Sada možemo da fiksiramo jednu od tih vrednosti i pronađemo najraniju poziciju (označimo tu poziciju sa $j$) na kojoj je trenutni blok mogao da počne da bi imao tu vrednost. Zatim, koristeći neku strukturu, npr. `set` pronađemo minimum vrednosti $dp$-ova prethodnog bloka od pozicije $j$ pa do $i$. Ovo možemo da realizujemo koristeći `set` na sledeći način. Možemo da čuvamo $K-1$ `set`-ova, jedan za svaku krajnju pozicija moduo $K-1$. U svakom od njih možemo da čuvamo vrednosti $dp$-ova sortirano po indeksu. Konačno kada ubacujemo $dp[i]$ u `set` koji odgovara modulu $i \mod (K-1)$, treba da izbacimo sve one koje koji se pojavljuju pre njega, a imaju veću vrednost od $dp[i]$. Kada tražimo minimum od pozicije $j$ do pozicije $i$, možemo da pozovemo ugrađenu funkciju `lower_bound`, da bi pronašli prvi indeks veći od $j$ koji postoji u odgovarajućem `set`-u. Složenost ovog rešenja je $O(N \log N \log \max_i A[i])$.

## Glavno rešenje
Za glavno rešenje i poslednji podzadatak, dovoljno je optimizovati složenost na $O(N \log \max_i A[i])$. Ovo se može postići na primer tako što zamenimo `set` sa spars tabelom. 
