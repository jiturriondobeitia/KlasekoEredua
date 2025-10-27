# Klasean Git erabiltzeko oinarrizko jarraibideak

Klasean Git erabiliz lan egiteko oinarrizko pausoak:

## 1. Repositorioa klonatu
Lehenik eta behin, zure ordenagailura klaseko ereduaren **biltegia jaitsi**:
```bash
git clone https://github.com/jiturriondobeitia/KlasekoEredua.git
```

Jarraian lanean arituko gara: Fitxategietan aldaketak egin, fitxategi berriak gehitu edo ezabatu.

## 2. Aldaketak egiaztatu
Biltegi barruan kokatuta gaudela lokalean eta Github-eko zerbitzariaren arteko **aldaketak ikusi**:
```bash
git status
```

## 3. Fitxategiak gehitu
Aldaketak dituztenen fitxategien artean ,zerbitzarian eguneratu nahi diren **fitxategiak zehaztu**:
```bash
git add fitxategia1 fitxategia2 fitxategiaN
# Edo aldaketa guztiak gehitzeko:
git add .
```

## 4. Commit bat sortu
*Commit* bat egin aldaketen deskribapen labur batekin batera, aurrekoan gehitutako fitxategiak **zerbitzarira igo**tzeko:
```bash
git commit -m "Nire lehenengo commit-a"
```

## 5. Git identitatea konfiguratu
Biltegira lehenengo aldiz *commit*-a egiten bada, erabiltzailearen izena eta posta elektronikoa ezarri behar dira:
```bash
git config --global user.email "jiturriondobeitia@fpsanturtzilh.eus"
git config --global user.name "Jon Iturri"
```

## 6. Commit-ak zerbitzarira igo
Azkenik, *commit*-ak GitHub-era igo:
```bash
git push
```

> ### Adibidea  
> Imaginatu gure biltegian hiru motatako aldaketak ditugula:
>
> | Egoera          | Fitxategia      | Zer egin                   | Git komandoa                                |
> |-----------------|-----------------|----------------------------|---------------------------------------------|
> | Berria          | berria.txt      | Commit-ean gehitu          | `git add berria.txt`                        |
> | Aldatua         | aldatua.txt     | Aldaketak prestatu         | `git add aldatua.txt`                       |
> | Ezabatua        | ezabatua.txt    | Ezabatzea berretsi         | `git rm ezabatua.txt`                      |
>
> **`git status` komandoak horrelako zehozer erakutsiko zuen:**
> ```bash
> git status
>
> # Zer adarretan ari garen lanean erakusten du ("main" adar lehenetsia da)
> On branch main
> # Commit-eatu gabeko aldaketak
> Changes not staged for commit:
>   modified:   aldatua.txt
>   deleted:    ezabatua.txt
> # Fitxategi berriak
> Untracked files:
>   new file:   berria.txt
>
> # Aldaketa guztiak prestatu commit-a egiteko
> git add berria.txt aldatua.txt
> git rm ezabatua.txt
> # Commit-a egin horren aldaketek egiten duten deskripzioarekin batera
> git commit -m "berria.txt fitxategia gehituta, aldatuta.txt fitxategia aldatuta eta ezabatua.txt fitxategia ezabatua"
> # Commit-aren aldaketak GitHub-era igo
> git push
> ```

> **OHARRA:** `Push` egin baino lehen ziurtatu behar da biltegi lokala urrutiko biltegiaren azken bertsioarekin bat egiten duela, hau da, biltegi lokala eguneratuta dagoela.

---

# Zerbitzaritik fitxategiak eguneratzea

Klasean lanean gabiltzanean, baliteke beste ikasle edo irakasle batzuek biltegian aldaketak egitea.  
Horregatik, **zure biltegia eguneratu** behar da aldian behin, azken bertsioa edukitzeko.

## 1. Zerbitzariko aldaketak ikusi
Lehenik, egiaztatu ea zure biltegia **zaharkituta** dagoen edo **berria** den:

`fetch` komandoak GitHub-etik azken informazioa ekarriko du, baina ez du fitxategirik aldatuko zure ordenagailuan.
```bash
git fetch
```
Ondoren, konparatu zure egoera:
```bash
git status
```
Git-ek adieraziko dizu ea zure branch-a (adibidez, main) atzeratuta dagoen (behind) edo eguneratuta (up to date).

| Egoera (Git-en mezua) | Adiera                                                                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| `up to date`          | Zure biltegia **eguneratuta** dago, ez dago aldaketarik deskargatzeko.                                    |
| `behind`              | Zure biltegia **atzera** dago; beste norbaitek aldaketak igo ditu eta **jaitsi** behar dituzu.            |
| `ahead`               | Zure biltegia **aurreratuta** dago; zuk aldaketak egin dituzu baina **oraindik ez dituzu igo**.           |
| `diverged`            | Zure eta zerbitzariaren artean **desberdintasunak** daude; bateratu egin behar da (`merge` edo `rebase`). |

## 2. Aldaketak deskargatu eta aplikatu

Zerbitzariko aldaketak zure ordenagailura ekartzeko eta aplikatzeko:
```bash
git pull
```

Komando honek bi gauza egiten ditu:
- Aldaketak deskargatu: `fetch`
- Eta automatikoki bateratu zure fitxategiekin: `merge`

---

# Biltegien arteko sinkronizazioa

`git push` egiten bada, baina urruneko adarrean lokalean ez dituzun commit-ak badaude, Git-ek zure push eragiketa baztertuko du zure aldaketek zuk ez dituzun urruneko horiek ez gainidazteko. 

Git-ek ez du urruneko historia gainidazten uzten urruneko historiaren atzean zaudela detektatzen badu (hau da, zuk ez dituzun commit-ak badaude). 

Horrelako mezu bat ikusiko zenuke:
```bash
error: failed to push some refs to 'https://github.com/jiturriondobeitia/KlasekoEredua.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

**Nola konpontzen da?**

`Push` egin aurretik, urrutiko aldaketak zure adar lokalean integratzen.

Imajinatu adar batean lan egiten ari duzula:
- Urruneko adarrean zuk lokalean ez duzun commit berri bat dago (*C*).
- Lokalean berriz, zuk ere urruneko adarrean ez dagoen commit berri bat egin duzu (*D*). 

```bash
Urrunean: A --- B --- C
Lokalean: A --- B --- D
```

Zure *D* commit-a *B* commit-ean oinarrituta dago, baina urrunekoak *C* dauka.

`git push` egiten bada, huts egingo du urrunekoak *C* duelako eta zuk ez. 

### 1. aukera

`git pull` egiterakoan bi gauza egiten dira:
1. `git fetch`: Urruneko aldaketak deskargatzen dira.
2. `git merge`: Aldaketa horiek zure adarrean fusionatzen dira.
    - **Merge commit** (*M*) berri bat sortzen du, zure aldaketak urruneko aldaketekin lotzen dituenak.

```bash
A --- B --- C --- D
            \     /
             \---M  ← merge commit
```
Eragiketa honekin historikoa **merge commit**-ez betetzen da, eta horrek irakurketa zaildu dezake. 

### 2. aukera

`git pull --rebase` egiterakoan bi gauza egiten dira:
1. `git fetch`: Urruneko aldaketak deskargatzen dira.
2. `git rebase`: Zure commit-ak urrunekoen gainean **birkokatzen** dira.
    - *C* commit-a deskargatzen da.
    - Tenporalki zure *D* commit-a kentzen da.
    - *C* commit-a aplikatzen da.
    - *D* commit-a aplikatzen du berriro baina *C* commit-aren gainean.

```bash
A --- B --- C --- D
```
Horrela beti historiala lineala izango da. 

| Komandoa            | Zer egiten du?     | Emaitza                        |
| ------------------- | -------------------| ------------------------------ |
| `git pull`          | `fetch` + `merge`  | Historiala *merge commit*-ekin |
| `git pull --rebase` | `fetch` + `rebase` | Historiala lineala eta garbia  |

Behin biltegi lokala urruneko zerbitzariarekin sinkronizatuta dagoenean, `git push` erabili daiteke gure aldaketak urrunera igotzeko. 

---

# Gatazkak Git-en

Bi pertsonak artxibo beraren zati bera aldatzen dutenean *gatazka* bat gertatzen, Git-ek ez bait daki zein bertsio gorde `merge` edo `rebase` bat egitean.

> Imajinatu aurreko adibidean *C* eta *D* commit-ek fitxategi bera aldatzen dutela.
> 
> ```text
> |   index.html (Commit C)  |   index.html (Commit D)  |
> |--------------------------|--------------------------|
> | <h1>                     | <h1>                     |
> | Hola mundo               | Hola clase               |
> | </h1>                    | </h1>                    |
> ```
> 

## 1. Nola ikusten da gatazka bat?

Git-ek gatazkaren bat aurkitzen badu fitxategi batean, marka bereziak uzten ditu artxibo horretan: 

```text
<h1>
<<<<<<< HEAD
Hola mundo
=======
Hola clase
>>>>>>> feature
</h1>
```

Lerro hauek adierazten dutena:

```text
<<<<<<< HEAD → zure bertsio lokalaren kodea
======= → bi bertsioen arteko bereizketa
>>>>>>> feature → urruneko bertsioaren kodea
```

## 2. Nola konpondu gatazka bat

1. Gatazka duen fitxategia ireki. Git-ek adieraziko du `git status` egiterakoan:
```bash
both modified: index.html
```

2. Fitxategia eskuz editatu, mantendu nahi den kodea aukeratuz:
```text
<h1>Hola 'mundo' edo 'clase'</h1>
```

3. Gatazka konpondutzat markatzea: 
```bash
git add index.html
```

4. Eragiketa amaitzea: 
- `merge` eragiketa batean bazeunden:
> ```bash
> git commit
> ```
- `rebase` eragiketa batean bazeunden:
> ```bash
> git rebase --continue
> ```

Behin gatazka lokalean konpondu dugula, `git push` erabili daiteke gure aldaketak urrunera igotzeko.

---

# Aldaketen historiko lerroa

Github-en grafiko baten bidez era bisual eta eroso batean jarraitu daiteke biltegian egindako `commit`-en [historiko](https://github.com/jiturriondobeitia/KlasekoEredua/network) osoa.