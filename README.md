# 🏭 Automatizarea și Controlul Distribuit al unui Cuptor Industrial (PLC Siemens LOGO!)

Acest proiect reprezintă o soluție completă de inginerie pentru automatizarea proceselor termice industriale. Sistemul utilizează o arhitectură de **control distribuit** bazată pe două automate programabile (PLC) **Siemens LOGO! 8**, optimizând procesul de încălzire prin separarea logicii de execuție de cea de monitorizare și siguranță.

---

## 🎯 1. Scopul și Obiectivele Proiectului
Proiectul a fost conceput pentru a răspunde cerințelor riguroase din mediul industrial (metalurgie, ceramică sau tratamente termice):
* **Precizie Termică:** Controlul temperaturii într-un domeniu strict definit pentru a asigura calitatea produselor.
* **Redundanță și Siguranță:** Implementarea unor protocoale de avarie care să prevină distrugerea echipamentelor în caz de erori hardware.
* **Arhitectură Modulară:** Utilizarea a două PLC-uri interconectate pentru a reduce sarcina de procesare și a permite depanarea independentă.
* **Interfață Operator (HMI):** Sistem de semnalizare vizuală și sonoră pentru stările de funcționare, eroare și necesar de mentenanță.

---

## ⚙️ 2. Arhitectura Sistemului și Hardware
Sistemul este împărțit în două unități de procesare distincte, interconectate galvanic prin semnale de stare ($Q \rightarrow I$).

### 2.1. PLC 1 - Controlul Procesului Termic (Logică FBD)
Este unitatea „de forță” care interacționează direct cu senzorii analogici.
* **Intrări:** 1x Senzor de temperatură analogic (AI1 - PT100/TC).
* **Ieșiri:** 1x Comandă rezistențe de încălzire (Q1).
* **Limbaj:** **FBD (Function Block Diagram)** - optim pentru procesarea semnalelor analogice.
* **Fișier:** `Subproces_1.lsc`

### 2.2. PLC 2 - Monitorizare și Securitate (Logică LAD)
Gestionează partea electrică, interblocările și protecția operatorului.
* **Intrări:** Start/Stop, Reset, Senzor supraîncălzire, Semnal stare PLC1.
* **Ieșiri:** Alarmă sonoră, Indicator Mentenanță, Sistem Răcire progresivă.
* **Limbaj:** **LAD (Ladder Diagram)** - standardul industrial pentru logica de relee.
* **Fișier:** `Subproces2LADFinal.lld`

---

## 🧠 3. Logica de Funcționare Detaliată



### 3.1. Algoritmul de Control Termic
S-a implementat o logică de tip **Histerezis** (ON/OFF cu praguri) pentru a evita uzura prematură a contactoarelor:
* **Prag Minim:** Activarea rezistențelor la scăderea temperaturii sub limita setată.
* **Prag Maxim:** Decuplarea încălzirii la atingerea temperaturii de proces.

### 3.2. Răcirea Progresivă în Trepte
Pentru a proteja integritatea materialelor prelucrate, sistemul nu oprește brusc răcirea, ci utilizează un protocol în trei trepte gestionat prin temporizatoare:
1. **Treapta 1:** Ventilare maximă imediat după finalizarea ciclului.
2. **Treapta 2:** Reducerea intensității pentru stabilizarea temperaturii.
3. **Treapta 3:** Răcire lentă până la pragul de siguranță pentru descărcare.

---

## ⚠️ 4. Protocoale de Siguranță și Alarmare
Siguranța este pilonul central al acestui proiect, fiind implementate 4 filtre de eroare:

1. **Watchdog de Încălzire:** Dacă ieșirea de încălzire este activă, dar AI1 nu raportează o creștere de temperatură în 30 de secunde, sistemul declară "Rezistență Defectă".
2. **Filtru de Zgomot (Debouncing):** Alarma de supraîncălzire se activează doar dacă senzorul raportează eroarea pentru mai mult de 5 secunde continuu.
3. **Contor de Cicluri (Mentenanță):** La fiecare 5 cicluri, PLC2 blochează pornirea și aprinde un indicator de service.
4. **Interblocare (Safety Interlock):** Orice eroare critică necesită un **Reset Manual (I16)**; repornirea automată este interzisă pentru a forța verificarea de către operator.

---

## 📊 5. Implementarea în LOGO! Soft Comfort
Proiectul include diagramele complete dezvoltate în versiunea 8.3:
* **FBD:** Utilizarea blocurilor de comparare analogică și a amplificatoarelor de semnal.
* **LAD:** Implementarea circuitelor de automenținere și a temporizatoarelor de tip *Off-Delay* și *On-Delay*.
* **P&ID:** Reprezentarea simbolică a fluxului de proces (Piping and Instrumentation Diagram).



---

## 🛠️ 6. Ghid de Instalare și Rulare
1. **Software:** Instalați **LOGO! Soft Comfort V8.3** sau mai nou.
2. **Configurare:** Încărcați `Subproces_1.lsc` pe primul PLC și `Subproces2LADFinal.lld` pe al doilea.
3. **Hardware:** Realizați conexiunile fizice între Q-ul primului PLC și I-ul celui de-al doilea (conform schemei de comunicație din PDF).
4. **Simulare:** Puteți utiliza modul de simulare (F3) pentru a varia intrarea analogică AI1 și a observa declanșarea secvențelor.

---

## 👨‍💻 Realizat de
**Nicolae-Bogdan Proaspătu**
*Student la Automatică și Informatică Aplicată, Universitatea Tehnică de Construcții București*
An universitar 2025–2026

---

## ⚖️ Licență
Acest proiect este dezvoltat exclusiv în scop educațional pentru disciplina **Aplicații cu Automate Programabile**.
