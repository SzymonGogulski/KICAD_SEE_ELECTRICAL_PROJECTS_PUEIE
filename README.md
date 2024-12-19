English version at the bottom.
# Projekty PUEIE
To repozytorium zawiera dokumentację oraz projekty wykonane w ramach przedmiotu Projektowanie Układów Elektrycznych i Elektronicznych na 7 semestrze kierunku Automatyka i Robotyka na Politechnice Poznańskiej. Autorami projektów są Marcin Gałąska i Szymon Gogulski.
### Główne cele projektów
1. <b> Opracowanie projekt układu elektrycznego w automatycznym terrarium dla zwierząt. </b>
    - Układ seteruje parametrami mikroklimatu terrarium.
    - Zapewniona jest regulacja automatyczna temperatury, oświetlenia i wilgotności.
    - Układ steruje pompą doprowadzającą wodę do karmnika.
2. <b> Opracowanie projekt układu elektronicznego oraz projektu płytki PCB dalmierza laserowego. </b>
    - Urządzenie wykonuje pomiar odległości czujnikiem laserowym i ultradźwiękowym.
    - Urządzenie jest zasilane z ogniwa LI-ION lub z baterii 9V.
    - Urządzenie posiada 6 przycisków.
    - Urządzenie posiada wyświetlacz LCD 4x20.
    - Urządzenie wykonuje pomiar naładowania akumulatora/ baterii.
    - Urządzenie obsługuje transmisję bluetooth.
***
# Dalmierz
- Projekt opiera się o dokumentację mikroprocesora RP2040.
- Projekt został wykonany w programi KiCad7.0.

### Schemat ideowy projektu
![1](Zrzuty_ekranu/dalmierz_drawio.svg)

### Schemat elektronicznych
![2](Zrzuty_ekranu/dalmierz.svg)

### Płytka PCB
![3](Zrzuty_ekranu/dalmierz_PCB.png)

### Płytka PCB 3D
![3](Zrzuty_ekranu/dalmierz_3d.png)

### Zasilanie 
1. W celu zasilenia układu wykorzystaliśmy ogniwo li-ion 18650.
2. Układ składa się z dwóch przetwornic: BUCK-BOOST RT6150B-33GQW zapewniającej napięcie 3.3V oraz BOOST LM2775DSG zapewniającej napięcie 5V.
3. Przed wybraniem przetwornic przeprowadziliśmy rachunek poboru prądu przy założeniu najwyższego obciążenia.
4. W ramach możliwości monitorowania stanu naładowania ogniwa zastosowaliśmy moduł LC709203. Moduł ten umożliwia bierzące przekazywanie informacji o stanie ogniwa poprzez interface I2C od mikrokontrolera.
5. Wejścia układu zostały zabezpieczone diodami Schottkiego na wypadek odwrotnego zamontowania ogniwa w gnieździe.

### Czujniki
Czujniki odległości będą podłączane do układu poprzez konektory I2C

### Bluetooth
W ramach umożliwienia możliwości zapisu pomiarów na urządzeniu zewnętrzym dodaliśmy moduł komunikacji bluetooth.

### Macierz przycisków / Wyświetlacz I2C LCD
1. Jako interface użytkownika zastosowaliśmy macierz  sześciu przycisków oraz wyświetlacz I2C.
2. Na płytce zamontowaliśmy osobny konektory I2C umożliwiający podłączenie wyświetlacza.
2. W celu uniknięcia drgań styków lepszym rozwiązanie było by zastosowanie dodatkowych filtrów RC pomiędzy przyciskami oraz mikrokontrolerem.

### RP2040, pamięć Flash i oscylator
Sercem układu jest mikrokontroler RP2040, sugerowany schemat mikrokontrolera, pamięci flash i oscylatora kwarcowego został wykorzystany z ogólnodostępnej dokumentacji.
### Programowanie
Wejści USB micro B zostało aby umożliwić bierzący rozwój oprogramowania.

***


# Terarrium
Projekt zostanie wykonany w programie See Electrical.



