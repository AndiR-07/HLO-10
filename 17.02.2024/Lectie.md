# Discutarea subiectului de la olimpiada pe scoala
> Un pas mai aproape de ONI
### [Matrix Masterclass](https://kilonova.ro/problems/2293)

### Cerinta 1:
Aceasta cerinta putea fi abordata un foarte multe modalitati (avand in considerare gradul scazut de dificultate a acesteia). Cateva modalitati sunt:

* Observam ca dupa fiecare parcurgere pe o directie, matricei initiale i se "taie" o latura. Ne putem folosi de aceasta observatie, actualizand dupa fiecare parcurgere coltul **stanga-sus** si **dreapta-jos**, astfel facandu-ne viata mai usoara.

    Exemplu de implementare:
    ```cpp
    int l1, l2, c1, c2, dir;
    l1=c1=1;
    l2=c2=n;
    dir=0; // 0 -> dreapta, 1 -> jos, 2 -> stanga, 3 -> sus
    while (l1<=l2 && c1<=c2){
        if (dir==0){ // dreapta
            for (int j=c1; j<=c2; j++)
                fout<<mat[l1][j]<<' ';
            l1++;
        }
        else if (dir==1){ // jos
            for (int i=l1; i<=l2; i++)
                fout<<mat[i][c2]<<' ';
            c2--;
        }
        else if (dir==2){ // stanga
            for (int j=c2; j>=c1; j--)
                fout<<mat[l2][j]<<' ';
            l2--;
        }
        else{ // sus
            for (int i=l2; i>=l1; i--)
                fout<<mat[i][c1]<<' ';
            c1++;
        }
        dir=(dir+1)%4;
    }
    ```
* Notam cu `-1` elementele vizitate, cu `0` cele nevizitate si ne deducem niste cazuri pentru fiecare directie (Est / Sud / Vest / Nord).

    Exemplu de implementare:
    ```cpp
    void bordare(){
        for(int i = 0; i <= n + 1; i++){
            mat[0][i] = -1;
            mat[n + 1][i] = -1;

            mat[i][0] = -1;
            mat[i][n + 1] = -1;
        }
    }

    void parcurgere_spirala(){
        int x, y, dir;

        bordare();

        x = y = 1;
        dir = 0;

        while(mat[x][y] != -1){
            fout << mat[x][y] << " ";
            mat[x][y] = -1;

            if(mat[x + dirX[dir]][y + dirY[dir]] == -1){
                dir = (dir + 1) % NoDir;
            }

            x += dirX[dir];
            y += dirY[dir];
        }
    }
    ```
### Cerinta 2:
Aceasta cerinta putea fi abordata folosind recursivitatea, functionalitate care va fi predata la acest curs. Din nou, exista mai multe abordari, insa ideea generala ramane aceeasi. Ne impartim cadranul curent in $4$ cadrane si le parcurgem in ordinea din enunt, oprindu-ne cand cadranul curent ramane cu un singur patratel.

Exemple de implamentare:
* folosindu-ne de colturile cadranului curent:
```cpp
void cadran (int l1, int c1, int l2, int c2){
    if (l1==l2 && c1==c2){
        fout<<mat[l1][c1]<<' ';
        return;
    }
    int lmij=(l1+l2)/2;
    int cmij=(c1+c2)/2;
    cadran(l1, c1, lmij, cmij); // stanga-sus
    cadran(l1, cmij+1, lmij, c2); // dreapta-sus
    cadran(lmij+1, cmij+1, l2, c2); // dreapta-jos
    cadran(lmij+1, c1, l2, cmij); // stanga-jos
}

...

cadran(1, 1, n, n);
```
* folosindu-ne de lungimea laturii cadranului curent:
```cpp
void parcurgere_recursiv(int x, int y, int ordin){
    if(ordin == 1){
        fout << mat[x][y] << " ";
        return;
    }

    ordin /= 2;
    parcurgere_recursiv(x, y, ordin);
    parcurgere_recursiv(x, ordin + y, ordin);
    parcurgere_recursiv(ordin + x, ordin + y, ordin);
    parcurgere_recursiv(ordin + x, y, ordin);
}

...

parcurgere_recursiv(1, 1, n);
```
### Cerinta 3:
Pentru aceasta cerinta limitele de timp erau indeajuns de mari incat sa permita verificarea primalitatii unui numar atat folosind ciurul lui Eratosthenes ce permite o complexitate totala de $O(N^2+Vmax \cdot log \ log \ Vmax)$, cat si folosind o verificare in $O(\sqrt {Vmax})$, complexitatea totala fiind $O(N^2 \cdot \sqrt {Vmax})$. Este totusi de preferat sa folositi ciurul, deoarece in conditiile actuale, complexitatea lui era mai mica.

Exemplu de implementare:
```cpp
ciur[0]=ciur[1]=1;
    for (int i=2; i*i<=Vmax; i++)
        if (ciur[i]==0)
            for (int j=i*i; j<=Vmax; j+=i)
                ciur[j]=1;

...

for (int i=1; i<=n; i++)
    for (int j=1; j<=n; j++){
        fin>>mat[i][j];
        if (!ciur[mat[i][j]]) // e prim
            nrp++;
    }

...

fout<<nrp<<'\n';
for (int i=1; i<=n; i++)
    for (int j=1; j<=n; j++)
        if (!ciur[mat[i][j]])
            fout<<i<<' '<<j<<'\n';
```
### Cerinta 4:
Pentru aceasta cerinta modalitatile de verificare a primalitatii unui numar raman aceleasi. Pentru a raspunde in $O(1)$ la intrebari va trebui sa ne formam o matrice de sume partiale, in care `sp[i][j]=numarul de numere prime de la (0, 0) pana la (i, j)`. Complexitatea folosind ciurul va fi  $O(N^2+Q+Vmax \cdot log \ log \ Vmax)$

Exemplu de implemantare:
```cpp
ciur[0]=ciur[1]=1;
    for (int i=2; i*i<=Vmax; i++)
        if (ciur[i]==0)
            for (int j=i*i; j<=Vmax; j+=i)
                ciur[j]=1;

...

// transformam matricea in matrice binara unde 0 -> nu e prim, 1 -> e prim
for (int i=1; i<=n; i++)
    for (int j=1; j<=n; j++)
        if (!ciur[mat[i][j]])
            mat[i][j]=1;
        else mat[i][j]=0;
// transformam in matrice de sume partiale
for (int i=1; i<=n; i++)
    for (int j=1; j<=n; j++)
        mat[i][j]+=mat[i-1][j]+mat[i][j-1]-mat[i-1][j-1];
fin>>q;
int l1, c1, l2, c2;
for (int i=0; i<q; i++){
    fin>>l1>>c1>>l2>>c2;
    fout<<mat[l2][c2]-mat[l1-1][c2]-mat[l2][c1-1]+mat[l1-1][c1-1]<<'\n';
}
```

<details><summary>Solutie completa Tanasescu</summary>

```cpp
#include <iostream>
#include <fstream>

using namespace std;
ifstream fin ("matrix.in");
ofstream fout ("matrix.out");
const int Nmax=1000, Vmax=1e6;

int c, n, q, mat[Nmax+1][Nmax+1], nrp;
bool ciur[Vmax+1];

void cadran (int l1, int c1, int l2, int c2){
    if (l1==l2 && c1==c2){
        fout<<mat[l1][c1]<<' ';
        return;
    }
    int lmij=(l1+l2)/2;
    int cmij=(c1+c2)/2;
    cadran(l1, c1, lmij, cmij); // stanga-sus
    cadran(l1, cmij+1, lmij, c2); // dreapta-sus
    cadran(lmij+1, cmij+1, l2, c2); // dreapta-jos
    cadran(lmij+1, c1, l2, cmij); // stanga-jos
}

int main(){
    // precalculare ciur pentru cerintele 3 & 4
    ciur[0]=ciur[1]=1;
    for (int i=2; i*i<=Vmax; i++)
        if (ciur[i]==0)
            for (int j=i*i; j<=Vmax; j+=i)
                ciur[j]=1;

    fin>>c;
    fin>>n;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=n; j++){
            fin>>mat[i][j];
            if (!ciur[mat[i][j]]) // e prim
                nrp++;
        }
    if (c==1){
        int l1, l2, c1, c2, dir;
        l1=c1=1;
        l2=c2=n;
        dir=0; // 0 -> dreapta, 1 -> jos, 2 -> stanga, 3 -> sus
        while (l1<=l2 && c1<=c2){
            if (dir==0){ // dreapta
                for (int j=c1; j<=c2; j++)
                    fout<<mat[l1][j]<<' ';
                l1++;
            }
            else if (dir==1){ // jos
                for (int i=l1; i<=l2; i++)
                    fout<<mat[i][c2]<<' ';
                c2--;
            }
            else if (dir==2){ // stanga
                for (int j=c2; j>=c1; j--)
                    fout<<mat[l2][j]<<' ';
                l2--;
            }
            else{ // sus
                for (int i=l2; i>=l1; i--)
                    fout<<mat[i][c1]<<' ';
                c1++;
            }
            dir=(dir+1)%4;
        }
    }
    else if (c==2){
        int l1, l2, c1, c2, dir;
        l1=c1=1;
        l2=c2=n;
        cadran(l1, c1, l2, c2);
    }
    else if (c==3){
        fout<<nrp<<'\n';
        for (int i=1; i<=n; i++)
            for (int j=1; j<=n; j++)
                if (!ciur[mat[i][j]])
                    fout<<i<<' '<<j<<'\n';
    }
    else{
        for (int i=1; i<=n; i++)
            for (int j=1; j<=n; j++){
				// transformam matricea in matrice binara unde 0 -> nu e prim, 1 -> e prim
                if (!ciur[mat[i][j]])
                    mat[i][j]=1;
                else mat[i][j]=0;
				// transformam in matrice de sume partiale
				mat[i][j]+=mat[i-1][j]+mat[i][j-1]-mat[i-1][j-1];
			}
        fin>>q;
        int l1, c1, l2, c2;
        for (int i=0; i<q; i++){
            fin>>l1>>c1>>l2>>c2;
            fout<<mat[l2][c2]-mat[l1-1][c2]-mat[l2][c1-1]+mat[l1-1][c1-1]<<'\n';
        }
    }

    return 0;
}
```
</details>
<details><summary>Solutie completa Dragulescu</summary>

```cpp
#include <iostream>
#include <fstream>

using namespace std;
ifstream fin ("matrix.in");
ofstream fout ("matrix.out");
const int Nmax=1000, Vmax=1e6;

int c, n, q, mat[Nmax+1][Nmax+1], nrp;
bool ciur[Vmax+1];

void cadran (int l1, int c1, int l2, int c2){
    if (l1==l2 && c1==c2){
        fout<<mat[l1][c1]<<' ';
        return;
    }
    int lmij=(l1+l2)/2;
    int cmij=(c1+c2)/2;
    cadran(l1, c1, lmij, cmij); // stanga-sus
    cadran(l1, cmij+1, lmij, c2); // dreapta-sus
    cadran(lmij+1, cmij+1, l2, c2); // dreapta-jos
    cadran(lmij+1, c1, l2, cmij); // stanga-jos
}

int main(){
    // precalculare ciur pentru cerintele 3 & 4
    ciur[0]=ciur[1]=1;
    for (int i=2; i*i<=Vmax; i++)
        if (ciur[i]==0)
            for (int j=i*i; j<=Vmax; j+=i)
                ciur[j]=1;

    fin>>c;
    fin>>n;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=n; j++){
            fin>>mat[i][j];
            if (!ciur[mat[i][j]]) // e prim
                nrp++;
        }
    if (c==1){
        int l1, l2, c1, c2, dir;
        l1=c1=1;
        l2=c2=n;
        dir=0; // 0 -> dreapta, 1 -> jos, 2 -> stanga, 3 -> sus
        while (l1<=l2 && c1<=c2){
            if (dir==0){ // dreapta
                for (int j=c1; j<=c2; j++)
                    fout<<mat[l1][j]<<' ';
                l1++;
            }
            else if (dir==1){ // jos
                for (int i=l1; i<=l2; i++)
                    fout<<mat[i][c2]<<' ';
                c2--;
            }
            else if (dir==2){ // stanga
                for (int j=c2; j>=c1; j--)
                    fout<<mat[l2][j]<<' ';
                l2--;
            }
            else{ // sus
                for (int i=l2; i>=l1; i--)
                    fout<<mat[i][c1]<<' ';
                c1++;
            }
            dir=(dir+1)%4;
        }
    }
    else if (c==2){
        int l1, l2, c1, c2, dir;
        l1=c1=1;
        l2=c2=n;
        cadran(l1, c1, l2, c2);
    }
    else if (c==3){
        fout<<nrp<<'\n';
        for (int i=1; i<=n; i++)
            for (int j=1; j<=n; j++)
                if (!ciur[mat[i][j]])
                    fout<<i<<' '<<j<<'\n';
    }
    else{
        for (int i=1; i<=n; i++)
            for (int j=1; j<=n; j++){
				// transformam matricea in matrice binara unde 0 -> nu e prim, 1 -> e prim
                if (!ciur[mat[i][j]])
                    mat[i][j]=1;
                else mat[i][j]=0;
				// transformam in matrice de sume partiale
				mat[i][j]+=mat[i-1][j]+mat[i][j-1]-mat[i-1][j-1];
			}
        fin>>q;
        int l1, c1, l2, c2;
        for (int i=0; i<q; i++){
            fin>>l1>>c1>>l2>>c2;
            fout<<mat[l2][c2]-mat[l1-1][c2]-mat[l2][c1-1]+mat[l1-1][c1-1]<<'\n';
        }
    }

    return 0;
}
```
</details>

# Functii in C++
* Functiile reprezinta subprograme care se pot apela primind un set de parametrii, returnand astefel un set de date sau modificand anumite variabile din program
* Functia din C++, asemanatoare cu cea din matematica este definita prin: **Nume**, **Parametrii**, **Instructiuni** si **Valoare returnata**. Parametrii sau valoarea returnata pot sa nu existe.
* Functia se defineste in cod in urmatorul fel:
    ```cpp
    TipDeDateReturnat NumeFunctie(Paramterii){
        Instruciuni
        return DateReturnate;
    }
    // tipul de date returnat va fi "void" daca functia nu returneaza nimic, iar campul de parametrii va fi lasat liber daca functia nu primeste niciun parametru
    ```
    Un exemplu de functie numita `sum` care primste ca parametrii $2$ valori si returneaza suma este:

    ```cpp
    int sum(int a, int b){
        int s=a+b;
        return s;
    }
    ```
* **! ATENTIE:** O data ce instructiunea `return` se executa, programul iese din functie, astfel incat orice eventuala instructiune dupa `return` nu se va mai executa,
* Functia se apeleaza in cod in urmatorul fel:
    ```cpp
    NumeFunctie(Parametrii);
    ```
    Un exemplu de apel al functiei `sum` este:

    ```cpp
    int a=3, b=5;
    int s=sum(a, b);
    cout<<s;
    // se va afisa 8
    ```
* In mod normal, functia va copia valoarea parametrilor dati la apel si va crea variabile noi, locale functiei cu acealeasi valori.

    **! IMPORTANT: O consecinta a acestui fapt este ca daca modificam valoarea unui parametru in functie, acesta nu va fi modificat si in progrmul principal**
* Pentru a combate acest lucru, parametrul va trebui transmis prin **referinta**, punandu-se un `&` intre tipul de date si numele variabilei.

    Exemplu:
    ```cpp
    void modif(int &nr){
        nr=3;
    }
    // cand vom apela modif, nr va fi modificat atat in functie cat si in programul din care a fost apelata functia
    ```
* Daca vrem sa avem ca parametru un vector, acesta va fi transmis automat ca referinta (deoarece copiera lui in intregime ar lua mai mult timp).

    Exemplu:
    ```cpp
    void Clear(int v[], int n){
        for (int i=0; i<n; i++)
            v[i]=0;
    }
    //schimbarea asupra vectorului v se va mentine si dupa apel
    ```
* Daca vrem ca vectorul sa fie copiat pentru ca eventualele schimbari sa nu se mentina si dupa apel, va trebui sa folosim un vector STL, dar apelul functiei va deveni $O(LungimeaVectorului)$.
# Recursivitate
> Chiar e folositoare

* Recursivitatea este tehnica prin care apelam o functie in ea insasi
* Pentru a evita autoapelul la infinit vom avea nevoie de o conditie clara de incheiere.
* Exemplu de functie recurenta care ne va da al $k$-ulea termen din sirul lui Fibonacci:
    ```cpp
    int fib(int k){
        if (k==0)
            return 0;
        if (k==1)
            return 1;
        return fib(k-1)+fib(k-2);
    }
    ```
* Observatie: deoarece functiile de baza vor returna 1 sau in cazuri speciale 0 $=>$ complexitatea acestei recurente va fi $O(fib(k))$, care creste exponential cu un factor care, coincidenta sau nu, este similar cu $\phi=1.618\cdots= \frac{\sqrt{5} - 1}{2} (phi)$.
* Observatie: functia noastra recursiva calculeaza de mai multe ori al $k_i$-ulea termen Fibbonaci, chiar daca acesta a fost calculat in trecut. Pentru a nu se intampla acest lucru si a tine cont de termenii deja calculati vom folosi o tehnica numita **memoizare**(nu este un typo, tehnica se numeste surprinzator memoizare si nu memorizare). Astfel, ne vom tine un vector suplimentar de valori deja calculate.
    ```cpp
    int v[Kmax]; 
    int fib(int k){
        if (k==0)
            return 0;
        if (k==1)
            return 1;
        if (v[k]!=0)
            return v[k];
        return v[k]=fib(k-1)+fib(k-2);
    }
    ```
    Complexitatea acestui algoritm scade la $O(k)$.

## Exponentierea Rapida
Intreabarea este urmatoarea: cum putem calcula rapid $n^k \ $?
* O functie naiva care ar face acest lucru ar arata ceva de genul:
    ```cpp
    int pow(int n, int k){
        int p=1;
        for (int i=0; i<k; i++)
            p*=n;
        return p;
    }
    ```
    $\cdots$ si ar avea complexitatea $O(k)$
* Ne putem folosi de observatia ca $`n^k = \begin{cases} 1 &\text{daca } k=0 \\ (n^2)^{k/2} &\text{daca } k\%2=0 \\ n \cdot n^{k-1} &\text{daca } k\%2=1 \end{cases}`$

	pentru a scrie urmatoarea functie:
    ```cpp
    int pow(int n, int k){
	if (k==0)
	    return 1;
	if (k%2==0)
	    return pow(n*n, k/2);
	return n*pow(n, k-1);
    }
    ```
    Deoarece o data la maxim 2 pasi exponentul se va imparti la 2, complexitatea este $O(log \ k)$. Acest algoritm poarta numele de **Exponentiere Rapida**.

## Evaluarea unei Expresii
> Foarte comuna pe la OJI-uri

In continuare vom studia problema [Infoarena - Evaluarea unei expresii](https://www.infoarena.ro/problema/evaluare). Pe scurt, se da o expresie aritmetica sub forma de sir de caractere formata din **operanzi**(numere), **paranteze** si **operatorii**: `+`, `-`, `*`, `/`(catul impartirii). Se cere sa se afiseze rezultatul. De exemplu expresia `(1+1)*13+10/2` are rezultatul `31`.

* Aceasta tip de problema poate fi abordata in 2 moduri:
    * prin forma poloneza (ceva mai avansata)
    * prin recursivitate indirecta (ceva mai simpluta)
* In lectia aceasta ne vom concentra pe rezolvarile ce folosesc recursivitatea indirecta:
    * ne vom defini $3$ functii:
        * functia `evaluare` se va ocupa de evaluarea unei expresii intregii si va lua in considerare termenii pe care ii va aduna/scadea intre ei (inmultirile, impartirile si rezultatele din paranteze vor fi deja calculate).
        * functia `termen` se va ocupa de evaluarea termenilor ce vor intra in expresie si va lua in considerare factorii care se vor inmulti/imparti intre ei (parantezele vor fi deja calculate iar adunarea si scaderea se vor evalua in functia `evaluare`).
        * functia `factor` se va ocupa de evaluarea factorilor ce vor intra in expresie. Daca avem paranteze, se va apela din nou functia `evaluare` pentru a evalua expresia din paranteze, returnandu-se rezultatul. In caz contrar, functia va returna numarul curent, deoarece adunarile/scaderile, inmultirile/impartirile si parantezele vor fi evaluate ulterior.
    * Ca si detaliu de implementare vom avea grija ca la sfarsitul expresiei sa adaugam un caracter nefolosit, de exemplu `$`, pentru a opri recursivitatea la infinit.
* Exemplul `(1+1)*13+10/2` se va evalua in urmatorul fel:

    evaluare(`(1+1)*13+10/2`){
    * termen(`(1+1)*13`){
        * factor(`(1+1)`){
            * evaluare(`1+1`){
                * termen(`1`){
                    * factor(`1`){
                        
                        return $1$

                        }

                    return $1$

                    }
                * +termen(`1`){
                    * factor(`1`){
                        
                        return $1$

                        }
                    
                    return $1$

                    }
                
                return $2$

                }

            return $2$

            }
        * *factor(`13`){
            
            return $13$

            }

        return $26$

        }
    * +termen(`10/2`){
        * factor(`10`){

            return $10$

            }
        * /factor(`2`){

            return $2$

            }

        return $5$

        }

    return $31$

    }

    Rapsuns: $31$
###
<details><summary>Solutie</summary>

```cpp
#include <iostream>
#include <fstream>

using namespace std;
typedef long long ll;
ifstream fin ("evaluare.in");
ofstream fout ("evaluare.out");

int pos;
string sir;
short s[256];
ll factor(), termen();
ll evaluare(){
    ll r=termen();
    while (s[sir[pos]]==1){
        pos++;
        if (sir[pos-1]=='+')
            r+=termen();
        else r-=termen();
    }
    return r;
}
ll termen(){
    ll r=factor();
    while (s[sir[pos]]==2){
        pos++;
        if (sir[pos-1]=='*')
            r*=factor();
        else r/=factor();
    }
    return r;
}
ll factor(){
    ll r=0;
    if (sir[pos]=='('){
        pos++;
        r=evaluare();
        pos++;
    }
    else while (s[sir[pos]]==3){
            r=r*10+sir[pos]-'0';
            pos++;
        }
    return r;
}
int main()
{
    s['+']=s['-']=1;
    s['*']=s['/']=2;
    for (int i='0'; i<='9'; i++)
        s[i]=3;

    fin>>sir;
    sir+='$';
    fout<<evaluare();

    return 0;
}
```
</details>

###

# Tema
* [PreONI 2004 - Bool](https://www.infoarena.ro/problema/bool)
* [OJI 2011 X - Expresie](https://kilonova.ro/problems/816?list_id=329)
* [OJI 2018 X - Eq4](https://kilonova.ro/problems/901/)
* [OJI 2006 X - Ecuatii](https://kilonova.ro/problems/397?list_id=359)
* [OJI 2009 X - Reteta](https://kilonova.ro/problems/792?list_id=341)
* [OJI 2012 X - Compresie](https://kilonova.ro/problems/827?list_id=323)
* [OJI 2020 X - Arh](https://kilonova.ro/problems/928?list_id=275)
* [OJI 2017 X - Caps](https://kilonova.ro/problems/887?list_id=293)
