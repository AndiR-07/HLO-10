# MATRICI
## Probleme ad-hoc:
- [COCI 2023-2024 R2 Pahuljice](https://evaluator.hsin.hr/tasks/HONI232423pahuljice/)
```cpp
#include <iostream>

using namespace std;
const int Nmax=55; //dimensiunea matricei

int n, m, sol;
char mat[Nmax][Nmax];

int main()
{
    cin>>n>>m;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=m; j++)
            cin>>mat[i][j];

    for (int i=1; i<=n; i++)
        for (int j=1; j<=m; j++)
            if (mat[i][j]=='+'){
                int k=1;
                // marim lungimea fulgului de nea daca caracterele din cele 8 directii sunt in concordanta cu cerinta
                while (mat[i][j+k]=='-' && mat[i][j-k]=='-' &&
                       mat[i+k][j]=='|' && mat[i-k][j]=='|' &&
                       mat[i-k][j+k]=='/' && mat[i+k][j-k]=='/' &&
                       mat[i-k][j-k]=='\\' && mat[i+k][j+k]=='\\')
                    k++;
                sol=max(sol, k-1);
            }
    cout<<sol;
    return 0;
}
```
## Sume partiale pe matrice:
-[Pbinfo - Cirese1](https://www.pbinfo.ro/probleme/765/cirese1)
```cpp
#include <iostream>

using namespace std;
const int Nmax=1005; //dimensiunea matricei

int n, m, k, mx;
// matricea simpla   matricea de sume partiale
int mat[Nmax][Nmax], sp[Nmax][Nmax];

int main()
{
    cin>>n>>m>>k;

    //creare matrice de sume partiale
    for (int i=1; i<=n; i++)
        for (int j=1; j<=m; j++){
            cin>>mat[i][j];
            // se calculeaza suma partiale pentru patratul (i, j) pe baza celor calculate anterior
            sp[i][j]=mat[i][j]+sp[i-1][j]+sp[i][j-1]-sp[i-1][j-1];
        }

    int i1, j1, i2, j2;
    for (int i=0; i<k; i++){
        cin>>i1>>j1>>i2>>j2;
        // se calculeaza submatricea cu colturile (i1, j1), (i2, j2)
        mx=max(mx, sp[i2][j2]-sp[i2][j1-1]-sp[i1-1][j2]+sp[i1-1][j1-1]);
    }
    cout<<mx;
    return 0;
}
```
## Smenul lui Mars pe matrice
-[Pbinfo - Diff2dArrays](https://www.pbinfo.ro/probleme/3903/diff2darrays)
```cpp
#include <iostream>

//linia asta face programul mai rapid
#pragma GCC optimize ("O3")

using namespace std;
const int Nmax=505;

int n, q;
int mat[Nmax][Nmax], mars[Nmax][Nmax];

int main()
{
    cin>>n;
    for (int i=1; i<=n; i++)
        for (int j=1; j<=n; j++)
            cin>>mat[i][j];

    cin>>q;
    int i1, j1, i2, j2, val;
    // pregatim matricea mars
    for (int i=0; i<q; i++){
        cin>>i1>>j1>>i2>>j2>>val;
        mars[i1][j1]+=val;
        mars[i1][j2+1]-=val;
        mars[i2+1][j1]-=val;
        mars[i2+1][j2+1]+=val;
    }

    //calculam matricea mars pe baza rezultatelor anterioare (ca la sume partiale)
    for (int i=1; i<=n; i++){
        for (int j=1; j<=n; j++){
            mars[i][j]+=mars[i][j-1]+mars[i-1][j]-mars[i-1][j-1];
            cout<<mat[i][j]+mars[i][j]<<' ';
        }
        cout<<'\n';
    }
    
    return 0;
}
```
## Alte Materiale folositoare:
- [Materialul Pbinfo](https://www.pbinfo.ro/articole/2541/Smenul-lui-mars-difference-array)
- [Materialul Algopeida](https://www.algopedia.ro/wiki/index.php/Clasa_a_VII-a_lec%C8%9Bia_5_-_10_oct_2019#%C3%8Embun%C4%83t%C4%83%C8%9Bire)
# Tema:
- [Nerdarena - flori](https://www.nerdarena.ro/problema/flori)
- [ONI 2010 - Plaja](https://www.pbinfo.ro/probleme/2383/plaja1)
- [ONI 2019 - amat](https://www.pbinfo.ro/probleme/3042/amat)
- [IIOT 2020-2021 R3 - Vaccination](https://kilonova.ro/problems/976)

