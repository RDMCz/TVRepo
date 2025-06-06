# Floorp 11 jakožto náhrada Firefoxu

* Floorp lze stáhnout na <https://floorp.app/en/download>
  * U _Portable_ verze je problematičtější nastavovat Floorp jako výchozí program (prohlížeč webu, pdf, ...); nedoporučuji

* V úvodním wizardovi vybrat _Základní_ template

* Stejně jako ve Firefoxu můžeme z lišty odebrat (jen některé) rušivé prvky

* Podporuje Firefox sync: _Nastavení_ → _Synchronizace_ → _Přihlásit se k synchronizaci..._

## Nastavení

* _Obecné_
  * _Display the Find Bar at the top of the page_

* _Workspaces_
  * _Enable workspaces_

* _Soubory a aplikace_
  * _Ukládat všechny soubory do složky_ / _U každého souboru se zeptat, kam ho uložit_

* _Web Apps_
  * _Enable Web Apps_

* _Vyhledávání_
  * _Vyhledávače_ – zde lze smazat/vypnout vyhledávače, nebo jim přiřadit nové písmeno (např. `g` pro Google)

## about:config

* `gfx.font_rendering.cleartype_params.rendering_mode` → `5` – vyhlazenější font

* `browser.translations.enable` → `false` – vypne podprůměrný Firefox překlad, pro překlad lze použít [TWP - Translate Web Pages](https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/)

* `network.protocol-handler.external.mailto` → `false` – vypne neúprosný popup _Přidat xyz.cz jako aplikaci pro odkazy typu mailto?_

## userChrome.css

* K souboru se dostaneme přes: `about:support` → _Složka s profilem_ → _Otevřít_ → `chrome` → `userChrome.css`

* Vlastní pravidla píšeme pod řádek `/* Please write your custom CSS under this line*/`

```css
/* Skrytí tlačítka 'Sdílet tuto stránku s vaším telefonem' */
#QRCodeGeneratePageAction{display: none !important;}

/* Skrytí tlačítka 'Seznam všech panelů' */
#alltabs-button { display: none !important; }
.titlebar-spacer[type="post-tabs"] { display: none !important; } 
```

---

## Nastavení TWP - Translate Web Pages

* `Ctrl` + `Shift` + `A` nás přesune do správy rozšíření, zde lze nastavovat doplňky (_[…] → Možnosti_)

* _Překlady_
  * _Zobrazit tlačítko pro překlad vybraného textu_

* _Klávesové zkratky_

* _Ostatní_
  * _Zobrazit možnost překladu vybraného textu v kontextové nabídce_
