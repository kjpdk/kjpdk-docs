# 🏸 Badminton Manager

Dette program er et interaktivt, terminal-baseret værktøj bygget i **Python 3.13** til at spore og beregne match-statistikker i min lokale badmintonklub mod **Thomas**, **Kim H** og **Kim P**.

---

## 🚀 Hurtig start (Hvordan bruges det?)

For at køre programmet i mit **KJP-Lab**, skal jeg blot navigere til projektmappen og bruge `uv` til at starte det:

```bash
cd ~/Repos/[github.com/kjpdk/kjpdk-lab/dev/python/PyProjekt-001](https://github.com/kjpdk/kjpdk-lab/dev/python/PyProjekt-001)
uv run main.py
```

Når programmet starter, mødes man af en interaktiv hovedmenu:

1. **Vis aktuel statistik:** Beregner og udskriver en flot tabel med win-rate og point.
2. **Registrer et nyt kampresultat:** Spørger efter modstanderens navn samt point, gemmer det i datafilen og viser den opdaterede tabel med det samme.
3. **Luk programmet:** Afslutter applikationen rent.

## 📂 Dataformat (`kampe.csv`)

Al data gemmes eksternt i en simpel tekstfil kaldet `kampe.csv`. Det betyder, at jeg også kan åbne og redigere mine resultater direkte i f.eks. Google Sheets eller min Obsidian Zettelkasten. Filen er opbygget således:

```markdown
modstander,egne_point,modstander_point
Thomas,21,18
Kim H,19,21
Kim P,21,15
```

## 🧠 Sådan fungerer koden (Pædagogisk gennemgang)

Programmet er opdelt i små, overskuelige funktioner, som har hver deres ansvarsområde:

- `indlaes_kampe_fra_csv(filnavn):` Åbner datafilen og bruger Pythons indbyggede `csv.DictReader`. Den omdanner automatisk hver række til en ordbog (dictionary) og konverterer pointene fra tekst til rigtige heltal (`int`), så vi kan regne på dem.
- `gem_kamp_til_csv(...):`` Åbner filen i `a` (append) tilstand. Det betyder, at den hopper helt ned i bunden af filen og tilføjer det nye resultat uden at røre ved eller slette dine gamle kampe.
- `beregn_statistik(kampe):` Løber alle registrerede kampe igennem med en løkke (`for kamp in kampe`). Den tæller, hvor mange gange dine point var højere end modstanderens, og beregner din samlede sejsprocent (Win Rate) med formlen:` (vundet / antal_kampe) * 100`.

## 💻 Komplet Kildekode (`main.py`)

Her er den fulde kode, som den er implementeret i mit Git-arkiv:

```python
import csv
from tabulate import tabulate


def indlaes_kampe_fra_csv(filnavn):
    kampe = []
    try:
        with open(filnavn, mode="r", encoding="utf-8") as fil:
            csv_laeser = csv.DictReader(fil)
            for raekke in csv_laeser:
                kampe.append(
                    {
                        "modstander": raekke["modstander"],
                        "egne_point": int(raekke["egne_point"]),
                        "modstander_point": int(raekke["modstander_point"]),
                    }
                )
    except FileNotFoundError:
        pass
    return kampe


def gem_kamp_til_csv(filnavn, modstander, egne_point, modstander_point):
    with open(filnavn, mode="a", encoding="utf-8", newline="") as fil:
        feltnavne = ["modstander", "egne_point", "modstander_point"]
        csv_skriver = csv.DictWriter(fil, fieldnames=feltnavne)

        if fil.tell() == 0:
            csv_skriver.writeheader()

        csv_skriver.writerow(
            {
                "modstander": modstander,
                "egne_point": egne_point,
                "modstander_point": modstander_point,
            }
        )


def beregn_statistik(kampe):
    if not kampe:
        return None

    samlet_vundet = 0
    samlet_point_vundet = 0
    samlet_point_tabt = 0

    for kamp in kampe:
        samlet_point_vundet += kamp["egne_point"]
        samlet_point_tabt += kamp["modstander_point"]

        if kamp["egne_point"] > kamp["modstander_point"]:
            samlet_vundet += 1

    antal_kampe = len(kampe)
    win_rate = (samlet_vundet / antal_kampe) * 100

    return {
        "antal_kampe": antal_kampe,
        "vundet": samlet_vundet,
        "win_rate": win_rate,
        "point_vundet": samlet_point_vundet,
        "point_tabt": samlet_point_tabt,
    }


def vis_statistik(filnavn):
    badminton_kampe = indlaes_kampe_fra_csv(filnavn)
    stats = beregn_statistik(badminton_kampe)

    if stats is None:
        print("\nℹ Der er ikke registreret nogen kampe i systemet endnu.")
        return

    tabel_data = [
        ["Antal Kampe Spillet", stats["antal_kampe"]],
        ["Kampe Vundet", stats["vundet"]],
        ["Sejsprocent (Win Rate)", f"{stats['win_rate']:.1f}%"],
        ["Point Scoret (Egne)", stats["point_vundet"]],
        ["Point Tabt (Modstander)", stats["point_tabt"]],
    ]

    print(f"\n=== KJP-LAB BADMINTON STATISTIK (Hentet fra {filnavn}) ===")
    print(tabulate(tabel_data, headers=["Metrik", "Resultat"], tablefmt="fancy_grid"))


def modtag_heltal(ledetekst):
    while True:
        try:
            return int(input(ledetekst))
        except ValueError:
            print("⚠ Fejl: Du skal indtaste et gyldigt heltal. Prøv igen.")


def tilfoej_ny_kamp(filnavn):
    print("\n--- REGISTRER NY BADMINTON KAMP ---")
    modstander = input("Indtast modstanderens navn: ").strip()

    if not modstander:
        print("⚠ Fejl: Modstanderens navn må ikke være tomt.")
        return

    egne_point = modtag_heltal("Indtast dine point: ")
    modstander_point = modtag_heltal("Indtast modstanderens point: ")

    gem_kamp_til_csv(filnavn, modstander, egne_point, modstander_point)
    print(f"✔ Kampen mod {modstander} blev gemt!")


def main():
    filnavn = "kampe.csv"

    while True:
        print("\n=================================")
        print("    KJP-LAB BADMINTON MANAGER    ")
        print("=================================")
        print("1. Vis aktuel statistik")
        print("2. Registrer et nyt kampresultat")
        print("3. Luk programmet")

        valg = input("Vælg en mulighed (1-3): ").strip()

        if valg == "1":
            vis_statistik(filnavn)
        elif valg == "2":
            tilfoej_ny_kamp(filnavn)
            vis_statistik(filnavn)
        elif valg == "3":
            print("\nTak for i dag! Hav en god træning i klubben.")
            break
        else:
            print("⚠ Ugyldigt valg. Tast venligst 1, 2 eller 3.")


if __name__ == "__main__":
    main()
```
