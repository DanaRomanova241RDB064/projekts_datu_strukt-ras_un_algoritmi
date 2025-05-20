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
_wb.create_sheet('Vērtējumi')_ izveido otro lapu, kur tiek saglabātas visas atsevišķās vērtības(vērtējumi), lai pie atkārtotas programmas palaišanas tie nepazustu, jo sākumā tie tiek pievienoti sarakstiem, kas paliek tukši programmas beigās, tādēļ tas palīdz aprēķināt kopējo vidējo vērtējumu, ja arī programma tiek palaista vairākas reizes.
* Darbības ar excel šūnām:
_ws.cell(row, column, value=...)_ - ievieto vai nolasa konkrētas šūnas saturu. Piemēram, kad vajag aprēķināt vidējo kontroldarbu atzīmi, _ws.cell(row=rinda, column=2, value=vid_vert)_ ieraksta to Excel konkrētā šūnā.
_ws.append([...])_ pievieno jaunu rindu.
_cell.number_format = '0.00'_ formatē excel tabulās redzamos vērtējumus ar 2 cipariem aiz komata
### os
Šī bibliotēka ir paredzēta, lai ātri pārbaudītu failus pirms kādas darbības, tieši šajā projektā _os.path.exists(path)_ funkcijā _faila_izveide()_ sākumā pārbauda, vai vertejumi.xlsx jau ir mapē, ja nav, tad izveido jaunu failu ar šo nosaukumu, pretējā gadījumā failu vienkārši atver un turpina visas darbības ar to.

### Datu struktūras un to izskaidrojums
Projektā galvenokārt ir trīs veidu Python datu struktūras, lai sakārtotu un apstrādātu studentu vērtējumus:
* Saraksti _kd, ld, md, teo, eks = [],[],[],[],[]_
Katrs no šiem sarakstiem satur atsevišķā vērtējumu veida rezultātus: kontroldarbi, laboratorijas darbi, mājasdarbi, teorijas testi un eksāmens. Piemēram, kad lietotājs pievieno jaunu kontroldarbu vērtējumu, ar _kd.append(vert)_ šī vērtība tiek saglabāta sarakstā kd, lai to vēlāk izmantotu vidējā vērtējuma aprēķinam ar _sum(kd) / len(kd)_.
* Vārdnīca _varianti={1:(kd,2,"kontroldarba"),2:(ld,3,"labaratorijas darba"),3:(md,4,"mājasdarba"),4:(teo,5,"teorijas testa"),5:(eks,6,"eksāmena")}_
  Atslēgvērtības (1–5) sasaista trīs vērtības, kas satur pārbaudes veida sarakstus, excel kolonnas indeksu un vērtējuma veida nosaukumu.
Piemērs: _varianti[2] == (ld, 3, "laboratorijas darba")_, ja lietotājs izvēlas “2) Laboratorijas darbs”, tad programma izmanto sarakstu ld, raksta vidējo vērtējumu 3. kolonnā un pie izvades, kur vajag norādīt pārbaides veidu, izvada -  "laboratorijas darba".
* "Tuple" saraksts _vert_veidi = [("Kontroldarbi", 2), ("Labaratorijas darbi", 3), ("Mājasdarbi", 4),("Teorijas testi", 5),("Eksāmens", 6)]_
vert_veidi ir saraksts ar tuple vērtībām, kas satur vērtējuma veida nosaukumu un atbilstošo excel kolonnas indeksu. Šī datu struktūra tiek izmantota gala vērtējuma aprēķināšanai, kad nepieciešams iterēt _for veids, kol in vert_veidi:_, iegūstot katra veida vidējo vērtējumu no šūnas un reizināt ar lietotāja ievadīto vērtējuma veida īpatsvaru, lai aprēķinātu galīgo vidējo vērtējumu studiju kursā.

Izmantotās datu struktūras palīdz ātri un ērti pievienot un aprēķināt vērtējumus, jo kodā nevajag pārrakstīt liekas rindiņas katram vērtējuma veidam un, ja būs nepieciešamība pieveinot papildus vērtējuma veidu, tad tas būs izdarāms daudz ātrāk.

### Definētās funkcijas un to darbība
* 


### 
