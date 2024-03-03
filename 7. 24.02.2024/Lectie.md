> Cine spune ca matematica nu are treaba cu informatica nu se numara printre progrmatorii adevarati
# [Elemente de Combinatorica](https://ro.wikipedia.org/wiki/Combinatoric%C4%83)

Combinatorica este latura matematicii care se ocupa de numararea unui numar de posibilitati.

## [Submultimi](https://ro.wikipedia.org/wiki/Submul%C8%9Bime)
* Fie multimea $M$. Multimea $S$ este submultime a multimii $M$ daca $S \subset M$.
* Fie $S_n$ numarul de submultimi ale unei multimi de $n$ elemente
* **Lema:** $S_n=2^n$
* **Demonstratie:** Fiecare element poate sa fie sau poate sa nu fie luat in submultime $=>$ fiecare element poate fi in $2$ stari $=>$ $S_n=2^n$.

## [Permutari](https://ro.wikipedia.org/wiki/Permutare)
* O **permutare** a unei multimi reprezinta o ordonare a multimii.
* Numarul de permutari ale unei multimi de $n$ elemente va fi notat cu $P_n \ $.
* **Lema: $P_n=n!$**
* **Demonstratie:** Ne vom tine un set de elmente nefolosite. La pasul $i$, vom scoate un element din set si il vom pune pe pozitia $i$. Ne oprim cand ramanem fara elemente in set. Observam ca numarul de **permutari** diferite va fi: $P_n=n(n-1)(n-2) \cdots 2 \cdot 1=n! \ $.
* **Observatie:** Pentru $n \ge 21, \ P_n \ge 2^{64}$ (deci $P_n$ nu va incapea pe long long).

## [Aranjamente](https://ro.wikipedia.org/wiki/Aranjament)
* Un **aranjament** al unei multimi reprezinta o submultime ordonata a acesteia
* Numarul de aranjamente ale unei multimi de $n$ elemente luate cate $k$ va fi notat cu $A_n^k \ $.
* **Lema: $A_n^k=\dfrac{n!}{(n-k)!}\ $**.
* **Demonstratie:** Ne vom tine un set de elmente nefolosite. La pasul $i$, vom scoate un element din set si il vom pune pe pozitia $i$. Ne oprim dupa pasul $k\ $, astfel incat ramanem cu $n-k$ elemente nefolosite in set. Observam ca numarul de **aranjamente** diferite va fi: $A_n^k=n(n-1)(n-2) \cdots (n-k+1)=\dfrac{n!}{(n-k)!} \ $.

## [Combinari](https://ro.wikipedia.org/wiki/Combinare)
* O **combinare** a unei multimi reprezinta o submultime neordonata a acesteia
* Numarul de combinari ale unei multimi de $n$ elemente luate cate $k$ va fi notata cu $C_n^k$ sau $\dbinom{n}{k}$.
* **Lema: $C_n^k=\dfrac{n!}{k!\ (n-k)!}\ $**.
* **Demonstratie:** $S_k$ o submultime de $k$ elemente a multimii initiale $S$. Exista $P_k=k!$ modalitati de a ordona aceasta submultime $=>$ fiecarei combinari ii corespund $P_k$ aranjamente $=>$ $C_n^k=\dfrac{A_n^k}{p_k}=\dfrac{n!}{k!\ (n-k)!}\ $.
* **Observatii:**

    1. $C_n^0=C_n^n=1$

    1. $\displaystyle\sum_{k=0}^n C_n^k=2^n$ (destul de intuitiv)
    1. $C_n^k=C_{n}^{n-k}$ (sirul combinarilor unei multimi de n elemente este simetric)
    1. $C_n^k=C_{n}^{k-1} \dfrac{n-k+1}{k}$
    1. $C_n^k=C_{n-1}^{k} \dfrac{n}{n-k}$
    1. $C_n^k=C_{n-1}^{k-1} \dfrac{n}{k}$
    1. $C_n^k=C_{n-1}^{k-1}+C_{n-1}^k$ (din acest fapt reiese triunghiul lui Pascal)

        ![Triunghiul lui Pascal](https://www.pbinfo.ro/resurse/9dc152/articole/tr-pascal/triunghi-pascal-4.png)
    
    1. In triunghiul lui Pascal numarul de pe linia $n$ si coloana $k$ (indexate de la 0) este $C_{n}^{k}\ $.
    1. Daca convertim numarul format din elementele de pe colana $n$ in baza $10$ acesta va fi $=11^n$

## [Permutari cu Repetitie](https://www.pbinfo.ro/articole/18941/elemente-de-combinatorica#intlink-3)
* Fie $n$ obiecte de $k$ tipuri diferite. Exista $T_i$ obiecte de tip $i => \displaystyle\sum_{i=1}^k T_i=n$
* Fie $P_R$ numarul de permutari diferite ale celor n obiecte (doua permutari sunt diferite daca pe macar o pozitie tipul obiectului este diferit).
* **Lema:** $P_R=\dfrac{n!}{T_1!\cdot T_2!\cdots T_k!}$.
* **Demonstratie:** Luam o permutare $P$. Fie $D_i$ dispunerea elementelor de tip $i$ in permutarea $P$. Daca fiecarui element de tip $i$ ii dam un numar de ordine $=>$ exista $T_i!$ permutari ale acestor elemente $=>$ exista $T_i!$ dispuneri $=D_i$ dar care au ordinea numerelor de ordine diferita $=>$ pentru fiecare permutare cu repetitie, pentru un tip $i$ corespund $T_i!$ dispuneri egale cu $D_i$ dar pentru care numarul de ordine difera $=>P_R=\dfrac{n!}{T_1!\cdot T_2!\cdots T_k!}$

## [Stars and Bars](https://www.pbinfo.ro/articole/25586/stars-and-bars)
* Fie $n$ obiecte si $k$ cutii.
* Fie $S_b$ numarul de modalitati de a plasa cele $n$ obiecte in cele $k$ cutii (doua modalitati sunt diferite daca pentru macar o cutie numarul de obiecte este diferit).
* **Lema:** $S_b=C_{n+k-1}^{k-1}$
* **Demonstratie:** O modalitate de plasare poate fi reprezentata grafic drept o insiruire de $n$ `⋆` (stele) si $k-1$ `|` (bare). Un obiect corespunde unei stele, iar o cutie este delimitata de $2$ bare consecutive, de inceputul sirului si prima bara sau ultima bara si sfarsitul sirului.

    De exemplu o configuratie de 6 obiecte si 4 cutii in care prima cutie contine un obiect, a doua contine $2$, a treia nu contine niciunul si a patra contine $3$ va fi reprezentata: `⋆|⋆⋆||⋆⋆⋆` 

    Problema se reduce la o permutare cu repetitie a $n+k-1$ obiecte, $n$ de un tip si $k-1$ de alt tip $=>S_b=\dfrac{(n+k-1)!}{n!\ (k-1)!}=C_{n+k-1}^{k-1}$

## [Numerele lui Catalan](https://ro.wikipedia.org/wiki/Num%C4%83r_Catalan)
* Fie o parantezare corecta o dispunere de `(` (paranteze deschise) si `)` (paranteze inchise) astefel incat fiecare `)` poate formea pereche cu o `(` din stanga lui neimperecheata cu alta `)`.
* Fie $C_n$ (numar Catalan) numarul de parantezari corecte cu $n$ paranteze deschise si $n$ paranteze inchise.
* **Lema:** $C_n=\displaystyle\sum_{i=0}^{n-1} C_i \ C_{n-i-1}$
* **Demonstratie:** Impartim o parantezare corecta in 2 parti: Prima parte va fi cel mai scurt subfix parantezat corect, iar cealalta parte restul. Vom itera cu un indice $i$ de la $0$ la $n-1$. Prima parte va fi imprejmuita de o pereche de paranteze, si deci numarul de posibilitati de parantezare corecta a acesteia este $C_i\ $. Ramanem cu $n-i-1$ perechi de paranteze pentru a doua parte, deci $C_{n-i-1}$ posibilitati $=>$ pentru un $i$ selectat avem formula $C_i \ C_{n-i-1} =>$ formula finala va fi  $C_n=\displaystyle\sum_{i=0}^{n-1} C_i \ C_{n-i-1}$
* **Lema:** $C_n=\dfrac{C_{2n}^n}{n+1}$
* **Demonstratie:** [Brilliant.org - Catalan](https://brilliant.org/wiki/catalan-numbers/). Pe scurt, exista $C_{2n}^n$ modalitati de parantezare nu neaparat corecte. Daca $U_n$ este numarul de modalitati de parantezare incorecte, atunci $C_n=C_{2n}^n-U_n$. Pentru a calcula $U_n$ vom analiza o secventa incorecta, $A$. Pentru aceasta secventa va exista un $k$ minim astfel incat parantezarea cu prefixul pana la indicele $k$ sa fie incorecta $=>$ parantezarea cu prefixul pana la indicele $k$ va contine $\dfrac{k-1}{2}$ paranteze deschise si $\dfrac{k+1}{2}$ paranteze deschise. Daca inversam parantezele acestui prefix si notam parantezarea totala rezultata cu $B$ atunci va exista o bijectie intre multimiea $A$-urilor si multimea $B$-urilor, si cum multimea $B$-urilor are cardinalul $C_{2n}^{n+1}=>U_n=C_{2n}^{n+1} => C_n=C_{2n}^n-U_n=C_{2n}^n-C_{2n}^{n+1}=\dfrac{C_{2n}^n}{n+1}\ $. 

## [Partitiile unei multimi](https://ro.wikipedia.org/wiki/Parti%C8%9Bie_(matematic%C4%83))
* O **partitie** a unei multimi este o multime de submultimi nevide ce nu au niciun element in comun si a caror reuniune este multimea initiala.
* Numarul de partitii de n elemente in k submultimi poarta numele de [numar al lui Stirling de speta a doua](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind), si se noteaza cu $S(n, k)$ sau $n\brace k$.
* **Relatie de recurenta:** $S(n, k)=k \cdot S(n-1, k)+S(n-1, k-1)$.
* **Demonstratie:** al $n$-ulea numar din partitie poate constitui o multime noua, caz care genereaza $S(n-1, k-1)$ posibilitati sau poate intra in alta multime, caz care genereaza $k \cdot S(n-1, k)$ posibilitati, deoarece avem $S(n-1, k)$ posibilitati de a imparti multimile si $k$ multimi in care putem introduce al $n$-ulea element.

* $\displaystyle\sum_{k=0}^n S(n, k)=B_n$, unde $B_n$ este numit [Numarul lui Bell](https://en.wikipedia.org/wiki/Bell_number)
* **Relatie de recurenta:** $B_n=\displaystyle\sum_{k=0}^n C_n^k \cdot B_k$
* **Demonstratie:** Daca eliminam primul element din partitie, atunci vom ramane cu $k$ elemente. Exista $C_n^k$ moduri de a alege numerele pentru aceste multimi si $B_k$ moduri de a partiona elementele.
* O tehnica ingenioasa de genera numerele lui Bell intr-un mod diferit poarta numele de [Triunghiul lui Bell](https://en.wikipedia.org/wiki/Bell_triangle).

# [Artitmetica Modulara](https://www.pbinfo.ro/articole/18944/aritmetica-modulara)
> Se cam invata in a 12-a $\dots$
* Operatia **modulo** reprezinta restul impartirii unui numar la alt numar si se noteaza cu ``%``.
* Exemple:
    * $5\ \% \ 2=1$
    * $6\ \% \ 3=0$
    * $4\ \% \ 5=4$
* **Definitie:** $2$ numere sunt congruente *modulo* $N$ daca ambele numere sunt egale *modulo* $N$ $(a \equiv b \bmod N \iff a \ \% \ N=b \ \% \ N)$ 
* $a \equiv b \bmod N \iff N\ |\ a-b$
* $a+b \equiv a\ \%\ N + b\ \%\ N \bmod N\iff (a+b)\ \%\ N = (a\ \%\ N + b\ \%\ N)\ \%\ N$
* $a-b \equiv a\ \%\ N - b\ \%\ N +N\bmod N\iff (a-b)\ \%\ N = (a\ \%\ N - b\ \%\ N + N)\ \%\ N\ $ (adaugam $N$, deoarece in urma scaderii putem avea rezultat negativ)
* $a \cdot b \equiv (a\ \%\ N) \cdot (b\ \%\ N) \bmod N\iff (a \cdot b)\ \%\ N = ((a\ \%\ N) \cdot (b\ \%\ N))\ \%\ N$
* **Atentie! :** $a\ /\ b \not \equiv (a\ \%\ N)\ /\ (b\ \%\ N) \bmod N$, adica $(a\ /\  b)\ \%\ N \not = ((a\ \%\ N)\ /\ (b\ \%\ N))\ \%\ N$

    Pentru a putea calcula impartirea a doua numere modulo alt numar va trebui sa ne folosim de un numar numit **invers modular**. In loc da a impartii $a$ la $b$, vom inmulti $a$ cu **inversul modular** al lui $b$.

## [Invers Modular](https://en.wikipedia.org/wiki/Modular_multiplicative_inverse)
* Inversul modular al unui numar, $a$ , este un alt numar, $x$ , astfel incat $a \cdot x \equiv 1 \bmod N$ (numarul cu care il putem inmulti pe $a$, astfel incat restul impartirii la $N$ sa fie $1$).
* Nu toate numerele au invers modular. In mod evident, daca cmmdc al $a$ si $N$ nu este $1$, atunci $a$ nu are invers modular modulo $N$.
### Calculul inversului modular
* Inversul modular al unui numar se poate calcula prin [Teorema lui Euler](https://en.wikipedia.org/wiki/Euler%27s_theorem).
    * Teorema lui Euler este urmatoarea: daca $\gcd(a, N)=1$, atunci $a^{\varphi(N)}$ este inversul modular al lui $a$ modulo $N$, unde $\varphi(N)$ este [coeficientul lui Euler al lui $N$](https://en.wikipedia.org/wiki/Euler%27s_totient_function), adica numarul de numere pozitive, mai mici cu $N$ si coprime cu acesta.
    * Pentru $N$ numar prim, $\varphi(N)=N-2$, deci inversul modular al lui $a$ va fi $a^{N-2}$.
* In concluzie, daca $n$ este prim, $a\ /\ b \equiv (a\ \%\ N) \cdot (b^{N-2}\ \% \ N) \bmod N \iff (a\ /\  b)\ \%\ N = ((a\ \%\ N) \cdot ((b\ \%\ N)^{N-2}\ \% \ N))\ \%\ N$

## Calculul informatic al elementelor combinatoriale
Mai jos aveti un program care calculeaza principalele elemente combinatoriale
```cpp
// Vom considera ca modulo-ul pentru care calculam
// elementele combinatoriale este 10^9+7, numar prim.
#include <iostream>

using namespace std;
typedef long long ll;
const ll MOD=1e9+7, Nmax=1e6;

ll f[Nmax+1];
ll fact(ll nr){ // ne folosim de memoizare, pentru a avea O(1) amortizat
    if (f[nr]==0){
        if (nr==0)
            f[nr]=1;
        else f[nr]=fact(nr-1)*nr%MOD;
    }
    return f[nr];
}
ll exp(ll b, ll e){ // b^e
    if (e==0)
        return 1;
    if (e%2==0)
        return exp(b*b%MOD, e/2);
    return b*exp(b, e-1)%MOD;
}
ll perm(ll n){ // numar de permutari cu n elemente
    return fact(n);
}
ll aran(ll n, ll k){ // aranjamente de n luate cate k
    return fact(n)*exp(fact(n-k), MOD-2)%MOD;
}
ll comb(ll n, ll k){ // combinari de n luate cate k
    return fact(n)*exp(fact(n-k)*fact(k)%MOD, MOD-2)%MOD;
}
ll catalan(ll n){ // al n-ulea numar catalan
    return fact(2*n)*exp(fact(n)*fact(n+1)%MOD, MOD-2)%MOD;
}
```
> Bafta la mate :))
