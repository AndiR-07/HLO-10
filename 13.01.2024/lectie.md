> **Biblia lectiei care urmeaza: [cpp_reference](https://en.cppreference.com/w/), disponibila si in cursul olimpiadei**
# Structuri in C++
```cpp
struct nume_structura{
    // variabile
    int ...
    float ...
    vector<int> ...
    ...

    // constructor (daca nu exista toate elementele vor fi nedefinite)
    nume_structura(parametrii){
        // definim elementele din structura
        ...
    }

    // functii 
    int nume_functie1(){
        ...
        return rezultat;
    }
    void nume_functie2(){
        ...
    }
    ...

    // operatori (programul nu va stii de exemplu cum sa adune 2 structuri 'nume_structura')
    // asa ca trebuie sa il ajutam
    tip_de_date_rezultat operator+(nume_structura const& x){
        /// ...
        return rezultat; // returneaza rezultatul dintre adunare structurii curente cu x
    }
    bool operator<(nume_structura const& x){
        /// ...
        return rezultat; // returneaza 1 daca structura curenta e mai mica decat x, 0 altfel
    }
    ...
};
```
## functionalitati:
* ne lasa sa ne cream propriul nostru tip de date / container.
## exemple de utilizare:
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct elev{
    string Nume;
    int Inaltime;
    float Medie_generala;
    elev(string nume, int inaltime, float medie_generala){ // constructor 
        Nume = nume;
        Inaltime = inaltime;
        Medie_generala = medie_generala;
    }
    // vom considera un elev mai mic decat altul daca are media mai mica. La medii egale, 
    // daca e mai scund. La aceeasi inaltime daca are numele mai mic d.p.d.v. alfabetic.
    // daca primul element e mai mic returnam 1, alfel 0. 
    // !!! Daca sunt egale trb mereu returnat 0

    bool operator<(elev const& x){
        if (Medie_generala != x.Medie_generala)
            return Medie_generala < x.Medie_generala;
        if (Inaltime != x.Inaltime)
            return Inaltime < x.Inaltime;
        if (Nume != x.Nume)
            return Nume < x.Nume;
        // sunt egale deci returnam 0;
        return 0;
    }
};
struct clasa{
    vector <elev> Elevi;
    clasa(vector<elev> elevi){
        Elevi=elevi;
    }
    void adauga_elev(elev x){
        Elevi.push_back(x);
    }
    void afiseaza_numele_elevilor(){
        for (auto it:Elevi)
            cout<<it.Nume<<' ';
    }
    vector<elev> arata_premiantii(){ // functie ce returneaza un vector de elevi premianti
        vector<elev> v;
        for (auto it:Elevi) // parcurgem elevii
            if (it.Medie_generala>=9.5)
                v.push_back(it);
        return v;
    }
    clasa operator+(clasa const& x){
        // initializam rezultatul cu elevii structurii curente
        clasa rez(Elevi);
        // aduagam la rezultat elevii structurii de dupa '+'
        for (auto it:x.Elevi)
            rez.adauga_elev(it);
        return rez;
    }
};

int main(){
    elev Ian("Ian", 170, 9.50);
    elev Azteca("Azteca", 180, 9.50);
    elev Amuly("Amuly", 175, 6.50);
    elev Oscar("Oscar", 180, 9.75);

    if (Azteca<Ian)
        cout<<"Azteca e mai mic decat Ian\n";
    else if (Ian<Azteca)
        cout<<"Ian e mai mic decat Azteca\n";
    else cout<<"Azteca e egal cu Ian\n";
    // cum media lor este egala dar Azteca e mai inalt se va afisa "Ian e mai mic decat Azteca"

    clasa Aprilie1({Azteca, Amuly});
    clasa Restul({Ian, Oscar});

    vector<elev> premianti = Aprilie1.arata_premiantii();
    // cum Amuly nu este premiant se va afisa Azteca;
    for (auto it:premianti)
        cout<<it.Nume<<' ';
    cout<<'\n';


    // concatenam Aprilie1 si Restul in clasa familie
    clasa familie=Aprilie1+Restul;
    // se vor afisa numele tuturor elevilor, premianti sau nepremianti
    familie.afiseaza_numele_elevilor();
    cout<<'\n';
}
```
* [Material Geeks for Geek despre supraincarcarea operatorilor](https://www.geeksforgeeks.org/operator-overloading-cpp/) (ei au implementat o clasa insa structul se comporta la fel)
* [Material Usaco despre supraincarcarea operatorilor](https://usaco.guide/silver/sorting-custom?lang=cpp)

# String
```cpp
#include <string> // optional
```
## functionalitati:
* e mult mai usor de folosit si mai sigur decat un vector clasic de caractere (`char v[100]`)
* nu e nevoie sa stim de dinainte numarul de caractere din sir
* se poate accesa caracterul de pe pozitia k prin [ ] (`string s="abc"; cout<<s[1]; //'b'`)
* daca vrem sa concatenam 2 siruri stringuri, se poate utiliza operatorul '+'
```cpp
    string s1="Tanasescu ";
    string s2="Andrei";
    string sefu=s1+s2;
    cout<<sefu;
    // "Tanasescu Andrei"
```
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/12645/cpp-stl-string)
* [Materialul W3schools](https://www.w3schools.com/cpp/cpp_strings.asp)
* [Materialul Geeks for Geek](https://www.geeksforgeeks.org/stdstring-class-in-c/)

## functii:
* `s.front()`: returneaza primul caracter $<=>$ `s[0]`. $O(1)$
* `s.back()`: returneaza ultimul caracter $<=>$ `s[s.size()-1]`. $O(1)$
* `s.empty()`: returneaza 1 daca e gol, alfel 0 $<=>$ `s.size()==0`. $O(1)$
* `s.size()` $<=>$ `.length()`: returneaza numarul de caractere. $O(1)$
* `s.push_back(ch)`: adauga caracterul 'ch' la final. $O(1)$
* `s.pop_back()`: sterge ultimul element. $O(1)$
* `s.find(sir_de_caractere)`: returneaza prima pozitie din s a.i. susbsecventa care incepe cu pozitia respactive si 'sir_de_caractere' sunt egale. Complexitatea nu e precizata si imi e frica ca converge catre $O(N^2)$.
* `s.clear()`: sterge toate caracterele din string. $O(N)$
* `s.erase(it)`: sterge elementul de la **iteratorul** it. Atentie, iteratorul it, nu pozitia it. $O(N)$
* `s.substr(pozitie, nr_elem)`: returneaza subsecventa care incepe la pozitia 'pozitie' si contine 'nr_elem' caractere. $O(nr\_elem)$
* `swap(s1, s2)`: interschimba sirul s1 cu sirul s2. $O(1)$
# vector
```cpp
#include <vector>
```
## functionalitati:
* permite sa operam pe un vector in care nu stim exact numarul de elemente cand il declaram
* **momentan** nu poate fi implementat *de mana*
* [Materialul Pbinfo 1](https://www.pbinfo.ro/articole/12533/cpp-stl-vector)
* [Materialul Pbinfo 2](https://www.pbinfo.ro/articole/23705/cpp-vector-scurt-tutorial)
## functii:
* `v.push_back(nr)`: adauga nr la sfarstiul vectorului. $O(1)$
* `v.pop_back()`: sterge ultimul elemnt din vector. $O(1)$
* `v.size()`: returneaza lungimea vectorului. $O(1)$
* `v.empty()`: returneaza 1 daca e gol si 0 daca nu e gol $<=>$ `v.size()==0`. $O(1)$
* `v.front()`: returneaza valoarea primului numar. $O(1)$
* `v.back()`: returneaza valoarea ultimului numar. $O(1)$
* `v.clear()`: sterge toate elementele din vector. $O(N)$
* `v.erase(it)`: sterge elementul de la **iteratorul** it. Atentie, iteratorul it, nu pozitia it. $O(N)$
* `swap (v1, v2)`: interschimbam vectorii v1 si v2. $O(1)$
# Stack
```cpp 
#include <stack> 
```
![](https://www.softwaretestinghelp.com/wp-content/qa/uploads/2019/06/pictorial-representation-of-stack.png)
## functionalitati:
* tip de date *LIFO* (last in first out)
* necesar cand ne intereseaza doar ultimul element bagat in stiva, si nu celelalte
* poate fi implementat si de mana:
```cpp
struct stack{
    int nr_elem=0;
    vector<int> v;
    void push(int nr){
        v.push_back(nr);
    }
    void pop(){
        v.pop_back();
    }
    int size(){
        return v.size();
    }
    bool empty(){
        return v.size()==0;
    }
    int top(){
        return v[v.size()-1];
    }
};
```
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/19577/stiva)
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/stack-in-cpp-stl/)
## problema discutata [Paranteze3](https://www.pbinfo.ro/probleme/852/paranteze3)

<details><summary>Solutie</summary>

```cpp
#include <vector>
#include <string>
#include <iostream>
#include <fstream>

using namespace std;
ifstream fin ("paranteze3.in");
ofstream fout ("paranteze3.out");
struct stiva{
    vector<char> stv;

    void push(char x){
        stv.push_back(x);
    }
    void pop(){
        stv.pop_back();
    }
    char top(){
        return stv[stv.size() - 1];
    }
    bool empty(){
        return stv.empty();
    }
};

stiva st;

int main(){
    int n, m, val;
    bool eParantezatCorect;
    char c;
    string s;

    fin >> n;
    
    for(int i = 1; i <= n; i++){
        fin >> s;

        eParantezatCorect = 1;
        for(int j = 0; j <s.size(); j++){
            c=s[j];

            if(eParantezatCorect){
                if(c == '(' || c == '['){
                    st.push(c);
                }
                else if(c == ')'){
                    if(st.top() == '('){
                        st.pop();
                    }
                    else{
                        eParantezatCorect = 0;
                    }
                }
                else{ // c = ']'
                    if(st.top() == '['){
                        st.pop();
                    }
                    else{
                        eParantezatCorect = 0;
                    }
                }
            }
        }
        if(!st.empty()){
            eParantezatCorect = 0;
            while(!st.empty()){
                st.pop();
            }
        }
        fout<<eParantezatCorect<<'\n';
    }

    return 0; 
}
```
</details>

## functii:
* `st.push(nr)`: adauga nr la stiva. $O(1)$
* `st.pop()`: sterge ultimul element. $O(1)$
* `st.top()`: returneaza ultimul element $O(1)$
* `st.size()`: returneaza lungimea stivei. $O(1)$
* `st.empty()`: returneaza 1 daca e goala si 0 daca nu e goala $<=>$ `v.size()==0`. $O(1)$
* `swap (st1, st2)`: interschimbam stivele st1 si st2. $O(1)$
* `st.clear()`: se sterg toate elementele din 'st'. $O(N)$
# Queue
```cpp
#include <queue>
```
![](https://cdn.educba.com/academy/wp-content/uploads/2020/01/c-queue.jpg)
## functionalitati:
* tip de date *FIFO* (first in, first out)
* necesar cand vrem sa analizam / scoatem elementele in ordinea in care au fost bagate
* poate fi implementat si de mana:
```cpp
struct queue{
    int nr_elem=0, posi=0;
    vector<int> v;
    void push(int nr){
        v.push_back(nr);
    }
    void pop(){
        posi++;
    }
    int size(){
        return v.size();
    }
    bool empty(){
        return v.size()==0;
    }
    int front(){
        return v[posi];
    }
    int back(){
        return v[v.size()-1];
    }
};
```
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/19579/coada)
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/queue-cpp-stl/)
## functii:
* `q.push(nr)`: adauga nr la finalul cozii. $O(1)$
* `q.pop()`: sterge primul element. $O(1)$
* `q.front()`: returneaza valoarea primului element. $O(1)$
* `q.back()`: returneaza valoarea ultimului element. $O(1)$
* `q.size()`: returneaza lungimea cozii. $O(1)$
* `q.empty()`: returneaza 1 daca e goala si 0 daca nu e goala $<=>$ `q.size()==0`. $O(1)$
* `swap (q1, q2)`: interschimbam cozile q1 si q2. $O(1)$
* `q.clear()`: se sterg toate elementele din 'q'. $O(N)$
# Deque
```cpp
#include <deque>
```
![](https://media.geeksforgeeks.org/wp-content/uploads/20221017165808/Deque.png)

front este prima pozitie, back/rear este ultima pozitie
## functionalitati:
* putem sa stergem / adaugam elemente la ambele capete
* desi iese din scopul cozii cu 2 capete, putem sa accesam al k-ulea element prin ```dq[k]```
* este ceva mai greu de implementat de mana (daca isi da seama cnv de o implementare mai simpla lmk):
```cpp
struct deque{
    vector<int> st, dr;
    int p1=-1, p2=-1;
    bool vp1=0, vp2=1; // vectorul in care se afla p1 / p2
    int size(){
        int sz;
        if (vp1==vp2)
            if (vp1==1)
                sz=p2-p1+1;
            else sz=p1-p2+1;
        else sz=p1+p2+2;
        return sz;
    }
    bool empty(){
        return size()==0;
    }
    void push_back(int nr){
        bool empt;
        if (empty())
            empt=1;
        if (vp2==1){
            if (p2+1==dr.size())
                dr.emplace_back(); // se adauga un element gol
            dr[++p2]=nr;
        }
        else if (p2<=0){
            p2=0;
            if (p2==dr.size())
                dr.emplace_back(); // se adauga un element gol
            vp2=1;
            dr[p2]=nr;
        }
        else st[--p2]=nr;
        if (empt){
            p1=p2;
            vp1=vp2;
        }
    }
    void push_front(int nr){
        bool empt;
        if (empty())
            empt=1;
        if (vp1==0){
            if (p1+1==st.size())
                st.emplace_back(); // se adauga un element gol
            st[++p1]=nr;
        }
        else if (p1<=0){
            p1=0;
            if (p1==st.size())
                st.emplace_back(); // se adauga un element gol
            vp1=0;
            st[p1]=nr;
        }
        else dr[--p1]=nr;
        if (empt){
            p2=p1;
            vp2=vp1;
        }
    }
    void pop_back(){
        if (vp2==0)
            p2++;
        else if (p2==0)
            vp2=0;
        else p2--;
    }
    void pop_front(){
        if (vp1==1)
            p1++;
        else if (p1==0)
            vp1=1;
        else p1--;
    }
    int front(){
        if (vp1==0)
            return st[p1];
        else return dr[p1];
    }
    int back(){
        if (vp2==0)
            return st[p2];
        else return dr[p2];
    }
};
```
* [Materialul Infoarena](https://www.infoarena.ro/deque-si-aplicatii)
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/deque-cpp-stl/)
## functii: 
* `dq.push_back()`: se adauga nr la final. $O(1)$
* `dq.push_front()`: se adauga nr la inceput. $O(1)$
* `dq.pop_back()`: se sterge ultimul numar. $O(1)$
* `dq.pop_front()`: se sterge primul numar. $O(1)$
* `dq.back()`: se returneaza valoarea ultimului numar. $O(1)$
* `dq.front()`: se returneaza valoarea primului numar. $O(1)$
* `dq.size()`: returneaza lungimea cozii. $O(1)$
* `dq.empty()`: returneaza 1 daca e goala si 0 daca nu e goala $<=>$ `q.size()==0`. $O(1)$
* `swap (dq1, dq2)`: interschimbam cozile dq1 si dq2. $O(1)$
* `dq.clear()`: se sterg toate elementele din 'dq'. $O(N)$
# Priority Queue
```cpp
#include <queue> //! nu priority_queue
```
![](https://media.geeksforgeeks.org/wp-content/uploads/20221208162308/max_priority_queue.png)
## functionalitati:
* folositoare cand dorim sa sortam elementele dupa un anumit criteriu astfel incat primul element sa fie cel mai mare.
* avem acces doar la cel mai mare element
* **momentan** nu putem sa implemantam de mana, deoarece intern, se foloseste un **heap**, structura de date despre care nu am vorbit
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/23915/cpp-stl-priority-queue)
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/priority-queue-in-cpp-stl/)
## functii:
* `pq.push(nr)`: aduagam elementul nr. $O(log N)$
* `pq.pop()`: stergem cel mai mare element. $O(log N)$
* `pq.top()`: returneaza valoarea celui mai mare element. $O(1)$
* `pq.size()`: returneaza lungimea cozii. $O(1)$
* `pq.empty()`: returneaza 1 daca e goala si 0 daca nu e goala $<=>$ `pq.size()==0`. $O(1)$
* `swap (pq1, pq2)`: interschimbam cozile pq1 si pq2. $O(1)$
* `pq.clear()`: se sterg toate elementele din 'pq'. $O(N)$
# Set
```cpp
#include <set>
```
## functionalitati:
* set-ul contine elementele in ordine crescatoare
* set-ul nu contine elemente duplicate
* folositor cand dorim sa tinem elementele in ordine si sa stergem elementele duplicate 
* **momentan** nu putem implemanta de mana, deoarece, intern, set-ul este implementat prin **red-black tree**, structura de date despre care nu am vorbit
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/4552/set-map#intlink-0)
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/set-in-cpp-stl/)
## functii:
* `st.insert(nr)`: se adauga 'nr' la set. $O(logN)$
* `st.erase()`
    * `st.erase(nr)`: se sterge elementul cu valoare 'nr' (daca exista). $O(logN)$
    * `st.erase(it)`: se sterge elementul cu iteratorul 'it'. $O(1)$
* `st.count(nr)`: daca exista 'nr' in set se returneaza 1, altfel 0. $O(logN)$
* `st.find(nr)`: se returneaza iteratorul elementului cu valoarea 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `st.lower_bound(nr)`: se returneaza iteratorul primului elementul cu valoarea mai mare sau egala cu valoarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `st.upper_bound(nr)`: se returneaza iteratorul primului elementul cu valoarea mai mare decat valoarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `swap(st1, st2)`: se interschimba 'st1' cu 'st2'. $O(1)$
* `st.clear()`: se sterg toate elementele din 'st'. $O(N)$
# Multiset
```cpp
#include <set> // nu multiset
```
## functionalitati:
* multiset-ul contine elementele in ordine crescatoare
* multiset-ul poate contine elemente duplicate
* folositor cand dorim sa tinem elementele in ordine
* **momentan** nu putem implemanta de mana, deoarece, intern, multiset-ul este implementat prin **red-black tree**, structura de date despre care nu am vorbit
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/multiset-in-cpp-stl/)
## functii:
* `st.insert(nr)`: se adauga 'nr' la multiset. $O(logN)$
* `st.erase()`
    * `st.erase(nr)`: se sterg toate elementele cu valoarea egala cu valoarea lui 'nr' (daca exista). $O(logN+Nr\_elem\_sterse)$
    * `st.erase(it)`: se sterge elementul cu iteratorul 'it'. $O(1)$
* `st.count(nr)`: se returneaza nr de elemente cu valoarea egala cu valoare lui 'nr'. $O(logN+Nr\_elem\_egale\_cu\_nr)$
* `st.find(nr)`: se returneaza iteratorul unui element cu valoarea egala cu vaoloarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `st.lower_bound(nr)`: se returneaza iteratorul primului elementul cu valoarea mai mare sau egala cu valoarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `st.upper_bound(nr)`: se returneaza iteratorul primului elementul cu valoarea mai mare decat valoarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `swap(st1, st2)`: se interschimba 'st1' cu 'st2'. $O(1)$
* `st.clear()`: se sterg toate elementele din 'st'. $O(N)$
# Unordered Set
```cpp
#include <unordered_set>
```
## functionalitati:
* unordered_set-ul nu contine elementele in ordine crescatoare
* unordered_set-ul nu contine elemente duplicate
* folositor cand dorim sa stergem elementele duplicate 
* **momentan** nu putem implemanta de mana, deoarece, intern, unordered_set-ul este implementat printr-un **hash map**, structura de date despre care nu am vorbit
* [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/unordered_set-in-cpp-stl/)
## functii:
* `st.insert(nr)`: se adauga 'nr' la set. $O(1)$
* `st.erase()`
    * `st.erase(nr)`: se sterge elementul cu valoarea egala cu valoarea lui 'nr' (daca exista). $O(1)$
    * `st.erase(it)`: se sterge elementul cu iteratorul 'it'. $O(1)$
* `st.count(nr)`: daca exista 'nr' in set se returneaza 1, altfel 0. $O(1)$
* `st.find(nr)`: se returneaza iteratorul elementului cu valoarea lui 'nr'. Daca acesta nu exista se returneaza `st.end()`. $O(1)$
* nu mai exista upper_bound() / lower_bound()
* `swap(st1, st2)`: se interschimba 'st1' cu 'st2'. $O(1)$
* `st.clear()`: se sterg toate elementele din 'st'. $O(N)$
# Map
```cpp
#include <map>
```
## functionalitati:
* ne permite sa ne tinem perechi de genul $(cheie, valoare)$.
* perechile sunt sortate dupa cheie
* cheile trebuie sa fie diferite intre ele
* putem sa modificam cand vrem valoarea elementului atat timp cat ii stim cheia prin sintaxa `mp[cheie]=val`. Daca pana atunci nu exista niciun element cu cheia respectiva, atunci se va crea unul nou si valoarea lui va fi 'val'.
* Un exemplu de utilizare este reprezentat de un vector de frecventa, care transpus in map ar contine perechi de genul $(val, Nr\_elem\_cu\_valoarea\_val)$. De fiecare data cand intalnim un $numar$ adaugam valorii elementului cu $cheia=numar$ 1.
* **momentan** nu putem implemanta de mana, deoarece, intern, map-ul este implementat prin **red-black tree**, structura de date despre care nu am vorbit
* [Materialul Pbinfo 1](https://www.pbinfo.ro/articole/4552/set-map#intlink-7)
* [Materialul Pbinfo 2](https://www.pbinfo.ro/articole/27941/map-data-structure-hash-table)
* [Materialul Geeks for Geek](https://www.geeksforgeeks.org/map-associative-containers-the-c-standard-template-library-stl/)
## functii:
* `st.insert(cheie, valoare)`: se adauga pereceha $(cheie, valoare)$ la map. $O(logN)$
* `st.erase()`
    * `st.erase(cheie)`: se sterge elementul cu cheia egala cu 'cheie' (daca exista). $O(logN)$
    * `st.erase(it)`: se sterge elementul cu iteratorul 'it'. $O(1)$
* `st.count(cheie)`: daca exista cheia 'cheie' in map se returneaza 1, altfel 0. $O(logN)$
* `st.find(cheie)`: se returneaza iteratorul elementului cu cheiea 'cheie'. Daca acesta nu exista se returneaza `st.end()`. $O(logN)$
* `st.lower_bound(nr)`: se returneaza iteratorul primului elementul cu cheia mai mare sau egala cu 'cheie'. Daca acesta nu exista, se returneaza `st.end()`. $O(logN)$
* `st.upper_bound(nr)`: se returneaza iteratorul primului elementul cu cheia mai mare decat 'cheie'. Daca acesta nu exista, se returneaza `st.end()`. $O(logN)$
* `swap(mp1, mp2)`: se interschimba 'mp1' cu 'mp2'. $O(1)$
* `mp.clear()`: se sterg toate elementele din 'mp'. $O(N)$
# Unoredered Map
```cpp
#include <unordered_map>
```
## functionalitati:
* ne permite sa ne tinem perechi de genul $(cheie, valoare)$.
* perechile nu sunt sortate dupa cheie
* cheile trebuie sa fie diferite intre ele
* putem sa modificam cand vrem valoarea elementului atat timp cat ii stim cheia prin sintaxa `mp[cheie]=val`. Daca pana atunci nu exista niciun element cu cheia respectiva, atunci se va crea unul nou si valoarea lui va fi 'val'.
* Un exemplu de utilizare este reprezentat de un vector de frecventa, care transpus in map ar contine perechi de genul $(val, Nr\_elem\_cu\_valoarea\_val)$. De fiecare data cand intalnim un $numar$ adaugam valorii elementului cu $cheia=numar$ 1.
* **momentan** nu putem implemanta de mana, deoarece, intern, unordered_map-ul este implementat printr-un **hash map**, structura de date despre care nu am vorbit
* [Materialul Geeks for Geek](https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/)
## functii:
* `st.insert(cheie, valoare)`: se adauga pereceha $(cheie, valoare)$ la map. $O(1)$
* `st.erase()`
    * `st.erase(cheie)`: se sterge elementul cu cheia egala cu 'cheie' (daca exista). $O(1)$
    * `st.erase(it)`: se sterge elementul cu iteratorul 'it'. $O(1)$
* `st.count(cheie)`: daca exista cheia 'cheie' in unordered_map se returneaza 1, altfel 0. $O(1)$
* `st.find(cheie)`: se returneaza iteratorul elementului cu cheiea 'cheie'. Daca acesta nu exista se returneaza `st.end()`. $O(1)$
* nu exista functiile lower_bound / upper_bound
* `swap(mp1, mp2)`: se interschimba 'mp1' cu 'mp2'. $O(1)$
* `mp.clear()`: se sterg toate elementele din 'mp'. $O(N)$
# Bitset
```cpp
#include <bitset>
```
## functionalitati:
* fata de un vector de bool, un bitset are de 8 ori mai putina memorie (N biti fata de N bytes).
* bitul k se poate accesa prin [ ]: `bset[k]`
* permite operatii pe biti, de cele mai multe ori in complexitate $O(N/64)$
* este destul de greu de implementat de mana, cerand materie ce trece de nivelul acestui cerc
* [Materialul Pbinfo](https://www.pbinfo.ro/articole/23872/cpp-stl-bitset)
* [Materialul Geeks for Geek](https://www.geeksforgeeks.org/cpp-bitset-and-its-application/)
## functii:
* `bset.all()`: returneaza 1 daca toti bitii sunt egali cu 1, alftel returneaza 0. $O(1)$
* `bset.any()`: returneaza 1 daca macar un bit este egal cu 1, altfel returneaza 0. $O(1)$
* `bset.none()`: returneaza 1 daca niciun bit nu este egal cu 1, alftel returneaza 0. $O(1)$
* `bset.count()`: returneaza numarul de biti egali cu 1. $O(N/64)$
* `bset.size()`: returneaza numarul de biti. $O(1)$
* aparent nu exista swap
# Tema
## uitati-va peset containerele:
* pbds(policy based data structures)
    * [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/policy-based-data-structures-g/)
    * [Materialul CodeForces](https://codeforces.com/blog/entry/11080)
    * [Materialul Medium](https://medium.com/nybles/policy-based-data-structure-491ac62ddeaa)
* rope 
    * [Materialul Geeks for Geeks](https://www.geeksforgeeks.org/stl-ropes-in-c/)
