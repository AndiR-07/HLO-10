# Greedy vs DP vs Backtracking
>Famous last words: Asta nu e greedy?
## Greedy
* Greedy este o modalitate de abordare a unei probleme ce se bazeaza pe intuitie si pe gasirea unei reguli / proprietati generale, de multe ori bazate pe o sortare facuta in prealabil.
* Exista 3 nivele de gandire atunci cand analizam in minte un algoritm greedy
    1. Am reusit sa demonstrez matematic corectitudinea algoritmului greedy.
    2. Nu am facut o demonstratie riguroasa, insa daca ar trebui, as cam sti de unde sa incep si unde sa termin.
    3. Habar n-am daca algoritmul meu e bun, insa pare ca ar fi.
* In cazul in care nu suntem la niciun concurs, ar fi indicat sa scriem o mini-demonstratie de corectitudine a algoritmului nostru, pentru a ne antrena mintea (cazul 1)
* In cazul in care suntem la un concurs nivelul 2 este indeajuns de *safe* pentru a incepe implementarea. Daca totusi ne aflam la niveul 3, avem de ales intre a risca implementarea unui algoritm ce se poate dovedi a fi gresit, lucru ce va lua timp si a incerca a gasi un motiv pentru care algoritmul nostru merge sau un contraexemplu pentru care nu merge (lucru ce te va salva de la pierdere de timp inutila).
### **Probleme discutate**:
#### [CSES - Movie Festival](https://cses.fi/problemset/task/1629) (problema spectacolelor)
* Rezolvarea problemei se bazeaza pe observatia ca ne dorim sa alegem intotdeauna filme care se terimna cat mai devreme, pentru a lasa cat mai mult timp filmelor urmatoare.
* O demonstratie s-ar baza pe faptul ca daca nu am alege filmul care se termina cel mai devreme, atunci am putea rata un film care ar incepe inainte de ora filmului pe care l-am ales si care ar s-ar fi terminat rapid, inainte sa inceapa orice alt film, ca atare intotdeauna trebuie ales filmul ce se termina cat mai devreme.
<details><summary>Cod Tanasescu</summary>

```cpp
#include <iostream>
#include <algorithm>
 
#define fi first
#define se second
using namespace std;
const int Nmax=2e5+5;
typedef pair<int, int> pii;
 
int n, mx, nr, crt, sol;
pii v[Nmax];
 
bool cmp(pii a, pii b){
    return a.se<b.se;
}
 
int main(){
    cin>>n;
    for (int i=0; i<n; i++)
        cin>>v[i].fi>>v[i].se;
    sort(v, v+n, cmp);
    for (int i=0; i<n; i++)
        if (v[i].fi>=crt){
            sol++;
            crt=v[i].se;
        }
    cout<<sol;
 
    return 0;
}
```
</details>
<details><summary>Cod Dragulescu</summary>

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int Nmax = 2 * 1e5;

struct film{
    int start, finish;
};

bool cmp(film a, film b){
    if(a.finish == b.finish){
        return a.start < b.start;
    }
    return a.finish < b.finish;
}

film v[Nmax];

int main(){
    int n, timp, cate_filme;

    cin >> n;
    for(int i = 0; i < n; i++){
        cin >> v[i].start >> v[i].finish;
    }

    sort(v, v + n, cmp);

    timp = 0;
    cate_filme = 0;
    for(int i = 0; i < n; i++){
        if(v[i].start >= timp){
            cate_filme++;
            timp = v[i].finish;
        }
    }

    cout << cate_filme;

    return 0;
}
```
</details>

#### [Pbinfo - Rucsac](https://www.pbinfo.ro/probleme/1340/rucsac) (problema rucsacului fractionar)
* Observtia cheie este ca ne dorim sa luam obiecte care au pret cat mai mare si greutate cat mai mica. Cu alte cuvinte dorim sa luam obiecte cu raport valoare / greutate cat mai mare, deci le vom sorta dupa acest criteriu. 
* Este clar ca daca am ajuns la un obiect, atunci nu are sens sa-l impartim, decat daca greutatea lui este mai mare ca cea ramasa in rucsac.

<details><summary>Cod Tanasescu</summary>

```cpp
#include <iostream>
#include <algorithm>

using namespace std;
const int Nmax=1e3;

int n, Gmax;
double val, rap;
struct obiect{
    int G, V;
}v[Nmax];

// ordonam in ordine descrescatoare dupa report pret / greutate
bool cmp(obiect a, obiect b){
    // a.V / a.G > b.V / b.G <=> a.V * b.G > b.V * a.G
    return a.V * b.G > b.V * a.G;
}

int main(){
    cin>>n>>Gmax;
    for (int i=0; i<n; i++)
        cin>>v[i].G>>v[i].V;
    sort(v, v+n, cmp);
    for (int i=0; i<n; i++){
        if (Gmax>v[i].G){
            Gmax-=v[i].G;
            val+=v[i].V;
        }
        else{
            val+=v[i].V * Gmax / (double)v[i].G;
            break; // rucsacul nostru s-a umplut
        }
    }
    cout<<val;

    return 0;
}
```
</details>
<details><summary>Cod Dragulescu</summary>

```cpp
#include <iostream>
#include <algorithm>
#include <iomanip>

using namespace std;

const int Nmax = 1005;

struct obiect{
    int greutate, profit;
};

bool cmp(obiect a, obiect b){
    return a.profit * b.greutate > b.profit * a.greutate;
}

obiect v[Nmax];

int main(){
    int n, max_greutate, greutate_partiala;
    double profit_total;

    cin >> n >> max_greutate;
    for(int i = 0; i < n; i++){
        cin >> v[i].greutate >> v[i].profit;
    }

    sort(v, v + n, cmp);

    greutate_partiala = 0;
    profit_total = 0;
    for(int i = 0; i < n && greutate_partiala < max_greutate; i++){
        if(greutate_partiala + v[i].greutate <= max_greutate){
            profit_total += v[i].profit;
            greutate_partiala += v[i].greutate;
        }
        else{
            profit_total += (double)v[i].profit * ((double)max_greutate - greutate_partiala)/v[i].greutate;
            greutate_partiala = max_greutate;
        }
    }

    cout << fixed << setprecision(2) << profit_total;

    return 0;
}
```
</details>

#### [ONI IX 2023 - Bolovani](https://kilonova.ro/problems/539?list_id=195) (problema surprinzator de educationala pentru un ONI)
* In primul rand, consider ca este mai important sa stim ultima zi in care ne putem apuca de un bolovan decat ziua pana la care trebuie sa fie spart, deci vom nota cu $d'$ aceasta ultima zi.
* In al doilea rand, este bine sa ne gandim la o sortare, dupa ce ar fi benefic sa sortam bolovanii, dupa $z$? dupa $d$? dupa $d'$ poate dupa un raport asemanator cu problema anterioara? Putem sa incercam fiecare varianta si sa ne convingem de faptul ca cel mai bine ar fi sa le sortam dupa $d$ (deadlineul initial).
* Pe scurt, algoritmul va tine o multime de bolovani care *pe moment* intra in solutie.
Daca bolovanul la care am ajuns se incadreaza in timp, acesta este introdus in multime. Daca nu, incercam sa il inlocuim cu un bolovan din multimea solutiei care are timpul de spargere mai mare decat el.

<details><summary>Cod Tanasescu de acum</summary>

```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <fstream>

#define fi first
#define se second

using namespace std;
typedef long long ll;
typedef pair<ll, ll> pll;
const ll Nmax=1e4;

ifstream fin ("bolovani.in");
ofstream fout ("bolovani.out");

ll n, tcrt;
struct bolovan{
    ll ind, z, d, dprim;
    bool insol;
}v[Nmax];
pll sol[Nmax];

// ne dorim bolovanul care ia cel mai mult de spart
struct CMP{
    bool operator()(ll a, ll b){
        return (v[a].z<v[b].z);
    }
};
priority_queue<ll, vector<ll>, CMP> pq;

// ordonam crescator dupa deadline
bool cmp(bolovan a, bolovan b){
    return a.d<b.d;
}

int main(){
    fin>>n;
    for (int i=0; i<n; i++){
        v[i].ind=i; // este nevoie sa memoram indicele inainte de sortare
        fin>>v[i].z>>v[i].d;
        v[i].dprim=v[i].d-v[i].z+1;
    }
    sort(v, v+n, cmp);
    tcrt=1;
    for (int i=0; i<n; i++){
        if (v[i].dprim>=tcrt){ // putem adauga bolovanul in multimea solutiei
            pq.push(i);
            tcrt+=v[i].z;
        }
        else if (!pq.empty() && v[i].z<v[pq.top()].z){ // putem inlocui cu un alt bolovan din multime
            tcrt-=v[pq.top()].z;
            pq.pop();
            tcrt+=v[i].z;
            pq.push(i);
        }
        // else ignoram elementul 
    }
    fout<<pq.size()<<'\n';
    while (!pq.empty()){
        v[pq.top()].insol=1;
        pq.pop();
    }
    // contrium intervalele de timp pentru bolovanii din multime
    tcrt=1;
    for (int i=0; i<n; i++)
        if (v[i].insol){
            sol[v[i].ind].fi=tcrt;
            tcrt+=v[i].z;
            sol[v[i].ind].se=tcrt-1;
        }
    // contrium intervalele de timp pentru bolovanii din afara multimii
    for (int i=0; i<n; i++)
        if (!v[i].insol){
            sol[v[i].ind].fi=tcrt;
            tcrt+=v[i].z;
            sol[v[i].ind].se=tcrt-1;
        }
    for (int i=0; i<n; i++)
        fout<<sol[i].fi<<' '<<sol[i].se<<'\n';
    
    return 0;
}
```
</details>
<details><summary>Cod Tanasescu de acum un an</summary>

```cpp
#include <iostream>
#include <fstream>
#include <queue>
#include <algorithm>
#include <vector>

using namespace std;
ifstream fin ("bolovani.in");
ofstream fout ("bolovani.out");
const int Nmax=10000;

int n, ord[Nmax];
struct bolovan{
    long long ind, t, d, st;
    bool bf;
}v[Nmax];
struct cmp{
    bool operator()(int a, int b){
        return (v[a].t<v[b].t);
    }
};
priority_queue <int, vector<int>, cmp> q;

bool cmp(bolovan a, bolovan b){
    return a.d<b.d;
}
int main()
{
    fin>>n;
    for (int i=0; i<n; i++){
        fin>>v[i].t>>v[i].d;
        v[i].ind=i;
    }
    sort(v, v+n, cmp);
    for (int i=0; i<n; i++)
        ord[v[i].ind]=i;
    long long tt=0;
    for (int i=0; i<n; i++)
        if (tt+v[i].t<=v[i].d){
            tt+=v[i].t;
            q.push(i);
        }
        else if (!q.empty() && v[q.top()].t>v[i].t){
            tt+=v[i].t-v[q.top()].t;
            q.pop();
            q.push(i);
        }
    fout<<q.size()<<'\n';
    while (!q.empty()){
        v[q.top()].bf=1;
        q.pop();
    }
    long long crt=1;
    for (int i=0; i<n; i++)
        if (v[i].bf){
            v[i].st=crt;
            crt+=v[i].t;
        }
    for (int i=0; i<n; i++)
        if (!v[i].bf){
            v[i].st=crt;
            crt+=v[i].t;
        }
    for (int i=0; i<n; i++)
        fout<<v[ord[i]].st<<' '<<v[ord[i]].st+v[ord[i]].t-1<<'\n';
    return 0;
}
```
</details>

#
>Not so famous last words: Asta nu e dinamica?
## DP (Programare dinamica)
* Daca in cazul abordarii greedy, solutia se baza pe anumite reguli / patternuri pe care le putem observa in solutii, in cazul programarii dinamice, solutia este mai degraba logica o data ce o intelegem.
* In programarea dinamica (DP) ne folosim de chestii (stari) deja calculate pentru a ne forma solutia. De multe ori complexitatea va fi polinomiala $O(N^x)$ (asta pana cand vom invata dinamica pe stari exponentiale dar aia e o lectie pentru alta zi).
### **Probleme discutate**:
#### [Pbinfo - Sumtri1](https://www.pbinfo.ro/probleme/386/sumtri1)
* Observatia este ca ne-ar ajuta sa stim suma minima care se incheie cu elementul de pe pozitia $(l, c)$. Notam tabloul acestei sume cu `dp` si pe cel initial cu `mat`.
* Starea initiala va fi la pozitia $(1, 1)$ si va fi $dp[1][1]=mat[1][1]$.
* Putem construi urmatoarele stari prin formula $dp[l][c]=min(dp[l-1][c-1], dp[l-1][c])+mat[l][c]$.
* Putem reconstitui solutia uitandu-ne de la sfarsit catre inceput si punandu-ne intrebarea: **de unde am ajuns in pozitia aceasta**, al carui raspuns va fi: **din elementul cu dp-ul minim care se afla deasupra sau pe diagonala stanga sus**.

<details><summary>Cod Tanasescu de acum</summary>

```cpp
// Author: Tanasescu Andrei-Rares
#include <iostream>
#include <fstream>
#include <algorithm>
#include <cmath>
#include <map>
#include <unordered_map>
#include <set>
#include <unordered_set>
#include <queue>
#include <stack>
#include <deque>
#include <iomanip>
#include <vector>
#include <cassert>

#pragma GCC optimize("O3")

#define fi first
#define se second
#define pb push_back
#define pf push_front

using namespace std;

ifstream fin ("sumtri1.in");
ofstream fout ("sumtri1.out");

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const ll Nmax=105, inf=1e9+5;

int n;
int mat[Nmax][Nmax], dp[Nmax][Nmax];
int sol[Nmax];

int main()
{
    fin>>n;
    // bordare
    for (int i=0; i<=n; i++)
        dp[i][0]=dp[i][i+1]=inf;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=i; j++)
            fin>>mat[i][j];
    dp[1][1]=mat[1][1];
    for (int i=2; i<=n; i++)
        for (int j=1; j<=i; j++)
            dp[i][j]=mat[i][j]+min(dp[i-1][j-1], dp[i-1][j]);
    int pos, mn=inf;
    for (int j=1; j<=n; j++)
        if (dp[n][j]<mn){
            mn=dp[n][j];
            pos=j;
        }
    fout<<mn<<'\n';
    sol[n]=mat[n][pos];
    for (int i=n-1; i>=1; i--)
        if (dp[i][pos]<=dp[i][pos-1])
            sol[i]=mat[i][pos];
        else{
            sol[i]=mat[i][pos-1];
            pos--;
        }
    for (int i=1; i<=n; i++)
        fout<<sol[i]<<' ';

    return 0;
}
```
</details>
<details><summary>Cod Tanasescu din 2021 (reusisem sa il transform pe mat in dp si apoi inapoi in mat)</summary>

```cpp
#include <fstream>

using namespace std;
ifstream fin ("sumtri1.in");
ofstream fout ("sumtri1.out");

int m[102][102];
int mn=999999999,n,y;
int v[101];
int main()
{
    fin>>n;
    for (int i=0;i<=n+1;i++)
        m[i][0]=100001;
    m[1][2]=100001;
    fin>>m[1][1];
    for (int i=2;i<=n;i++)
    {
        for (int j=1;j<=i;j++)
        {
            fin>>m[i][j];
            m[i][j]+=min(m[i-1][j],m[i-1][j-1]);
        //fout<<m[i][j]<<' ';
        }
        m[i][i+1]=100001;
        //fout<<'\n';
    }
    for (int i=1;i<=n;i++)
        if (m[n][i]<mn)
        {
            mn=m[n][i];
            y=i;
        }
    fout<<mn<<'\n';
    int cnt=0;
    for (int i=n;i>=1;i--)
    {
        cnt++;
        v[cnt]=m[i][y]-min(m[i-1][y],m[i-1][y-1]);
        if (m[i-1][y-1]<m[i-1][y])
            y--;
    }
    for (int i=n;i>=1;i--)
        fout<<v[i]<<' ';

    return 0;
}
```
</details>

* **Atentie!** daca nu avem nevoie de reconstructia solutiei, este indeajuns sa pastram doar 2 linii din `dp` (cea actuala si ce anterioara), deoarece o stare se bazeaza doar pe stari de pe liniile anterioare.

<details><summary>Exemplu</summary>

```cpp
// Author: Tanasescu Andrei-Rares
#include <iostream>
#include <fstream>
#include <algorithm>
#include <cmath>
#include <map>
#include <unordered_map>
#include <set>
#include <unordered_set>
#include <queue>
#include <stack>
#include <deque>
#include <iomanip>
#include <vector>
#include <cassert>

#pragma GCC optimize("O3")

#define fi first
#define se second
#define pb push_back
#define pf push_front

using namespace std;

ifstream fin ("sumtri1.in");
ofstream fout ("sumtri1.out");

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const ll Nmax=105, inf=1e9+5;

int n;
int mat[Nmax][Nmax], dp[2][Nmax];

int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    cin>>n;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=i; j++)
            cin>>mat[i][j];
    dp[0][1]=mat[1][1];
    for (int i=2; i<=n; i++){
        dp[0][0]=dp[0][i]=inf;
        for (int j=1; j<=i; j++)
            dp[1][j]=mat[i][j]+min(dp[0][j-1], dp[0][j]);
        for (int j=1; j<=i; j++)
            dp[0][j]=dp[1][j];
    }
    int pos, mn=inf;
    for (int j=1; j<=n; j++)
        if (dp[0][j]<mn){
            mn=dp[0][j];
            pos=j;
        }
    cout<<mn<<'\n';

    return 0;
}
```
</details>

## Tema:
* Implementati toate problemele de azi. Chiar este important.
* [Infoarena - Lupul Urias si Rau](https://infoarena.ro/problema/lupu) (misto nume)
* [Pbinfo - Sumtri](https://www.pbinfo.ro/probleme/385/sumtri) (nu accept alta solutie decat cu 2 linii de dp)
* [CSES - Grid Paths](https://cses.fi/problemset/task/1638) (este chiar simpluta, iar ideea este extrem de clasica si comuna, deci importanta)
#
> Stăpâne, stăpâne,  
Mai chiamă ş-un câne
