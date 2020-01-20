# Podstawy ochrony danych

## Szyfr Playfair

### Opis algorytmu
Dana jest macierz 5x5 którą wypełniamy 25 literami bez powtórzeń. Najpierw wpisujemy klucz pomijając powtarzające się litery, a następnie dopełniamy macierz pozostałymi literami alfabetu ( bez polskich liter), przy czym literę J zamieniamy literę na I. Np. klucz KEYWORD:

|K|E|Y|W|O|
|-|-|-|-|-|
|R|D|A|B|C|
|F|G|H|I|L|
|M|N|P|Q|S|
|T|U|V|X|Z|

Następnie należy podzielić tekst jawny na pary liter np. WORD zapisujemy jako WO=RD.

#### Zasady szyfrowania
- Jeśli w parze liter występuje powtórzenie, wstawiamy filtr np. 'X', PASSOWRD szyfrujemy PA-SX-WO-RD.
- Jeżeli tekst ma nieparzystą ilość liter wstawiamy na końcu filtr np. 'X', 
- Jeśli litery wypadają w tym samym rzędzie, zastępujemy każdą literę literą z prawej, np. WO kodujemy jako OK.
- Jeśli obie litery występują w tej samej kolumnie, zastępujemy każdą z liter literą z dołu, np. KU kodujemy jako RK. 
- W przypadku, różnego wiersza i kolumnyzastępujemy je literami tworzącymi prostokąt z literami szyfrowanymi, np. KA kodujemy jako YR.

#### Przykład
Mamy klucz KEYWORD i chcemy zaszyfrować słowo PASSWORD:
1. Tworzymy macierz z użyciem klucza:

|K|E|Y|W|O|
|-|-|-|-|-|
|R|D|A|B|C|
|F|G|H|I|L|
|M|N|P|Q|S|
|T|U|V|X|Z|

2. Dzielimy słowo PASSWORD na pary liter zgodnie z zasadami:
PA-SX-WO-RD

3. Kodujemy słowo za pomocą macierzy i powyrzszych zasad otrzymując szyfrogram: VHQZOKDA.

## Steganografia
Dany jest obrazek i tekst do zaszyfrowania. Tekst zamianiany jest na postać binarną, a obrazek na postać macierzy, gdzie każdy piksel reprezentowany jest przez wartości RGBA. Dwa ostatnie bity każdego koloru oraz kanału alfa zamianiane są na dwa kolejne bity tekstu. Następnie obrazek zapisywany jest w formacie png. Dzięki temu tekst zostaje zakodowany w obrazku, którego zniekształcenie jest niezauważalne.

## Kryptografia wizualna
Dany jest obrazek który zawiera tekst do zaszyfrowania. Na podstawie tego obrazka tworzne są dwa nowe oba posiadają z przezroczystość i są dwa razy większe od oryginalnego obrazka, aby na każdy piksel oryginalnego obrazka odpowiadały cztery piksele nowych obrazków.

W pierwszym losowany jest układ pokseli w ten sposób, aby na czterech pikselach otrzymać dwa białe i dwa czarne piksele.

W drugim sprawdzamy oryginalny obrazek piksel po pikselu. Jeżeli piksel jest biały do obrazka wstawiamy cztery odpowiadające mu piksele w takim samym ułożeniu jak w pierwszym obrazku, natomiast jeżeli jest czarny piksele te są odwrtotne do pierwszego obrazka. 

Możliwe układy czterech pikseli:
|B|C|
|-|-|
|B|C|

|C|B|
|-|-|
|C|B|

|C|B|
|-|-|
|B|C|

|B|C|
|-|-|
|C|B|

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
