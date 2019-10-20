# Podstawy ochrony danych

## Szyfr Playfair

### Opis algorytmu
Dana jest macierz 5x5 którą wypełniamy 25 literami bez powtórzeń. Najpierw wpisujemy klucz pomijając powtarzające się litery, a następnie dopełniamy macierz pozostałymi literami alfabetu ( bez polskich liter), przy czym literę J zamieniamy literę na I. Np. klucz KEYWORD:

|K|E|Y|W|O|
|-|-|-|-|-|
|R|D|A|B|C|
|F|G|H|I|L|
|M|N|P|S|T|
|U|V|W|X|Z|

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
|M|N|P|S|T|
|U|V|W|X|Z|

2. Dzielimy słowo PASSWORD na pary liter zgodnie z zasadami:
PA-SX-WO-RD

3. Kodujemy słowo za pomocą macierzy i powyrzszych zasad otrzymując szyfrogram: VHQZOKDA.
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
