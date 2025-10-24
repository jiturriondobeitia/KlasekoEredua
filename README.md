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
git add *fitxategia1* *fitxategia2*
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
> # Commit-a egin horren aldaketaren deskripzioarekin batera
> git commit -m "berria.txt fitxategia gehituta, aldatuta.txt fitxategia aldatuta eta ezabatua.txt fitxategia ezabatua"
> # Commit-aren aldaketak GitHub-era igo
> git push
> ```

> **OHARRA:** `Push` egin baino lehen ziurtatu behar da biltegi lokala urrutiko biltegiaren azken bertsioarekin bat egiten duela, hau da, biltegi lokala eguneratuta dagoela.

# 🔄 Zerbitzaritik fitxategiak eguneratzea

Klasean lanean gabiltzanean, baliteke beste ikasle edo irakasle batzuek biltegian aldaketak egitea.  
Horregatik, **zure biltegia eguneratu** behar da aldian behin, azken bertsioa edukitzeko.

---

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
- Aldaketak deskargatu (fetch)
- Eta automatikoki bateratu zure fitxategiekin (merge)

Si haces git push pero en la rama remota hay commits que tú no tienes en tu rama local, Git rechazará el push por defecto para evitar que sobrescribas esos cambios remotos.

Git no permite sobrescribir el historial remoto si detecta que estás detrás del remoto (es decir, hay commits que tú no tienes).

Verás un mensaje como este:
```bash



```

Como se soluciona? Integrar los cambios antes de hacer push

Imagina que trabajas en la rama main:

En remoto (origin/main) hay un commit nuevo que tú no tienes.

En local, tú también hiciste un commit nuevo.

```bash
Remoto: A --- B --- C
Local:  A --- B --- D
```

Tu commit D está basado en B, pero el remoto tiene C.
Si haces git push, fallará porque el remoto tiene C y tú no.

### Opción 1

Cuando haces `git pull` haces dos cosas:
1. `git fetch` (descarga los cambios remotos)
2. `git merge` (fusiona esos cambios en tu rama)
    1. Crea un nuevo commit de merge (M) que une tus cambios con los remotos

```bash
A --- B --- C --- D
           \       /
             \--- M  ← merge commit
```
El historial se llena de commits de merge, lo que puede complicar la lectura.

### Opción 2

Cuando haces `git pull --rebase` haces dos cosas:
1. `git fetch` (descarga los cambios remotos)
2. `git rebase` (recoloca tus commits sobre los del remoto)
    1. Descarga el commit C.
    2. Quita temporalmente tu commit D.
    3. Aplica C.
    4. Vuelve a aplicar D encima de C.

```bash
A --- B --- C --- D
```
Así siempre tendrás un historial lineal.

| Comando             | Qué hace                             | Resultado                  |
| ------------------- | ------------------------------------ | -------------------------- |
| `git pull`          | Hace `fetch` + `merge`               | Historial con merge commit |
| `git pull --rebase` | Hace `fetch` + rebase de tus commits | Historial lineal y limpio  |


## 3. Aldaketen historikoa

Biltegian egindako `commit`-en [historikoa](https://github.com/jiturriondobeitia/KlasekoEredua/network) 