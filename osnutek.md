# PR18MLKL - AirBnB London
Projektna naloga za predmet Podatkovno Rudarjenje 2018 - Matic Lahajnar, Klemen Levstik

Obdelovala bova podatke o stanovanjih za mesto London, katera so objavljena na AirBnB. Zanima naju, kako se cena dviga / spušča glede na regijo mesta, tip stanovanja. AirBnB trdi, da 70% strank, ki stanuje v nekem stanovanju, pusti tudi oceno, tako da bova poskušala na nek način obdelati tudi obisk glede na tip, lokacijo, itd. stanovanja.

Podatke sva dobila s strani tomslee.net. Podatki so pridobljeni programsko s spletne strani na nek določen datum.
Vsebujejo naslednje atribute:
- room_id: unikaten id, ki predstavlja neko določeno stanovanje / sobo
- host_id: unikaten id, ki predstavlja gostitelja oz. lastnika stanovanja
- room_type: diskretni atribut, katerega možnosti so:
  - Entire home/apt
  - Private room
  - Shared room
- borough: Območje mesta
- neighborhood: Manjše območje mesta
- reviews: Število ocen stanovanja
- overall_satisfaction: Povprečna ocena stanovanja
- accommodates: Število gostov, ki lahko bivajo v stanovanju
- bedrooms: Število sob v stanovanju
- price: Cena (v $) na noč. V zgodnjih podatkih je cena možna za cel mesec
- minstay: Najmanjše število nočitev
- lattitude in longitude: Lokacija stanovanja v koordinatih (lahko odstopa za par 100m)
- last_modified: Datum in ura branja objave na AirBnB



# Vmesno poročilo


## 1. Opis problema
Odločila sva se, da bova obdelovala podatke o stanovanjih v Londonu, ti pa so objavljeni na spletni strani tomslee.net/airbnb. Zanima naju, kako se cena dviga / spušča glede na regijo mesta, tip stanovanja ipd. AirBnB trdi, da 70 % strank, ki stanuje v nekem stanovanju, pusti tudi oceno, tako da bova poskušala na nek način obdelati tudi obisk glede na tip, lokacijo, itd. stanovanja. 


## 2. Podatki
Za London sva se odločila zato, ker je mesto veliko in ima temu primerno tudi število stanovanj ter temu primerno veliko število podatkov. Podatki so bili zabeleženi v razponu 4 let, od leta 2013 do leta 2017. Strukturirani so v datoteke, kjer so podatki oz. atributi ločeni z vejico (.csv). Slednji so opisani spodaj:
- room_id: unikaten id, ki predstavlja neko določeno stanovanje / sobo
- host_id: unikaten id, ki predstavlja gostitelja oz. lastnika stanovanja
- room_type: diskretni atribut, katerega možnosti so:
  - Entire home/apt
  - Private room
  - Shared room
- borough: Območje mesta
- neighborhood: Manjše območje mesta
- reviews: Število ocen stanovanja
- overall_satisfaction: Povprečna ocena stanovanja
- accommodates: Število gostov, ki lahko bivajo v stanovanju
- bedrooms: Število sob v stanovanju
- price: Cena (v $) na noč. V zgodnjih podatkih je cena možna za cel mesec
- minstay: Najmanjše število nočitev
- lattitude in longitude: Lokacija stanovanja v koordinatih (lahko odstopa za par 100m)
- last_modified: Datum in ura branja objave na AirBnB


## 3. Glavne ugotovitve
Ugotovila sva, da cena stanovanj v Londonu skozi leta počasi pada, razlika pa je kar drastična, saj je povprečna cena stanovanj v razponu 3 let padla za prek 60 USD.
Očitno je tudi, da so dražja stanovanja ponavadi »boljša«, torej se s ceno stanovanja poveča tudi udobje, posledično pa stranka po navadi pusti višjo oceno.


## 4. Zanimivejši rezultati
Spodnji graf, skupaj s priloženo izvorno kodo prikazuje spremembo povprečne cene stanovanj v Londonu v obdobju 4 let. Razvidno je, da cena sunkovito pada. Manjši krivec zato so morda tudi podatki, saj datoteke iz zgodnejših let, ki te vsebujejo, niso povsem konsistentne. Nekatere cene (teh je po podatkih AirBnB malo) so namreč navedene za cel mesec, kar malenkostno zviša povprečno ceno, predvsem za leto 2013.


```python
from numpy import *
import operator
import matplotlib.image as mpimg
import matplotlib.pyplot as plt
from csv import DictReader
from os import listdir

overallSatisfactionGlobal = []
priceGlobal = []


def readData(filePath):
    reader = DictReader(open('files/' + filePath, 'rt', encoding='utf-8'))
    price = []
    overallSatisfaction = []

    for row in reader:
        try:
            if row["overall_satisfaction"] == "" or row["overall_satisfaction"] == "0.0":
                raise Exception()

            price.append(float(row["price"]))
            overallSatisfaction.append(row["overall_satisfaction"])
            overallSatisfactionGlobal.append(row["overall_satisfaction"])
            priceGlobal.append(float(row["price"]))

        except:
            pass

    return sum(price) / len(price)


def avgYears():
    years = {}

    years["2013"] = readData(listdir("files/")[0])
    years["2014"] = readData(listdir("files/")[1])
    years["2015"] = (readData(listdir("files/")[2]) +
                     readData(listdir("files/")[3])) / 2
    years["2016"] = sum([readData(listdir("files/")[i]) for i in range(4, 9)]) / 5
    years["2017"] = sum([readData(listdir("files/")[i]) for i in range(9, 16)]) / 7

    return years


avgYears = avgYears()

plt.figure()
plt.plot(['2013', '2014', '2015', '2016', '2017'],
         [avgYears['2013'], avgYears['2014'], avgYears['2015'], avgYears['2016'], avgYears['2017']], 'r')
plt.title('Povprečna cena stanovanja v Londonu po letih')
plt.xlabel('leto')
plt.ylabel('cena stanovanja v USD')

plt.figure()
plt.plot(overallSatisfactionGlobal, priceGlobal, 'k.')
plt.title('Zadovoljstvo strank glede na ceno stanovanja')
plt.xlabel('ocena')
plt.ylabel('cena stanovanja v USD')
```

![alt text](https://github.com/maticlahajnar/PR18MLKL/blob/master/Graf1.png)

Spodnji graf je na prvi pogled precej nenavaden, a z malo razmisleka lahko vidimo, da se frekvenca ocen stanovanj relativno povečuje z njegovo ceno, kar gre pričakovati.

![alt text](https://github.com/maticlahajnar/PR18MLKL/blob/master/Graf2.png)
