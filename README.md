**English version at the bottom.**
# Projektowanie Układów Elektrycznych i Elektronicznych
To repozytorium zawiera dokumentację oraz projekty wykonane w ramach przedmiotu Projektowanie Układów Elektrycznych i Elektronicznych na 7 semestrze kierunku Automatyka i Robotyka na Politechnice Poznańskiej. Autorami projektów są Marcin Gałąska i Szymon Gogulski.
### Główne cele projektów
1. <b> Opracowanie projektu układu elektrycznego sterującego mikroklimatem terrarium dla kameleona. </b>
    - Układ steruje parametrami mikroklimatu terrarium.
    - Automatyczna regulacja temperatury poprzez jedną dzienną oraz jedną nocną lampę grzewczą. 
    - Automatyczna regulacja oświetlenia poprzez świetlówkę UVB.
    - Automatyczna regulacja wilgotności poprzez zanurzony w zbiorniku wodnym fogger.
    - Pomiar temperatury i wilgotności powietrza poprzez czujniki z przetwornikami.
    - Informowanie o stanie mikroklimatu poprzez panel HMI.
2. <b> Opracowanie projektu układu elektronicznego oraz projektu płytki PCB dalmierza laserowego. </b>
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
Schemat lepiej widoczny w jasnym motywie GitHub.

### Płytka PCB
![3](Zrzuty_ekranu/dalmierz_PCB.png)

### Płytka PCB 3D
![3](Zrzuty_ekranu/dalmierz_3d.png)

### Zasilanie 
1. W celu zasilenia układu wykorzystaliśmy ogniwo li-ion 18650.
2. Układ składa się z dwóch przetwornic: BUCK-BOOST RT6150B-33GQW zapewniającej napięcie 3.3V oraz BOOST LM2775DSG zapewniającej napięcie 5V.
3. Przed wybraniem przetwornic przeprowadziliśmy rachunek poboru prądu przy założeniu najwyższego obciążenia.
4. W ramach monitorowania stanu naładowania ogniwa zastosowaliśmy moduł LC709203. Moduł ten umożliwia bieżące przekazywanie informacji o stanie ogniwa poprzez interface I2C do mikrokontrolera.
5. Wejścia układu zostały zabezpieczone diodami Schottkiego na wypadek odwrotnego zamontowania ogniwa w gnieździe.

### Czujniki
Czujniki odległości będą podłączane do układu poprzez konektory I2C

### Bluetooth
W ramach możliwości zapisu pomiarów na urządzeniu zewnętrznym dodaliśmy moduł komunikacji bluetooth.

### Macierz przycisków / Wyświetlacz I2C LCD
1. Jako interface użytkownika zastosowaliśmy macierz  sześciu przycisków oraz wyświetlacz I2C.
2. Na płytce zamontowaliśmy osobny konektory I2C umożliwiający podłączenie wyświetlacza.
3. W celu uniknięcia drgań styków lepszym rozwiązanie byłoby zastosowanie dodatkowych filtrów RC pomiędzy przyciskami oraz mikrokontrolerem.

### RP2040, pamięć Flash i oscylator
Sercem układu jest mikrokontroler RP2040, sugerowany schemat mikrokontrolera, pamięci flash i oscylatora kwarcowego został wykorzystany z ogólnodostępnej dokumentacji.
### Programowanie
Wejście USB micro B zostało dodane, aby umożliwić bieżący rozwój oprogramowania.

***

# Terarrium
- Projekt opiera się o sterownik PLC DC/DC/RELAY firmy Siemens serii S7-1200 model 6ES7212-1HE40-0XB0.
- Projekt został wykonany w programie SEE Electrical V8R4.

### Zasilanie
1. Głównym źródłem zasilania układu jest standardowa jednofazowa linia AC 230V 50Hz z przewodem uziemiającym.
2. Obwód zasilania zawiera wyłącznik nadprądowy MCN110E oraz wkładkę bezpiecznikową 1115282107T dla fazy, wraz z wyłącznikiem różnicowoprądowym CFI6-25/2/003 dla żyły fazy i odniesienia.
3. Rozgałęzienia linii głównej przechodzą przez styki stycznika iCT50-25-20-230, włączonego w obwód włącznika i wyłącznika głównego.
4. Podłączonym do głównej linii zasilania jest zasilacz AST-PWR-7524 firmy Astor, konwertujący otrzymywane 230 AC na 24 DC, służące do zasilania szeregu komponentów w układzie.

### Czujniki
Pomiar temperatury wykonywany jest poprzez trójprzewodowy czujnik Pt100 sparowany z przetwornikiem AR580. Pomiar wilgotności powietrza realizowany jest poprzez układ czujnika z przetwornikiem AR250. Oba czujniki przekazują swój sygnał wejściowy z zakresu 0...10V DC na wejścia analogowe sterownika PLC. Oba czujniki zasilane są z linii 24V DC.

### Elementy wykonawcze
Grzanie nocne wykonuje lampa grzewcza Exo Terra Ceramic Heat Emitter 50W PT2044, wkręcona w żaroodporny klosz PT2060. Grzanie nocne wykonuje lampa grzewcza Exo Terra Night Heat Lamp 25W PT2122, wkręcona w żaroodporny klosz PT2060. Oświetlenie terrarium zapewniane jest poprzez świetlówkę Exo Terra Reptile UVB Nano 9W PT2366, wkręconą w klosz PT2364. Elementem nawilżającym powietrze wewnątrz terrarium jest membranowy fogger Exo Terra Fogger Ultrasonic Fog Generator PT2080. Elementy te zasilane są z sieci 230 AC, jednak w swoich obwodach posiadają styki styczników pomocniczych CA3SK11BD, których cewki podłączone są do wyjść cyfrowych sterownika PLC, celem kontroli zasilania każdego z elementów wykonawczych.

### Wizualizacja stanu mikroklimatu
Elementem informującym użytkownika o parametrach mikroklimatu terrarium jest panel HMI Simatic KTP400 6AV2123-2DB03-0AX0 firmy Siemens, połączony ze sterownikiem PLC poprzez złącze Ethernet.

### Szafa rozdzielcza
![3](Zrzuty_ekranu/terrarium_szafa.png)

***

# Design of Electrical and Electronic Systems
This repository contains documentation and projects made as part of the course Design of Electrical and Electronic Systems in the 7th semester of the Automation and Robotics major at the Poznań University of Technology. The authors of the projects are Marcin Gałąska and Szymon Gogulski.
### Main project goals
1. <b> Development of an electrical system controlling the microclimate of a chameleon terrarium. </b>
    - The system controls the parameters of the terrarium microclimate.
    - Automatic temperature control using one daytime and one nighttime heating lamp.
    - Automatic lighting control using a UVB fluorescent lamp.
    - Automatic humidity control using a fogger immersed in the water tank.
    - Measurement of air temperature and humidity using sensors with transducers.
    - Information about the microclimate status using the HMI panel.
2. <b> Development of an electronic circuit and a PCB design for a laser rangefinder. </b>
    - The device measures distance with a laser and ultrasonic sensor.
    - The device is powered by a LI-ION cell or a 9V battery.
    - The device has 6 buttons.
    - The device has a 4x20 LCD display.
    - The device measures the battery charge.
    - The device supports Bluetooth transmission.
***
# Rangefinder
- The project is based on the RP2040 microprocessor documentation.
- The project was made in KiCad7.0.

### Project flowchart
![1](Zrzuty_ekranu/dalmierz_drawio.svg)

### Electronic diagram
![2](Zrzuty_ekranu/dalmierz.svg)
Diagram more visible in GitHub's light theme.

### PCB board
![3](Zrzuty_ekranu/dalmierz_PCB.png)

### PCB board 3D view
![3](Zrzuty_ekranu/dalmierz_3d.png)

### Power supply
1. To power the system, we used a 18650 Li-ion cell.
2. The system consists of two converters: BUCK-BOOST RT6150B-33GQW providing a voltage of 3.3V and BOOST LM2775DSG providing a voltage of 5V.
3. Before selecting the converters, we calculated the current consumption assuming the highest load.
4. TO monitor the cell charge status, we used the LC709203 module. This module allows the transmission of information about the cell status via the I2C interface to the microcontroller.
5. The inputs of the system were protected with Schottky diodes in case the cell is mounted in the socket the other way around.

### Sensors
Distance sensors will be connected to the system via I2C connectors

### Bluetooth
In order to enable the recording of measurements on an external device, we have added a Bluetooth communication module.

### Button matrix / I2C LCD display
1. We have used a matrix of six buttons and an I2C display as the user interface.

2. We have mounted separate I2C connectors on the board to connect the display.

3. In order to avoid contact vibrations, a better solution would be to use additional RC filters between the buttons and the microcontroller.

### RP2040, Flash memory and oscillator
The heart of the system is the RP2040 microcontroller, the suggested diagram of the microcontroller, flash memory and quartz oscillator was used from publicly available documentation.

### Programming
The USB micro B inputs were left to enable ongoing software development.

***

# Terarrium
- The project is based on the Siemens S7-1200 series PLC DC/DC/RELAY model 6ES7212-1HE40-0XB0.
- The project was made in the SEE Electrical V8R4 program.

### Power supply
1. The main power supply for the system is a standard single-phase AC 230V 50Hz line with a ground wire.

2. The power supply circuit includes an MCN110E circuit breaker and a 1115282107T fuse insert for the phase, along with a CFI6-25/2/003 differential circuit breaker for the phase and reference wires.

3. The main line branches pass through the contacts of the iCT50-25-20-230 contactor, included in the main switch and switch circuit.
4. Connected to the main power line is the Astor AST-PWR-7524 power supply, converting the received 230 AC to 24 DC, used to power a number of components in the system.

### Sensors
Temperature measurement is performed by a three-wire Pt100 sensor paired with an AR580 transducer. Air humidity measurement is performed by a sensor system with an AR250 transducer. Both sensors transmit their input signal from the 0...10V DC range to the analog inputs of the PLC controller. Both sensors are powered from the 24V DC line.

### Actuators
Night heating is performed by the Exo Terra Ceramic Heat Emitter 50W PT2044 heating lamp, screwed into a heat-resistant PT2060 lampshade. Night heating is performed by the Exo Terra Night Heat Lamp 25W PT2122 heating lamp, screwed into a heat-resistant PT2060 lampshade. Terrarium lighting is provided by an Exo Terra Reptile UVB Nano 9W PT2366 fluorescent lamp, screwed into a PT2364 lampshade. The element that humidifies the air inside the terrarium is the Exo Terra Fogger Ultrasonic Fog Generator PT2080 membrane fogger. These elements are powered from the 230 AC network, but in their circuits they have contacts of auxiliary contactors CA3SK11BD, the coils of which are connected to the digital outputs of the PLC controller, in order to control the power supply of each of the executive elements.

### Visualization of the microclimate state
The element informing the user about the parameters of the terrarium microclimate is the HMI panel Simatic KTP400 6AV2123-2DB03-0AX0 by Siemens, connected to the PLC controller via an Ethernet port.

### Switchboard
![3](Zrzuty_ekranu/terrarium_szafa.png)


