## Zadanie X. Hierarchie klas. _Mortal Machines_ - silnik gry

### Wprowadzenie

* W projektowanej grze istnieją dwa rodzaje maszyn: **czołgi** (_tanks_) i **myśliwce** (_fighters_). Każda maszyna ma **nazwę**, **pilota**, **punkty życia** (_health points_), **punkty siły ataku** (_attack points_), **punkty siły obrony** (_defense points_) i **cele ataku** (_attack targets_).

* Każdy pilot ma swoją **nazwę** (_name_) i **maszyny**, którymi kieruje (czołgi lub myśliwce).

* Piloci sporządzają **raporty** stanu dla wszystkich maszyn, których używają.

* Pilot w danym momencie może obsługiwać tylko jedną maszynę.

* **Czołgi** mają **tryb obrony**, który można **włączać** i **wyłączać**.

* **Myśliwce** mają tryb **agresywny**, który można **włączać** i **wyłączać**.

#### Cel zadania

Twoim celem jest uzupełnić kod gry według podanych powyżej zasad oraz poniższych wytycznych.

* Pobierz _solution_ zawierające szkielet gry, dołączony do tego zadania.

* Korzystając z podanego _solution_ dopisz wymagane klasy, dopisz wymagany kod

* Pamiętaj, że:

  1. **Nie możesz modyfikować interfejsów i ich przestrzeni nazw!**
  2. Używaj **dziedziczenia** i **dostarczonych interfejsów** wszędzie, gdzie to jest możliwe.
  3. Stosuj [silną spójność i luźne powiązania kodu (strong cohesion and loose coupling)](https://stackoverflow.com/questions/14000762/what-does-low-in-coupling-and-high-in-cohesion-mean).
  4. **Nie naruszaj implementacji interfejsów** poprzez dodawanie publicznych metod lub właściwości do konkretnych klas ponad te, które zdefiniowane są w interfejsach!
  5. Upewnij się, że w definiowanych klasach nie masz nigdzie **publicznych pól**.
  6. Wykorzystaj **maksymalnie** dostarczony kod do realizacji zadania (nie twórz tego, co już zostało utworzone poprawnie).

⚠️ Zanim rozpoczniesz kodowanie, zapoznaj się z całością tego dokumentu!

---

### Krok 1 - struktura klas i interfejsów

Masz 4 interfejsy i musisz zaimplementować ich funkcjonalność we właściwych klasach i właściwych lokalizacjach.

W aplikacji są 4 rodzaje bytów: `BaseMachine`, `Fighter`, `Tank` i `Pilot`:

#### -- `BaseMachine` --

`BaseMachine` jest klasą bazową dla wszystkich typów maszyn i **nie może być instancjonowana.**

**Dane:**

```cs
Name // string (Jeśli nazwa jest null lub pusta zgłość ArgumentNullException 
     // z treścią "Machine name cannot be null or empty.")
Pilot // pilot maszyny (jeśli pilot jest null zgłoś NullReferenceException 
      // z treścią "Pilot cannot be null.")
AttackPoints  // double
DefensePoints // double
HealthPoints  // double
Targets       // kolekcja string-ów
```

**Działanie/zachowanie:**

```cs
void Attack(IMachine target)
//Jeśli target jest null zgłoś NullReferenceException z treścią "Target cannot be null".
//Maszyna atakuje cel (target) i zmniejsza punkty życia celu 
//odejmując od punktów obrony celu punkty siły ataku atakującego punkty.
//Gdyby punkty życia celu po obliczeniu były ujemne, ustawiane są na 0.
//Dodaje nazwę celu do listy celów atakującego.
```

```cs
string ToString()
//Zwraca string z informacją o każdej maszynie. 
//Zwracany string musi być zgodny z poniższym formatem:
//"- {machine name}"
//" *Type: {machine type name}"
//" *Health: {machine health points}"
//" *Attack: {machine attack points}"
//" *Defense: {machine defense points}"
//" *Targets: " – if there are no targets "None". Otherwise {target1},{target2}…{targetN}
```

**Konstrukcja:**

Podczas inicjowania maszyna bazowa powinna przyjąć następujące wartości:

```cs
string name;
double attackPoints;
double defensePoints;
double healthPoints;
```

**Klasy potomne:**

Potencjalnie jest wiele konkretnych typów maszyny bazowej. W zadaniu rozważamy dwa: `Fighter` oraz `Tank`.

<center><hr width=30%/></center>

#### -- `Fighter` --

* ma `200` startowych punktów życia

**Dane:**

```cs
bool AggressiveMode //domyślnie true
```

**Działanie/zachowanie:**

```cs
void ToggleAggressiveMode()
//Przełącza DefenseMode(true <-> false).  
//Gdy AggressiveMode jest aktywowane, punkty ataku są zwiekszane o 50 
//i punkty obrony są zmniejszane o 25, 
//w przeciwnym przypadku (tzn. AggressiveMode jest deaktywowane) punkty ataku są zmniejszane
//o 50 i punkty obrony są zwiekszane o 25.
```

```cs
string ToString()
//Zwraca te same informacje, co klasa BaseMachine, 
//plus na końcu dopisuje - w zależności od trybu ataku - komunikat:
//" *Aggressive: {ON/OFF}"
```

<center><hr width=30%/></center>

### -- `Tank` --

* ma `100` startowych punktów życia

**Dane:**

```cs
bool DefenseMode //domyślnie true
```

**Działanie/zachowanie:**

```cs
void ToggleDefenseMode()
//Przełącza DefenseMode(true <-> false). 
//Gdy DefenseMode jest aktywowane, punkty ataku są zmniejszane o 40 
//i punkty obrony są zwiększane o 30, 
//w przeciwnym przypadku (tzn. DefenseMode jest deaktywowany) punkty ataku 
//są zwiększane o  40 i punkty obrony są zmniejszane o 30.
```

```cs
string ToString()
//Zwraca te same informacje, co klasa BaseMachine, 
//plus na końcu dopisuje - w zależności od trybu obrony - komunikat:
//" *Defense: {ON/OFF}"
```

<center><hr width=30%/></center>

#### -- `Pilot` --

**Dane:**

```cs
Name     // string (Jeśli nazwa pilota jest null lub pusta zgłasza ArgumentNullException 
         //z komunikatem "Pilot name cannot be null or empty string.") 
Machines // kolekcja maszyn
```

**Działanie/zachowanie:**

```cs
void AddMachine(machine)
//Dodaje maszynę przekazaną jako argumemnt do listy maszyn pilota. 
//Jeśli maszyna jest null zgłoś NullReferenceException 
//z komunikatem "Null machine cannot be added to the pilot."
```

```cs
string Report()
//Zwraca informacje o poniższym formacie:
//"{pilot name} - {machines count} machines"
//i dla każdej maszyny:
//"- {machine name}"
//" *Type: {machine type name}"
//" *Health: {machine health}"
//" *Attack: {machine attack)"
//" *Defense: {machine defense}"
//" *Targets: {machine targets}"
```

**Konstrukcja:**

Podczas inicjowania pilot powinien przyjąć następujące wartości:

```cs
string name;
```

---

### Krok 2 - logika biznesowa

**Zapoznaj się z architekturą projektu i uzupełnij go o wymagane klasy/interfejsy. Staraj się je umieszczać we właściwych lokalizacjach (przestrzeniach nazw).**

Logika biznesowa aplikacji koncentruje się wokół kilku **komend**.

Kluczowym interfejsem jest `IMachinesManager`. Musisz utworzyć klasę `MachinesManager`, która implementuje ten interfejs, realizując logikę działania komend.

> ⚠️ Klasa `MortalEngines.Core.MachinesManager` **nie może** przechwytywać wyjątków. Ona ma je zgłaszać lub przekazywać (ma komunikować się z zewnętrznym kodem za pomocą wyjątków, a nie za pomocą komunikatów). Metody tej klasy zwracają komunikaty zdefiniowane w klasie `OutputMessages`.

#### Komendy

* `HirePilot` 
  * parametry: `Name` - string
  * funkcjonalność: tworzy pilota o zadanej nazwie i dodaje go do kolekcji pilotów. Metoda zwraca jeden z komunikatów:
    * `"Pilot {name} hired"`
    * `"Pilot {name} is hired already"` - jeśli pilot o podanej nazwie istnieje, i wtedy nie tworzony jest pilot.

* `PilotReport`
  * parametry: `Name` - string
  * funkcjonalność: znajduje pilota o podanej nazwie i zwraca wynik działania metody `IPilot.Report()`.

* `MachineReport`
  * parametry: `Name` - string
  * funkcjonalność: znajduje maszynę o podanej nazwie i zwraca wynik działania metody `ToString()`.

* `ManufactureTank`
  * parametry: `Name` - string, `Attack` - double, `Defense` - double
  * funkcjonalność: tworzy czołg o zadanej nazwie i odpowiedniej liczbie punktów atakujących i obronnych. Zwraca jeden z podanych komunikatów:
    * `"Tank {name} manufactured - attack: {attack}; defense: {defense}"`
    * `"Machine {name} is manufactured already"` – jeśli czołg z podaną nazwą istnieje, i wtedy nie tworzony jest czołg.

* `ToggleTankDefenseMode`
  * parametry: `Name` - string
  * funkcjonalność: wyszukuje czołg o podanej nazwie i przełącza go w tryb _defense_. Zwraca jeden z podanych komunikatów:
    * `"Tank {name} toggled defense mode"`
    * `"Machine {name} could not be found"` – jeśli czołg z podaną nazwą nie istnieje.

* `ManufactureFighter`
  * parametry: `Name` - string, `Attack` - double, `Defense` - double
  * funkcjonalność: tworzy myśliwiec o zadanej nazwie i odpowiedniej liczbie punktów atakujących i obronnych. Zwraca jeden z podanych komunikatów:
    * `"Fighter {name} manufactured - attack: {attack}; defense: {defense}"`
    * `"Machine {name} is manufactured already"` – jeśli myśliwiec z podaną nazwą istnieje, i wtedy nie jest on tworzony.

* `ToggleFighterAggressiveMode`
  * parametry: `Name` - string
  * funkcjonalność: wyszukuje myśliwiec o podanej nazwie i przełącza go w tryb _aggressive_. Zwraca jeden z podanych komunikatów:
    * `"Fighter {name} toggled aggressive mode"`
    * `"Machine {name} could not be found"` – jeśli myśliwiec z podaną nazwą nie istnieje.

* `EngageMachine`
  * parametry: `PilotName`, `MachineName` - strings
  * funkcjonalność: wyszukuje pilota i maszynę o podanej nazwie. Jeśli oboje istnieją i maszyna nie jest zajęta, dodaje maszynę do listy pilotów i ustawia pilota jako obsługującego maszynę. Zwraca jeden z podanych komunikatów:
    * `"Pilot {pilot name} could not be found"`
    * `"Machine {machine name} could not be found"`
    * `"Machine {machine name} is already occupied"`
    * `"Pilot {pilot name} engaged machine {machine name}"`
    * UWAGA: Postępuj zgodnie z tą kolejnością komunikatów.

* `AttackMachines`
  * parametry: `AttackingMachineName`, `DefendingMachineName` - strings
  * funkcjonalność: wyszukuje dwie maszyny o zadanych nazwach i pierwsza atakuje drugą, jeśli obie maszyny mają _życie_ większe niż `0`. Zwraca jeden z podanych komunikatów:
    * `"Machine {name} could not be found"` - jeśli jedna z maszyn nie istnieje, maszyna atakująca ma priorytet dla komunikatu
    * `"Dead machine {name} cannot attack or be attacked"` - jeśli jedna z maszyn ma _życie_ większe niż `0`, maszyna atakująca ma priorytet dla komunikatu
    * `"Machine {machine name} is already occupied"`
    * `"Machine {defending machine name} was attacked by machine {attacking machine name} - current health: {defending machine health}"`

---

### Specyfikacja wejścia/wyjścia

Napisz program sprawdzający poprawność Twojej implementacji gry. Możesz (ale nie musisz) wykorzystać interfejsy `IEngine`, `IReader`, `IWriter`. Być może będziesz musiał uzupełnić kod o brakujące elementy.

Na standardowe wejście, w kolejnych liniach otrzymujesz komendy. Ostatnią komendą jest `Quit`.

Format komend (w nawiasach klamrowych podawane są wartości dla zmiennych):

```console
HirePilot {name}
PilotReport {name}
ManufactureTank {name} {attack} {defense}
ManufactureFighter {name} {attack} {defense}
MachineReport {name}
AggressiveMode {name}
DefenseMode {name}
Engage {pilot name} {machine name}
Attack {attacking machine name} {defending machine name}
Quit
```

Przyjmij, że komendy zawsze są dostarczone w poprawny sposób.

Na standardowe wyjście wypisujesz wynik działania komendy.

Jeśli w trakcie wykonania komendy zgłoszony zostaje wyjątek, wypisz słowo `Error:` plus komunikat związany z tym wyjątkiem

---

### Przykłady

#### Przykład 1

**Wejście:**

```console
HirePilot John
HirePilot Nelson
ManufactureFighter Boeing 180 90
Engage John Boeing
AggressiveMode Boeing
Quit
```

**Wyjście:**

```console
Pilot John hired
Pilot Nelson hired
Fighter Boeing manufactured - attack: 230.00; defense: 65.00; aggressive: ON
Pilot John engaged machine Boeing
Fighter Boeing toggled aggressive mode
```

#### Przykład 2

**Wejście:**

```console
HirePilot Smith
HirePilot Stones
ManufactureFighter Boeing 180 90
ManufactureTank T-72 100 100
Engage Stones T-72
Engage Smith Boeing
Attack Boeing T-72
MachineReport Boeing
MachineReport T-72
Quit
```

**Wyjście:**

```console
Pilot Smith hired
Pilot Stones hired
Fighter Boeing manufactured - attack: 230.00; defense: 65.00; aggressive: ON
Tank T-72 manufactured - attack: 60.00; defense: 130.00
Pilot Stones engaged machine T-72
Pilot Smith engaged machine Boeing
Machine T-72 was attacked by machine Boeing - current health: 0.00
- Boeing
 *Type: Fighter
 *Health: 200.00
 *Attack: 230.00
 *Defense: 65.00
 *Targets: T-72
 *Aggressive: ON
- T-72
 *Type: Tank
 *Health: 0.00
 *Attack: 60.00
 *Defense: 130.00
 *Targets: None
 *Defense: ON
```

---

⚠️ Jeśli jakieś polecenie nie jest wyczerpująco wyjaśnione, podejmij samodzielnie decyzje projektowe/implementacyjne.

---
Do oceny przesyłasz skompresowany plik z Twoim _solution_. Kod z błędami nie będzie oceniany. Przed przesłaniem pozbądź się binarnych plików (`Build -> Clean Solution`, usuń foldery `/bin` oraz `/obj`).

⚠️ Ocenie podlega kompletność i jakość kodu.

⚠️ Wolno korzystać z materiałów, dokumentacji, zasobów internetowych. **NIE WOLNO** się komunikować, udostępniać swojej pracy innym studentom i korzystać z pracy innych studentów. Złamanie tej zasady grozi nie poprawieniem pracy (0 pkt.) oraz - w sytuacjach drastycznych - zgłoszeniem nagannego postępowania do Komisji Dyscyplinarnej.
