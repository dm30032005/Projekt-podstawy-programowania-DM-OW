# Podstawy Programowania – Projekt

**Autorzy:** Oliwier Wilkosz, Daniel Macander
**Kierunek**: Inżynieria danych, I rok

---

## Zadanie 3 – **Niesforne dane**

### Cel zadania
Celem zadania jest przekształcenie pliku tekstowego z jedną kolumną danych liczbowych na format trzykolumnowy z nagłówkami, co umożliwi łatwy import do arkusza kalkulacyjnego i sporządzenie wykresów.

### Polecenia

```bash
echo -e "x \ty \tz" > D:/DM/dane_poprawione.txt
$ tr -d '\r' < D:/DM/dane.txt | paste - - - >> D:/DM/dane_poprawione.txt
```

---

## Zadanie 4 – **Dodawanie poprawek**

### Cel zadania
Celem zadania jest porównanie dwóch wersji listy słów, przygotowanie łatki aktualizującej plik oryginalny do wersji poprawionej wraz z instrukcją obsługi, a następnie zweryfikowanie poprawności całego procesu poprzez porównanie sum kontrolnych MD5 obu plików.

### Polecenia

```bash
cd ~/zadania
diff -u lista.txt lista-pop.txt > latka.patch
patch lista.txt < latka.patch
md5sum lista.txt md5sum lista-pop.txt
```
---

## Zadanie 5 – **Z CSV do SQL i z powrotem**

### Cel zadania
Celem zadania jest opracowanie metod dwukierunkowej konwersji danych: najpierw z pliku CSV do skryptu z zapytaniami INSERT INTO bazy SQL, a następnie ze skryptu SQL z powrotem do pliku CSV wraz z jednoczesną zmianą dokładności stempla czasu Unix z milisekund na sekundy.

### Polecenia

```bash
awk -F';' 'NR > 1 {print "INSERT INTO stepsData (time, intensity, steps) VALUES (' $1 ' , ' $2 ' , ' $3 ');"}' D:/DM/steps-2sql.csv > D:/DM/steps-2sql.sql
echo "dataTime; steps; synced" > D:/DM/steps-2csv.csv
sed -n 's/.*VALUES (\([0-9]*\)[0-9]\{3\}, *\([0-9]*\), *\([0-9]*\));/\1;\2;\3/p' D:/DM/steps-2sql.sql >> D:/DM/steps-2csv.csv
```

---

## Zadanie 6 – **Marudny tłumacz**

### Cel zadania
Celem zadania jest przygotowanie plików pomocniczych do lokalizacji oprogramowania poprzez zdublowanie i zakomentowanie linii z angielskiego pliku JSON oraz wyodrębnienie do osobnego pliku wyłącznie nowych fraz, które pojawiły się w zaktualizowanej wersji językowej aplikacji.

### Polecenia

```bash
cd ~/zadania
sed -E 's/^([[:space:]]*".*":.*)$/\/\/ \1\n\1/' en-7.2.json5 > pl-7.2.json5
grep -Fvxf en-7.2.json5 en-7.4.json5 | sed -E 's/^([[:space:]]*".*":.*)$/\/\/ \1\n\1/' > pl-7.4.json5
```

---

## Zadanie 7 – **Fotografik gamoń**

### Cel zadania
Celem zadania jest rozpakowanie zdjęć z dwóch osobnych archiwów ZIP, zmiana ich formatu na JPG, zmniejszenie ich wysokości do 720 pikseli przy ustawieniu rozdzielczości na 96x96 DPI, a następnie ponowne spakowanie wszystkich przetworzonych plików do jednego wspólnego archiwum ZIP.

### Polecenia

```bash
cd ~/Zadania

unzip -q kopie-1.zip -d pliki_tymczasowe/
unzip -q kopie-2.zip -d pliki_tymczasowe/

cd ~/pliki_tymczasowe

magick mogrify -format jpg -resize x720 -density 96 -units PixelsPerInch *.*

zip -q ~/Zadania/fotografie-gotowe.zip *.jpg
```

---
## Zadanie 8 – **Wszędzie te PDF-y**

### Cel zadania
Celem zadania jest wygenerowanie z gotowych plików graficznych gotowego do druku dokumentu PDF w formacie A4 zawierającego portfolio, w którym na każdej stronie umieszczone jest osiem podpisanych nazwą pliku fotografii w układzie po dwie w czterech wierszach.

### Polecenia

```bash
cd ~/Zadania

mkdir -p pdf_strony

montage pliki_tymczasowe/*.jpg \
  -tile 2x4 \
    -geometry 380x260+10+10 \
      -gravity center \
        -pointsize 18 \
          -label '%f' \
            pdf_strony/strona-%03d.jpg

            magick pdf_strony/strona-*.jpg portfolio.pdf

            zip -q fotografie-gotowe.zip portfolio.pdf
            ```

            ---
            ## Zadanie 9 – **Porządki w kopiach zapasowych**

            ### Cel zadania
            Celem zadania jest uporządkowanie plików kopii zapasowych poprzez automatyczne pogrupowanie ich i przeniesienie do nowo utworzonej, hierarchicznej struktury katalogów podzielonej na lata oraz podkatalogi odpowiadające poszczególnym miesiącom.

            ### Polecenia

            ```bash
            mkdir -p kopie
            cd kopie
            unzip ./kopie-1.zip
            unzip ./kopie-2.zip
            for f in *.zip; do [ -e "$f" ] || continue; r=$(echo "$f" | cut -d- -f1); m=$(echo "$f" | cut -d- -f2); mkdir -p "$r/$m"; cp "$f" "$r/$m/"; done
            rm *.zip
            ```

            ---
            ## Zadanie 10 – **Galeria dla grafika**

            ### Cel zadania
            Celem zadania jest stworzenie internetowej galerii zdjęć poprzez wielokrotne wstawienie wskazanego fragmentu kodu HTML do szablonu i uzupełnienie go o nazwy plików graficznych z poprzedniego zadania.

            ### Polecenia

            ```bash
            mkdir -p galeria

            cp pliki_tymczasowe/archwall_dark_blue.png galeria/blue.png
            cp pliki_tymczasowe/archwall_dark_orange.png galeria/orange.png
            cp pliki_tymczasowe/archwall_dark_toxic.png galeria/toxic.png
            cp pliki_tymczasowe/archwall_dark_yellow.png galeria/yellow.png

            cd galeria

            for plik in *.png; do
              [ -e "$plik" ] || continue
                echo '<div class="responsive">' >> index.html
                  echo '  <div class="gallery">' >> index.html
                    echo "    <a target=\"_blank\" href=\"$plik\">" >> index.html
                      echo "      <img src=\"$plik\">" >> index.html
                        echo '    </a>' >> index.html
                          echo "    <div class=\"desc\">$plik</div>" >> index.html
                            echo '  </div>' >> index.html
                              echo '</div>' >> index.html
                                echo >> index.html
                                done

                                echo '<div class="clearfix"></div></body></html>' >> index.html

                                cd ~/Zadania
                                zip -q -r galeria-gotowa.zip galeria
                                ```
