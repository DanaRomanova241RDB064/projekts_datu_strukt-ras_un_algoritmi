## projekts_datu_strukt-ras_un_algoritmi
# Programmatūra, kas ļauj viegli pārskatīt studentu vidējos vērtējumus katrā studiju kursā
## Projekta apraksts un tā galvenie uzdevumi


  Universitātē katram studiju kursam ir savi vērtēšanas kritēriji un prasības, kā tiek veidots gala vērtējums, kā arī savi pārbaudījumu veidi, piemēram dažos kursos daļu no gala vērtējumu sastāda labaratorijas darbi, bet citos tāda pārbaudījuma veida nemaz nav.Turklāt šie pārbaudījumu veidi katra kursa gala vērtējumu, procentuāli, sastāda atšķirīgi, tādēļ studentiem aprēķināt savu vidējo vērtējumu ir diezgan sarežģīti un laikietilpīgi.


  Projekta galvenā doma ir atvieglot studentiem vērtējumu pārskatīšanu visiem studiju kursiem vienā failā, jo ORTUS(ā) var redzēt tikai atsevišķus vērtējumus katram studiju kursam un ja kursi ir vairāki, semestra beigās var rasties lieks stress un nelietderīgi atņemts laiks, vidējā gala vērtējuma rēķināšanā. 


  Lai atvieglotu šo procesu, programmatūra palīdz neveidot manuāli jaunus excel failus katram semestrim vai vēl sliktākā gadījumā - rēķināt visu ar kalkulatoru un katru reizi skatīties cik procentus ietekmē kāds pārbaudījumu veids konkrētajā kursā.
  Studenti var ātri izveidot jaunu excel failu, kurā jau ir izveidota tabula ar visbiežākajiem pārbaudes veidiem(ja ir vajadzība vēl kādu pievienot, ar nelielām izmaiņām programmatūrā tās var pievienot),pievienot jaunus studiju kursus, ievadīt un saglabāt vidējos vērtējumus, aprēķināt gala vērtējumus, ievadot katra pārbaudījuma veida īpatsvaru gala vērtējuma sastādīšanā, jo katrā studiju kursā tie ir savādāki, kā arī ja students ir labojis kādu pēdējo pārbaudes darbu, tad var izlabot pēdējo vērtējumu katram no pārbaudījumu veidiem.

  
  Tādējādi, tā var ietaupīt katram laiku un pacietību, visa semestra laikā, studentiem vajag tikai dažas sekundes, lai pievienotu tikko uzzināto jauno vērtējumu un savā veidā kontrolētu savus gala vērtējumus, analizējot kurus vērtējumus varētu "pavilkt uz augšu", ar pēdējiem darbiem, lai nebūtu tā, ka pietrūkst tikai dažu punktu līdz sekmīgam gala vērtējumam.
  

## Python bibliotēkas un to izmantošana
Programmatūras izstrādē lietotas tikai 2 bibliotēkas:
### openpyxl:
  Šī bibliotēka ļauj rakstīt un lasīt excel failus.

  
* Workbook un load_workbook: Kad pirmo reizi tiek iedarbināts kods, funkcija faila_izveide() izmanto Workbook() lai radītu jaunu dokumentu ar norādītajām tabulām un ja fails jau eksistē, load_workbook('vertejumi.xlsx') atver to, nevis pārraksta.

  
* Darblapas (Worksheet):
_wb.active_ dod piekļuvi aktīvajai, jeb pirmai excel lapai, kur glabājas kursu nosaukumi un vidējie vērtējumi.


* _wb.create_sheet('Vērtējumi')_ izveido otro lapu, kur tiek saglabātas visas atsevišķās vērtības(vērtējumi), lai pie atkārtotas programmas palaišanas tie nepazustu, jo sākumā tie tiek pievienoti sarakstiem, kas paliek tukši programmas beigās, tādēļ tas palīdz aprēķināt kopējo vidējo vērtējumu, ja arī programma tiek palaista vairākas reizes.


* Darbības ar excel šūnām:
_ws.cell(row, column, value=...)_ - ievieto vai nolasa konkrētas šūnas saturu. Piemēram, kad vajag aprēķināt vidējo kontroldarbu atzīmi, _ws.cell(row=rinda, column=2, value=vid_vert)_ ieraksta to Excel konkrētā šūnā.
_ws.append([...])_ pievieno jaunu rindu.


* _cell.number_format = '0.00'_ formatē excel tabulās redzamos vērtējumus ar 2 cipariem aiz komata


### os:
Šī bibliotēka ir paredzēta, lai ātri pārbaudītu failus pirms kādas darbības, tieši šajā projektā _os.path.exists(path)_ funkcijā 

_faila_izveide()_ sākumā pārbauda, vai vertejumi.xlsx jau ir mapē, ja nav, tad izveido jaunu failu ar šo nosaukumu, pretējā gadījumā failu vienkārši atver un turpina visas darbības ar to.

### Datu struktūras un to izskaidrojums
Projektā galvenokārt ir trīs veidu Python datu struktūras, lai sakārtotu un apstrādātu studentu vērtējumus:
* Vārdnīcas _kd, ld, md, teo, eks = {},{},{},{},{}_
Katra no šīm vārdnīcām glabā atsevišķus vērtējumu sarakstus un katras aclēgas vērtība ir studiju kursa nosaukums.
Piemēram, kad lietotājs pievieno jaunu kontroldarbu vērtējumu - 6 un 7, studiju kursā matemātika, tad - _kd{'matemātika':[6,7],...}_


* Vārdnīca _varianti={1:(kd,2,"kontroldarba"),2:(ld,3,"labaratorijas darba"),3:(md,4,"mājasdarba"),4:(teo,5,"teorijas testa"),5:(eks,6,"eksāmena")}_
  Atslēgvērtības (1–5) sasaista trīs vērtības, kas satur pārbaudes veida sarakstus, excel kolonnas indeksu un vērtējuma veida nosaukumu.
Piemērs: _varianti[2] == (ld, 3, "laboratorijas darba")_, ja lietotājs izvēlas “2) Laboratorijas darbs”, tad programma izmanto sarakstu ld, raksta vidējo vērtējumu 3. kolonnā un pie izvades, kur vajag norādīt pārbaides veidu, izvada -  "laboratorijas darba".


* "Tuple" saraksts _vert_veidi = [("Kontroldarbi", 2), ("Labaratorijas darbi", 3), ("Mājasdarbi", 4),("Teorijas testi", 5),("Eksāmens", 6)]_
vert_veidi ir saraksts ar tuple vērtībām, kas satur vērtējuma veida nosaukumu un atbilstošo excel kolonnas indeksu. Šī datu struktūra tiek izmantota gala vērtējuma aprēķināšanai, kad nepieciešams iterēt _for veids, kol in vert_veidi:_, iegūstot katra veida vidējo vērtējumu no šūnas un reizināt ar lietotāja ievadīto vērtējuma veida īpatsvaru, lai aprēķinātu galīgo vidējo vērtējumu studiju kursā.

Izmantotās datu struktūras palīdz ātri un ērti pievienot un aprēķināt vērtējumus, jo kodā nevajag pārrakstīt liekas rindiņas katram vērtējuma veidam un, ja būs nepieciešamība pieveinot papildus vērtējuma veidu, tad tas būs izdarāms daudz ātrāk.

### Definētās funkcijas un to darbība
1. _int_kludas_apstrade(izvades_teksts)_:
   
Nolasa lietotāja ievadi un pārbauda vai ievadīts vesels skaitlis, ja rodas kļūda, paziņo par to un atgriežas pie lietotāja ievades, nepārtraucot programmas darbību.

2. _float_kludas_apstrade(izvades_teksts)_:

Nolasa lietotāja ievadi, ja ir nepieciešamība, pārvērš komatus par punktiem, lai nerastos kļūdas un ievadītais vērtējums būtu float tipa(peldošais punkts) un līdzīgi kā ar int_kludas_apstrade, atgriežas pie lietotāja ievades.

3. _faila_izveide(path="vertejumi.xlsx")_:

Pārbauda ar _os.path.exists()_, vai fails eksistē, ja neeksistē, izveido jaunu _Workbook()_, pievieno galvenes un darba lapas.

4. _nolasit_vertejumus(wb)_:

Nolasa visus atsevišķos vērtējumus no lapas "Vērtējumi" un sadala tos atbilstošajos sarakstos (kd, ld, md, teo, eks), lai turpmāk varētu aprēķināt vidējos vērtējumus katram studiju kursam un pārbaudījuma veidam atsevišķi.

5. _pievienot_studiju_kursus(ws)_:

Pajautā lietotājam, cik kursus viņš vēlas pievienot, un ar _ws.append([])_ pievieno katru jauno kursu galvenajā lapā, pirmajā kolonnā.

6. _vertejuma_veida_izvele()_:

Parāda izvēlni ar pieejamajiem vērtējumu veidiem (1–5) un atgriež atbilstošās vērtības no "varianti" vārdnīcas.

7. _izveleties_studiju_kursu(ws)_:

Izvada visus pievienotos kursus, ļauj lietotājam izvēlēties tos pēc numura un atgriež rindas indeksu un kursa nosaukumu.

8. _pievienot_vid_vert(ws)_:

Apvieno vairākas definētās funkcijas: izvēlas kursu, vērtējuma veidu, nolasa jauno vērtējumu, aprēķina vidējo un ieraksta to excel kopā ar atsevišķo vērtējumu lapā - "Vērtējumi".

9. _studiju_kursu_ipatsvars_gala_vertejuma(vert_veidi)_:

Pajautā lietotājam procentuālo īpatsvaru katram vērtējuma veidam, lai aprēķinātu gala vērtējumu.

10. _aprekinat_gala_vertejumu(ws, rinda, ipatsvari)_:

Iterējot pa _vert_veidi_, nolasa vidējos vērtējumus katram vērtējuma veidam kursā un reizina tos ar lietotāja ievadīto īpatsvaru, lai iegūtu gala vērtējumu.

11. _saglabat_gala_vertejumu(ws, rinda, gala_vert)_:
    
Ieraksta aprēķināto gala vērtējumu 7. kolonnā un izvada paziņojumu konsolē.

13. _labot_pedejo_vertejumu(ws)_:
    
Ļauj lietotājam mainīt pēdējo ievadīto vērtējumu kādā no vērtējuma veidiem, piemēram, ja students kādu vērtējumu ir labojis, izdzēš aizvietoto un pārrēķina vidējo vērtējumu un saglabā to excel.


### Programmatūras izmantošana
Programma izpilda 6 galvenos uzdevumus:

1. Palaižot programmu un izvēloties izvēlnē 1) Izveidot jaunu failu, programma:

* Pārbaudīs, vai vertejumi.xlsx jau eksistē, ja fails nav atrodams, izveidos jaunu ar nepieciešamo sākotnējo darba lapas struktūru un nosaukumiem.

* Konsolē tiks izvadīts paziņojums par jauna faila izveidi.

![Ekrānuzņēmums 2025-05-20 204128](https://github.com/user-attachments/assets/a20ac8a0-7b6a-4fe6-b913-ea40fed7f838)

2. Izvēloties opciju 2) Pievienot jaunu studiju kursu:

* Lietotājam pajautās, cik studiju kursus vēlas pievienot, un lietotājam jāievada kursa nosaukumu:

![image](https://github.com/user-attachments/assets/d7d3d771-244d-40e5-a7fa-b9eeb73ed998)

3. Izvēloties opciju 3) Esošajam studiju kursam pievienot jaunu vērtējumu:

* Vispirms programma izvadīs vērtējuma veidu izvēli, lietotājam jāievada izvēles numurs, tad programma izvada pieejamos studiju kursus un atkal jāievada izvēles numurs.
  
* Izvadīs pieprasījumu ierakstīt konkrēta veida vērtējumu, kad lietotājs to ievadīs, tad izvadīs pašreizējo vidējo vērtējumu norādītajā vērtējuma veidā

![image](https://github.com/user-attachments/assets/478262b2-9b0e-44e5-ab91-42e1e3165a0b)

4. Izvēloties opciju 4) Aprēķināt vidējo gala vērtējumu studiju kursiem:

* Programma lūgs ievadīt katra vērtējuma veida īpatsvaru gala vērtējuma veidošanā
  
* Gala vērtējums tiks aprēķināts kā visu vidējo vērtējumu reizinājums ar īpatsvaru un to summa tiks ierakstīta 7. kolonnā.

![image](https://github.com/user-attachments/assets/3ae13344-41d7-4ca7-8a4f-8e2bd5e4bf6d)


5. Izvēloties opciju 5) Labot pēdējo vērtējumu kādam kursam:

* Programma izmantos iepriekšējos vērtējumus, izdzēsīs norādītā vērtējuma veida pēdējo vērtību un ļaus ievadīt jaunu vērtējumu.

* Ierakstot jauno vērtību, pārrēķinās vidējo vērtību un ierakstīs excel tabulā.

  ![image](https://github.com/user-attachments/assets/d49f44d6-a2e5-4f26-b74b-cce3ca60c789)


6. Izvēloties 6) Beigt darbu, programma saglabās visas izmaiņas, beigs izvēles darbību ciklu un pašu programmu.
   ![image](https://github.com/user-attachments/assets/9c80442f-a027-4766-a47a-8991b988f573)


Katru reizi pēc jebkuras darbības excel fails vertejumi.xlsx tiek atjaunināts, lai ievadītie vērtējumi vienmēr tiktu saglabāti.
