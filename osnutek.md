# PR18MLKL - AirBnB London
Projektna naloga za predmet Podatkovno Rudarjenje 2018 - Matic Lahajnar, Klemen Levstik

Obdelovala bova podatke o stanovanjih za mesto London, katera so objavljena na AirBnB. Zanima naju, kako se cena dviga / spušča glede na regijo mesta, tip stanovanja. AirBnB trdi, da 70% strank, ki stanuje v nekem stanovanju, pusti tudi oceno, tako da bova poskušala na nek način obdelati tudi obisk glede na tip, lokacijo, itd. stanovanja.

Podatke sva dobila s strani tomslee.net. Podatki so pridobljeni programsko s spletne strani na nek določen datum.
Vsebujejo naslednje atribute:
  -room_id: unikaten id, ki predstavlja neko določeno stanovanje / sobo
  -host_id: unikaten id, ki predstavlja gostitelja oz. lastnika stanovanja
  -room_type: diskretni atribut, katerega možnosti so:
    -Entire home/apt
    -Private room
    -Shared room
  -borough: Območje mesta
  -neighborhood: Manjše območje mesta
  -reviews: Število ocen stanovanja
  -overall_satisfaction: Povprečna ocena stanovanja
  -accommodates: Število gostov, ki lahko bivajo v stanovanju
  -bedrooms: Število sob v stanovanju
  -price: Cena (v $) na noč. V zgodnjih podatkih je cena možna za cel mesec
  -minstay: Najmanjše število nočitev
  -lattitude in longitude: Lokacija stanovanja v koordinatih (lahko odstopa za par 100m)
  -last_modified: Datum in ura branja objave na AirBnB
