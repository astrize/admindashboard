//////////////////////////////////////////////////////////////////
AA-ORDERS (Narudžbe)
http://admin.strize.codelofts.com/DEVELOPER/HTML_Full_Version/AA-orders.html
//////////////////////////////////////////////////////////////////

* Izbornici

- NARUDŽBE 
	- kronološki prikaz svih narudžbi. B2C prikaz kombinira narudžbe s brojevima 01-xxxx i 03-xxxx, a B2B prikaz kronološki slaže narudžbe s rednim brojem 02-xxxx
	- pod NARUDŽBE treba kombinirat sve statuse narudžbi kronološki neovisno o statusu, osim storniranih koje su skrivene

- NARUČENO DOBAVLJAČI
	- u ovom prikazu se vide sve narudžbe koje su dobile status NARUČENO

- ARTIKLI
	- prikaz artikala i mogucnost editiranja osobina, dnevne ponude, virtualne kolicine/vpc, slika artikala ...

- MODELI
	- editiranje modela, izmjena slika, opisa, detalja, teh specifikacija...

- KUPONI
	- dodavanje kupona (promo kodova)

- KORISNICI
	- izmjena informacija korisnika *B2B* i B2C

- POSTAVKE
	- izmjena postavki, prikaza rezultata na nivou korisnika




* NARUDŽBE - definicija elemenata po ID-u

** search
- obavlja pretragu svih podataka zapisanih u narudzbi. Znaci ako upišem ZAGREB on mi treba izlistat sve narudzbe kojima je dostava u zagreb, isto vrijedi i za model gume, npr ako se upiše CrossClimate trebao bi izlistati sve narudzbe u kojima je kupljen model gume CrossClimate. Pretragu najčešće radimo prema iznosu, načinu placanja, imenu kupca, broju narudzbe i nazivu proizvoda. Ali je isto tako bitno da sva polja narudzbe reagiraju na pretragu.
- Broj narudzbi koje treba prikazati po stranici definiran je poljem ID_entries

** entries
- definira broj prikazanih narudžbi po jednom page-u. Default je postavljeno na 50 narudžbi za sve korisnike. 
- Ovu opciju korisnik moze promjeniti u postavkama.

** prikaži stornirano
- ako se oznaci checkboxa, prikazati će se i narudzbe s statusom STORNIRANO

** open-close +/-
- klikom na + otvara se brzi pregled narudzbe (po prinicipu accordiona) i sljedeća polja narudzbe postanu editabilna: STATUS, NACIN PLACANJA, BROJ PAKETA, DOSTAVNA SLUŽBA, EMAIL i NAPOMENA. 
- Polja BROJ PAKETA, EMAIL i NAPOMENA imaju slobodni unos. A polja STATUS, NACIN PLACANJA i DOSTAVNA SLUZBA imaju predefinirane opcije. 
- Klikom na SPREMI izmjene u tim poljima se sačuvaju u narudzbi i accordion se zatvara. 
- Klikom na UREDI otvara sa nova stranica sa detaljima te narudžbe i više opcija (AA-orders-details.html)
- Klikom na ODBACI sve unešene promjene nece se spremiti i narudzba (accordion) ce se zatvoriti
- Na smanjenim ekranima (mobitelima, tabletima) moguce je urediti samo STATUS NARUDŽBE, ostala polja su staticna.

** STATUS narudžbe
- status narudžbe se moze definirati ručno ili se to radi automatski. 
- NOVO ; početni status, narudžba nije obrađena
- NA SLAGANJU ; automatski kada u bazi postoji radni nalog s istim brojem web narudžbe (npr 01-16981)
- SLOŽENO ; automatski kada se potvrdi radni nalog s istim brojem web narudžbe
- FAKTURIRANO ; automatski kada se napravi račun s istim brojem web narudžbe
- U OBRADI ; ručno, ako se radi o međuskladisnici, neplaćenom virmanu ili nekim drugim uvjetom
- ODBIJENO ; automatski ako je neuspjelo placanje karticom
- POSLANO ; automatski ako je upisan broj paketa ili ručno ako je paket dostavljen na trajekt ili dostavljen kombijem
- STORNIRANO ; ručno!!! Ukoliko je npr kartica odbijena, takva narudžba se mora pojaviti i imati status ODBIJENO, ukoliko je status plaćanja negativan (timeout, neispravna kartica, nedovoljan iznos na kartici...), te je potrebno za takvu narudzbu ispisati artikle koji su bili dodani u košaricu i sve ostale podatke koje imamo. Po neuspjeloj transakciji takvog kupca možemo nazvati i vidjeti u cemu je bio problem. Status takve narudzbe svakako treba ručno izmjeniti u STORNIRANO. Narudzbe s takvim statusom se nakon spremanja više ne prikazuju u NARUDŽBAMA, osim ako nije kliknut box 'prikazi stornirano'
- PREUZETO ; ručno za narudžbe kojima je preuzima bilo u poslovnicama Auto Antonio
- NARUČENO ; ručno za narudzbe gdje treba naruciti artikle kod dobavljaca, ako se odabere ovaj status treba prikazat dodatna dva polja za unos NARUCIO i dodati DOSTAVA INFO za dostavu
/ kada se promijeni status, potrebno je i promijeniti klasu u span-u npr. label-poslano ili label-stornirano
/ svi automatski statusi se mogu dodjeliti i ručno u slucaju da prilikom radnog naloga nije upisan broj web-narudzbe ili slicnog propusta. 

** NAČIN PLAĆANJA
- GOTOVINA - automatski; odnosi se samo na narudzbe kojima je preuzimanje u poslovnicama AA i placanje je gotovinom
- KARTICA AA - automatski; odnosi se samo na narudzbe kojima je preuzimanje u poslovnicama AA i placanje je karticom
- KARTICA AMEX 12 - automatski, odnosi se na sve narudzbe gdje je placanje karticom uspjesno obavljeno. Tada je u nazivu statusa potrebno ispisati ime kartice npr AMEX, i broj rata npr 06. Ime kartice i broj rata daje sustav za autorizaciju kartice. 
- KARTICE ODBIJENO - automatski ako je kartica odbijena. Ukoliko se radi o TIMEOUTu onda treba napisati KARTICE TIMEOUT ili suklnadno povratnoj poruci od autorizacijske kuce. Ako je ovaj status dodjeljen automatski narudžbu nije moguće obraditi i po njoj napraviti radni nalog. Nakon kontakta kupca, djelatnik će ručno izmjeniti status narudzbe u STORNIRANO
- POUZEĆEM, automatski ako je odabrano plaćanje prilikom preuzimanja
- VIRMANOM - NA ČEKANJU ; automatski ako je odabrano placanje virmanom/bankovnom uplatom
- VIRMANOM - PLAČENO ; ručno kada knjigovodstvo potvrdi da je uplata vidljiva na računu
Napomena: NAČIN PLAĆANJA se može mijenjati jedino ako je način "VIRMANOM NA ČEKANJU", ostale vrste plaćanja su needitabilne

** KUPAC & MJESTO & email
- polje kupac je uvijek ime i prezime preuzeto s ADRESE ZA RAČUN
- MJESTO je uvijek preuzeto s ADRESE ZA DOSTAVU
- E-MAIL je uvijek preuzeto s ADRESE ZA RAČUN

** DOSTAVNA SLUŽBA
- GLS - osnovno ako je odabrana dostava na adresu ili dostava kod montažnog partnera
- AA POSLOVNICA - ako je odabrano preuzimanje ili montaza u jednom od AA servisa
- Svi ostali statusi dostave se rucno mijenjaju!

** BROJ PAKETA
- ručni unos za sada, u budućnosti to polje ce se unositi automatski preko API-a kurirske sluzbe. Ako se broj paketa unese rucno ili automatski, status narudzbe treba izmjeniti automatski u POSLANO

** NAPOMENA
- ručno upisana napomena djelatnika koji je obradio narudzbu ili zaprimio poruku od kupca

** BOTUNI
- SPREMI ; sačuva sve izmjene i zatvori narudzbu (accordion)
- ODBACI ; zatvara narudzbu i odbacuje sve izmjene
- UREDI ; vodi na detaljno uređivanje narudzbe, izmjenu dobavljaca ili slicno (treba napravit prikaz, AA-orders-details.html)




//////////////////////////////////////////////////////////////////
AA-ORDERS-SUPPLIERS (Naručeno)
http://admin.strize.codelofts.com/DEVELOPER/HTML_Full_Version/AA-orders-suppliers.html
//////////////////////////////////////////////////////////////////

Na ovoj stranici se prikazuju sve narudžbe koje imaju status “Naručeno”. Prikaz je slican onome na stranici “Narudžbe”.

Svaki red prikazuje jednu narudžbu

** search
- obavlja pretragu svih podataka zapisanih u narudzbi. Znaci ako upišem ZAGREB on mi treba izlistat sve narudzbe kojima je dostava u zagreb, isto vrijedi i za model gume, npr ako se upiše CrossClimate trebao bi izlistati sve narudzbe u kojima je kupljen model gume CrossClimate. Pretragu najčešće radimo prema iznosu, načinu placanja, imenu kupca, broju narudzbe i nazivu proizvoda. Ali je isto tako bitno da sva polja narudzbe reagiraju na pretragu.
- Broj narudzbi koje treba prikazati po stranici definiran je poljem ID_entries

** entries
- definira broj prikazanih narudžbi po jednom page-u. Default je postavljeno na 50 narudžbi za sve korisnike. 
- Ovu opciju korisnik moze promjeniti u postavkama, vrijedi isto kao i u prikazu “Narudžbe”


** open-close +/-
- klikom na + otvara se brzi pregled narudzbe (po prinicipu accordiona) 

** BROJ NARUDŽBE
Prikazuje broj narudžbe (kao i kod stranice “Narudžbe”)

** DATUM
Sastoji se od dva datuma, prvi datum je kada je napravljena narudžba, dok drugi datum prikazuje kada je narudžba dobila status “Naručeno”

** KUPAC
Ime kupca (kao i kod stranice “Narudžbe” - iz “Adrese za račun”)

** DOBAVLJAČ
Nazivi dobavljača za svaki od proizvoda, može ih biti više, prikazuju se jedan ispod drugoga

** NARUČENO
Naziv svih naručenih stavki, ako ih je više prikazuju se jedno ispod drugog

** NAPOMENA
Napomena već prethodno upisana

** NARUČIO
Prikazuje tekst iz unešenog polja “Naručio”

** DOSTAVA INFO
Prikazuje tekst iz unešenog polja “Dostava info”

** NAČIN PLAĆANJA
Prikazuje način plaćanja iz narudžbe (kao i kod stranice “Narudžbe”)


U donjem dijelu se nalaze podaci jednaki kao i kod stranice “Narudžbe”, uz dodatak da kod tablice naručenih proizvoda, postoji checkbox sa lijeve strane. Checkboxevima se selektiraju proizvodi koji su dosli. Moguce je selektirani jedan ili sve checkboxove. Kada se odabare jedan checkbox i klikne se “Spremi”, narudžba jos uvijek ima isti status te taj checkbox ostaje spremljen. Kada se odaberu svi checkboxovi i klikne se botun “Spremi”, narudžbi se mijenja status iz “Naručeno” u novi status - “Stiglo na lager”, te se zbog toga više ne prikazuje na stranici “Naručeno”.

** BOTUNI
- SPREMI ; sačuva sve izmjene i zatvori narudzbu (accordion), ako su svi checkboxovi oznaceni - mijenja status narudžbe u “Stiglo na lager”
- ODBACI ; zatvara narudzbu i odbacuje sve izmjene
- UREDI ; vodi na detaljno uređivanje narudzbe, izmjenu dobavljaca ili slicno (AA-orders-details.html)





//////////////////////////////////////////////////////////////////
AA-ORDERS-DETAILS (Detalji narudžbe)
http://admin.strize.codelofts.com/DEVELOPER/HTML_Full_Version/AA-orders-details.html
//////////////////////////////////////////////////////////////////

Template sluzi za detaljan prikaz svake narudžbe. Aktivira se nakon klika na botun “Uredi” u prethodnom prikazu (“Naručeno”).


** SPREMI
Sprema sve izmjene i refresha ekran

** SPREMI I ZATVORI
Sprema sve izmjene i vraća korisnika na prethodni ekran (“Naručeno”)

** ZATVORI
Vraća korisnika na prethodni ekran bez spremanja izmjena

** STATUS NARUDŽBE, NAČIN PLAĆANJA, DOSTAVA, BROJ PAKETA, EMAIL KUPCA, NAPOMENA
Sva polja su editabilna, logika je ista kao i u prethodnom ekranu (“Narudžbe”) te ju se može kopirati.

** 1. TABLICA
Prikazuje sve naručene artikle, svaki artikl zauzima jedan red. Sa desne strane se nalazi botun “IZMJENI” koji otvara modal prozor (za svaki artikl zasebno) u kojem ce se prikazivati svi dobavljaci.

** MODAL PROZOR
Modal prozor se sastoji od tri dijela. Prikaz se vrsi po sljedecoj logici:

—— AKO JE DOBAVLJAC AUTO ANTONIO:
Prikazana je samo prva tablica koja sluzi informativno - prikazuju se sva skladista od Auto Antonia poredana po kolicini proizvoda koje sadrze. Botuni “Spremi izmjene” i “Zatvori” imaju istu funkciju te samo zatvaraju modal,a cijena ostaje ista.

—- AKO DOBAVLJAC NIJE AUTO ANTONIO:
Prikazuje se samo druga tablica. U njoj se nalaze svi dobavljaci koji sadrze naruceni proizvod. Poredak se vrsi prema najmanjoj brutto vrijednosti (EUR). U svakom retku se nalazi checkbox za odabir dobavljaca - moze biti odabran samo jedan dobavljac, ako se odabere neki drugi obavljac, checkbox sa prethodnog se uklanja. 
Prilikom oznacavanja checkboxa automatski se izracunava nova ukupna cijena na dnu za taj proizvod. Ispod nove ukupne cijene prikazuje se upozorenje da li je nova cijena veca ili manja. Klikom na “Spremi promjene” zatvara se modal te se azuriraju svi novi iznosi i naziv novog dobavljaca, dok se stari iznos i dobavlja prekrizi. Klikom na “Zatvori” - modal prozor se zatvori bez spremanja izmjena.

** 2. TABLICA
U drugoj tablici su prikazan podaci po jednakoj logici kao i na prethodnom ekranu (“Naruceno”) te ih se moze kopirati.


//////////////////////////////////////////////////////////////////
AA-MODELS-BATCH (Grupno uređivanje modela)
http://admin.strize.codelofts.com/DEVELOPER/HTML_Full_Version/AA-models-batch.html
//////////////////////////////////////////////////////////////////

Template sluzi za masovno (batch) uredivanje modela

Nakon otvaranja stranice prikazuje se samo gornji dio za pretrazivanje. Polja se odabiru s lijeva na desno, i ovisno o prethodnom polju, ucitavaju se vrijednosti za sljedeca polja.

** KATEGORIJA
Glavna kategorija

** PODKATEGORIJA
Podkategorija odabrane kategorije

** PROIZVOĐAČ
Lista proizvodaca odabranih kategorija

** PRIKAZI SAMO MODELE BEZ SLIKA
Ako se odabere ovo, rezultati pretrazivanja ce ukljucivati samo modele koji ne sadrze slike.


Nakon klikanja na botun pretraži, ucitavaju se rezultati pretrage te ostala polja ispod.

** REZULTATI PRETRAGE
Prikaz svih rezultata u obliku liste sortirane abecedno (asc). Svi rezultati se prikazuju na istoj stranici. Kraj svakog modela se nalazi checkbox kojim se moze odabrati model kojemu ce postavke biti izmjenjene.

** SLIKE BATCH UPLOAD
Sluzi za upload slika za svaki od odabrani model. Moze se uploadati jedna ili više slika

DODATNE POSTAVKE:

** ALT SLIKE
Zamjenuje ALT vrijednost i “Opis” (title) svake uploadane slike u prethodnom polju. Ako je više slika, na svaku se primjenjuje isti ALT.

** GARANCIJA
Broj godina garancije, numericka vrijednost ce biti unesena.

** VIDEO LINK
Youtube video link.

** OPIS PROIZVODA
Proizvoljan opis proizvoda u HTML obliku

** Testovi
Testovi napisani u HTML obliku

** DODATAK POLJU NAPOMENA
Dodatak napisan u HTML obliku

** SPREMI
Pridruzuje sve upisane postavke i slike selektiranim modelima. AKO NEKO POLJE OSTANE PREZNO, NJEGOVA VRIJEDNOST ZA SELEKTIRANI MODEL NECE BITI IZMJENJENA. (npr. ako neki od modela vec ima sliku, a sada se utipka nesto u polje “Garancija”, napraviti ce izmjenu samo za polje garancija, dok ce slike ostati one koje vec sadrzi.)
