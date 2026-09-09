# Kontrola podpisów w PDF

Strona sprawdza, które pliki PDF mają podpis elektroniczny, a które nie.
Cała praca odbywa się w przeglądarce: **dokumenty nie są nigdzie wysyłane ani zapisywane**.

Adres: https://przemyslawwywigaczcyfrowe.github.io/podpisy-pdf/

## Jak używać

Wrzuć pliki albo cały folder. Pliki bez podpisu stają na górze listy.
Wynik można zapisać do arkusza jako CSV.

## Jak rozpoznaje podpis

Podpis jest wtedy, gdy w pliku stoi `/ByteRange`, a obok niego `/Contents<...>`
wypełnione danymi certyfikatu, a nie samymi zerami.

Kluczowa pułapka: dokument tylko *przygotowany* do podpisania ma pełną strukturę
podpisu razem z `/SubFilter` i `/SigFlags 3`, ale miejsce na certyfikat wypełnione
zerami. Samo szukanie `/ByteRange` albo `/Type /Sig` dałoby tam fałszywe
„podpisany". Takie pliki są tu osobną grupą „puste pole podpisu" i liczą się jako
brak podpisu.

Znacznik czasu (`/DocTimeStamp`, `ETSI.RFC3161`) jest wykazywany osobno, bo nie mówi,
kto zatwierdził dokument.

Nazwa podpisującego pochodzi z certyfikatu: numer seryjny z SignerInfo dopasowany
do certyfikatu w strukturze CMS. Pole `/Name` z dokumentu służy tylko jako rezerwa,
bo w zaszyfrowanym PDF zawiera nieczytelne dane.

## Czego to nie sprawdza

Narzędzie nie ocenia, czy podpis jest ważny i czy wystawca certyfikatu jest zaufany.
Do tego służy Adobe Reader albo weryfikator podpisu. Tutaj odpowiedź brzmi tylko:
podpis jest albo go nie ma. Skan odręcznego podpisu wychodzi jako brak podpisu,
bo dla pliku to zwykły obrazek.

## Na czym sprawdzone

25 dokumentów z publicznego zestawu testowego ETSI (repozytorium `esig/dss`),
w tym pliki z pustym polem podpisu, z 25 podpisami, z samym znacznikiem czasu
i dokument zaszyfrowany. Zgodność z niezależnym wynikiem odniesienia: 25 na 25.

## Licznik uzysku

Na dole strony stoi licznik sprawdzonych plików i przeliczony na nich czas
(30 sekund na dokument). Licznik trzyma tylko liczbę, bez nazw i bez treści plików.
