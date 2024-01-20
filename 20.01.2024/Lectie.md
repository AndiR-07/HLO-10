> IIOT 2024 de la extaz la agonie...
# Problema ad-hoc a zilei:
## [IIOT 2024 R3 - Swapping Brackets (problema cu paranteze)](https://kilonova.ro/problems/2164?list_id=899)
![](https://kilonova.ro/assets/problem/2164/attachment/bracketswap.jpg)
(poza foarte inspirata)
### **Idei**:
* in momentul in care dam de o formatiune `()` o ignoram.
* in momentul in care dam de `)` si nu avem nicio `(` disponibila in stanga ei, atunci este clar ca `)` va deveni `(` si deci putem face aceasta schimbare din start.
### **Codul meu:**
<details><summary>Spoiler</summary>

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

ifstream fin ("");
ofstream fout ("");

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const ll Nmax=1e6+5, inf=1e9+5;

int n;
string s;
stack <int> pi;
stack <int> pd;
vector<pii> sol;

int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    cin>>n;
    cin>>s;
    for (int i=0; i<s.size(); i++){
        if (s[i]=='(')
            pd.push(i);
        else if (pd.empty()){
            pi.push(i);
            pd.push(i);
        }
        else pd.pop();
    }
    while (!pi.empty()){
        sol.pb({pi.top(), pd.top()});
        pi.pop();
        pd.pop();
    }
    cout<<sol.size()<<'\n';
    for (auto it:sol)
        cout<<it.fi<<' '<<it.se<<'\n';

    return 0;
}
```
</details>
(Puteti sa ignorati include-urile / typedef-urile / pragmaurile / define-urile inutile de sus. Sunt puse automat la orice cod de-al meu ca sa imi faca viata mai usoara. Daca nu stiti cum functioneaza puteti sa ma intrebati)

### **Codul colegului meu, Dragulescu Andrei**
<details><summary>Spoiler</summary>

```cpp
#include <iostream>
#include <string>
#include <vector> 

using namespace std;

vector<pair<int, int>> sol;
string s;

int main(){
    int n, p1, p2, op, cnt;
    
    cin >> n >> s;
    
    op = 0;
    cnt = 0;
    p2 = n - 1;
    for(p1 = 0; p1 < n; p1++){
        if(s[p1] == '('){
            cnt++;
        }
        else{
            cnt--;
            
            if(cnt < 0){
                op++;
                
                while(p2 >= 0 && s[p2] != '('){
                    p2--;
                }
                
                swap(s[p1], s[p2]);
                sol.push_back({p1, p2});
                
                cnt += 2;
                p2--;
            }
        }
    }
    
    cout << op << '\n';
    for(pair<int, int> indici : sol){
        cout << indici.first << " " << indici.second << '\n';
    }

    return 0;
}
```
</details>

#
# Probleme cu Stiva:
## [Pbinfo UEMM](https://www.pbinfo.ro/probleme/1883/uemm)
* discutata rapid, se poate rezolva in $O(N^2)$ cautand pentru fiecare element in mod naiv urmatorul element mai mare

<details><summary>Solutie</summary>

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

ifstream fin ("");
ofstream fout ("");

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const ll Nmax=1e3+5, inf=1e9+5;

int n, v[Nmax];

int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    cin>>n;
    for (int i=0; i<n; i++)
        cin>>v[i];
    for (int i=0; i<n; i++){
        int j=i+1;
        while (j<n && v[j]<=v[i])
            j++;
        // nu am gasit element mai mare
        if (j==n)
            cout<<"-1 ";
        // pozitia elementului mai mare este chiar j
        else cout<<v[j]<<' ';
    }

    return 0;
}
```
</details>

## [Pbinfo UEMM1](https://www.pbinfo.ro/probleme/1884/uemm1)
* $N$-ul este ceva mai mare, deci avem nevoie de un algoritm in $O(N)$.
* Ne vom folosi de un concept numit stiva ordonata care se bazeaza pe urmatorii pasi:
    * Iteram prin fiecare element si ne mentinem o stiva
    * Notam elementul curent cu `i` si varful stivei cu `top`:
        *  cat timp stiva nu este goala si `top` este mai mic decat `i`
            * urmatorul element mai mare pentru `top` este chiar `i`, deoarece daca ar fi fost alt element, `top ar fi fost deja scos din stiva`
            * `top` este scos din stiva
        * `i` este adaugat in stiva
    * iteram prin toate elementele ramase in stiva. Deoarece ele nu au fost scoase, inseamna ca nu s-a gasit niciun element mai mare decat ele la dreapta
* Observam ca fiecare element eset bagat in stiva o data si scos tot o data, deci folosindu-ne de *analiza amortizata* complexitatea de timp va fi $O(N)$.
### Codul colegului meu, Andrei Dragulescu:

<details><summary>Spoiler</summary>

```cpp
#include <iostream>
#include <stack>

using namespace std;

const int Nmax = 1005;

stack<int> st;

int v[Nmax], uemm[Nmax];

int main(){
    int n;
    
    cin >> n;
    for(int i = 1; i <= n; i++){
        cin >> v[i];
    }
    
    for(int i = 1; i <= n; i++){
        while(!st.empty() && v[st.top()] < v[i]){
            uemm[st.top()] = v[i];
            st.pop();
        }
        st.push(i);
    }
    while(!st.empty()){
        uemm[st.top()] = -1;
        st.pop();
    }
    
    for(int i = 1; i <= n; i++){
        cout << uemm[i] << " ";
    }

    return 0;
}
```
</details>

### Codul meu din 2021:
<details><summary>Spoiler</summary>

```cpp
#include <iostream>

using namespace std;
int st[100000],n,sf=0,x,i;
int v[100000],cod[100000];
int main()
{
    cin>>n;
    for (i=0;i<n;i++)
    {
        cin>>v[i];
        st[sf]=i;
        while (sf!=0 && v[st[sf-1]]<v[i])
        {
            sf--;
            cod[st[sf]]=v[i];
        }
        st[sf]=i;
        sf++;
    }
    for (i=0;i<sf;i++)
        cod[st[i]]=-1;
    for (i=0;i<n;i++)
        cout<<cod[i]<<' ';
    return 0;
}
```
</details>

## [Nerdarena skyline](https://www.nerdarena.ro/problema/skyline)
* problema foarte importanta ce se rezolva cu stiva
* putem afla pentru fiecare bloc cu inaltimea `h` un interval maximal $(L, R)$ a.i. inaltimea blocurilor cuprinse in interval sa fie $\ge h$. Cu alte cuvinte, un dreptungi de inaltime `h` se poate extinde pana la `L` neinclusiv si pana la `R` neinclusiv. Pentru a calcula aria lui mai avem nevoie de latimea tuturor blocurilor $\in [L, R]$, lucru ce se poate calcula printr-un vector de sume partiale.
* observam ca nu poate exista un bloc de arie mai mare care sa nu fie determinat prin procedeul enuntat anterior, deoarece, daca ar exista, ar insemna ca ori nu este maximal ca inaltime, ori nu este maximal ca latime (se mai poate extinde ori in sus ori pe latime).
### Solutie:

<details><summary>Spoiler</summary>

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

ifstream fin ("skyline.in");
ofstream fout ("skyline.out");

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const ll Nmax=40000, inf=1e9+5;

int n, h[Nmax+1], l[Nmax+1], st[Nmax+1], dr[Nmax+1];
ll sp[Nmax+1];
int stiva[Nmax+1], p=0;
ll mx=0;

int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    fin>>n;
    for (int i=1; i<=n; i++){
        fin>>h[i]>>l[i];
        sp[i]=sp[i-1]+l[i];
    }
    // cel mai mic element din dreapta lui
    for (int i=1; i<=n; i++){
        while (p!=0 && h[stiva[p]]>h[i]){
            dr[stiva[p]]=i;
            p--;
        }
        stiva[++p]=i;
    }
    while (p!=0){
        dr[stiva[p]]=n+1;
        p--;
    }
    // cel mai mic element din stanga lui
    for (int i=n; i>=1; i--){
        while (p!=0 && h[stiva[p]]>h[i]){
            st[stiva[p]]=i;
            p--;
        }
        stiva[++p]=i;
    }
    while (p!=0){
        st[stiva[p]]=0;
        p--;
    }
    // determinare arie
    for (int i=1; i<=n; i++){
        ll aria=(sp[dr[i]-1]-sp[st[i]])*h[i];
        if (aria>mx)
            mx=aria;
    }
    fout<<mx;

    return 0;
}
```
</details>

#
# Probleme cu Coada:

## [Pbinfo Acces](https://www.pbinfo.ro/probleme/866/acces)
* Vom avea nevoie de un algoritm ce va rula in complexitate $O(N \cdot M)$
* Algoritmul ce rezolva aceasta problema se numeste algoritmul lui Lee:
    * Ne tinem o coada in care introducem prima oara pozitia de start
    * Pentru fiecare element din coada:
        * Pentru fiecare vecin al sau:
            * Daca veccinul nu este zid si nu a fost inca parcurs, atunci:
                * distanta de la pozitia de start la vecin este distanta de le pozitia de start la elementul curent din coada $+1$
                * adaugam vecinul la coada
        * scoatem elementul din coada
* Cateva concepte care ajuta la implementare sunt:
    * Bordarea matricei: atribuim tuturor elementelor de pe marginea matricei valoarea de zid
    * Vectori de directie: construim 2 vectori, de linie si de coloana, fiecare cu 4 elemente (directii) care sa reprezinte diferenta de la pozitia initiala la pozitia dupa mutarea in directia respectiva pentru linie / coloana
### Codul colegului meu, Andrei Dragulescu:

<details><summary>Spoiler</summary>

```cpp
#include <fstream>
#include <queue>

using namespace std;

const int Nmax = 1005;
const int NoDir = 4;

int mat[Nmax][Nmax], dist[Nmax][Nmax];
queue<pair<int, int>> q;

int dirX[] = {1, -1, 0,  0};
int dirY[] = {0,  0, 1, -1};

int main(){
    ifstream cin("acces.in");
    ofstream cout("acces.out");
    
    int n, m;
    char c;
    pair<int, int> poz_start, poz, vecin;

    cin >> n >> m;
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= m; j++){
            cin >> c;

            if(c == '#'){
                mat[i][j] = 1;
            }
            else{
                mat[i][j] = 0;

                if(c == 'P'){
                    poz_start = {i, j};
                }
            }
            dist[i][j] = -1;
        }
    }

    ///bordare matrice
    for(int i = 0; i <= n + 1; i++){
        mat[i][0] = 1;
        mat[i][m + 1] = 1;
    }
    for(int j = 0; j <= m + 1; j++){
        mat[0][j] = 1;
        mat[n + 1][j] = 1;
    }

    q.push(poz_start);
    dist[poz_start.first][poz_start.second] = 0;
    while(!q.empty()){
        poz = q.front();
        q.pop();

        for(int h = 0; h < NoDir; h++){
            vecin = {poz.first + dirX[h], poz.second + dirY[h]};

            if(mat[vecin.first][vecin.second] == 0 && dist[vecin.first][vecin.second] == -1){
                dist[vecin.first][vecin.second] = dist[poz.first][poz.second] + 1;
                q.push(vecin);
            }
        }
    }

    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= m; j++){
            cout << dist[i][j] << " "; 
        }
        cout << '\n';
    }

    return 0;
}
```
</details>

### Codul meu din 2021:
<details><summary>Spoiler</summary>

```cpp
#include <iostream>
#include <fstream>

#define Nmax 1000
using namespace std;
ifstream fin ("acces.in");
ofstream fout ("acces.out");

short n,m,i,j,ip,jp,pozi,pozj;
int crt,crtadd;
char ch;
short M[Nmax+2][Nmax+2], dist[Nmax+2][Nmax+2];
short qi[Nmax*Nmax+1], qj[Nmax*Nmax+1];
short dl[]={-1,0,1,0}, dc[]={0,1,0,-1};
int main()
{
    fin>>n>>m;
    for (i=1;i<=n;i++)
    {
        for (j=1;j<=m;j++)
        {
            fin>>ch;
            if (ch=='-')
                M[i][j]=1;
            else if (ch=='P')
                {
                    ip=i;
                    jp=j;
                }
            else dist[i][j]=-1;
        }
    }
    qi[crt]=ip;
    qj[crt]=jp;
    while (crt<=crtadd)
    {
        pozi=qi[crt];
        pozj=qj[crt];
        for (i=0;i<4;i++)
        {
            if (M[pozi+dl[i]][pozj+dc[i]]==1)
            {
                crtadd++;
                qi[crtadd]=pozi+dl[i];
                qj[crtadd]=pozj+dc[i];
                dist[pozi+dl[i]][pozj+dc[i]]=dist[pozi][pozj]+1;
                M[pozi+dl[i]][pozj+dc[i]]=0;
            }
        }
        crt++;
    }
    for (i=1;i<=n;i++)
    {
        for (j=1;j<=m;j++)
        {
            if (dist[i][j]==0 && (i!=ip || j!=jp))
                fout<<"-1 ";
            else fout<<dist[i][j]<<' ';
        }
        fout<<'\n';
    }
    return 0;
}
```
</details>

#
# Probleme cu Deque (sau poate doar 2 pointers)
## [CSES Playlist](https://cses.fi/problemset/task/1141)
* Avem nevoie de solutie in $O(N)$.
* Ne tinem 2 pointeri, notati cu `st`(stanga) si `dr`(dreapta) si un set.
* Iteram cu `dr` de la `1` la `N`:
    * Atat timp cat in set mai exista un element cu valoarea lui `dr`:
        * scoatem  din set elementul cu valoarea lui `st`
        * Crestem `st` cu 1
    * Adaugam in set valoarea de pe pozitia `dr`
    * Verificam daca lungimea set-ului curent este mai mare decat maximul
* Afisam maximul
### Solutie:
<details><summary>Spoiler</summary>

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
 
ifstream fin ("");
ofstream fout ("");
 
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
 
const ll Nmax=2e5+5, inf=1e9+5;
 
int n, p1, p2, v[Nmax], mx;
set <int> st;
 
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
 
    cin>>n;
    p1=0;
    p2=-1;
    for (int i=0; i<n; i++){
        p2++;
        cin>>v[i];
        while (st.count(v[i])){
            st.erase(v[p1]);
            p1++;
        }
        st.insert(v[i]);
        int dist=p2-p1+1;
        if (dist>mx)
            mx=dist;
    }
    cout<<mx;
 
    return 0;
}
```
</details>

#
# **Tema**:
## Probleme cu Stiva:
* ### [Nerdarena Dreptunghi1](https://www.nerdarena.ro/problema/dreptunghi1)
    (odata ce faceti problema asta va veti simti ca si cum ati terminat informatica)
## Probleme cu Coada: 
* ### [Pbinfo Mewtwo](https://www.pbinfo.ro/probleme/4505/mewtwo)
* ### [Infoarena Barbar](https://www.infoarena.ro/problema/barbar)
    (ceva mai gretua, genul de problema care se poate da la o judeteana de a 10 a ca problema usoara)
## Probleme cu Deque:
* ### [Infoarena Vila2](https://www.infoarena.ro/problema/vila2)
    (Ca hint, folositi deque-ul ca o stiva din care poti sa scoti si primul element)

    (Ca hint-hint, folositiva de conceptul de stiva ordonata)
* ### [USACO Gold Haybale Feast](https://www.usaco.org/index.php?page=viewproblem2&cpid=767)
* ### [CSES Maximum Subarray Sum](https://cses.fi/problemset/task/1643)
* ### [CSES Maximum Subarray Sum II](https://cses.fi/problemset/task/1644)

> Good luck, have fun. 8 weeks left

