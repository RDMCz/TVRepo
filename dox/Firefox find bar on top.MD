# Firefox – _Najít na stránce..._ ve vrchní části prohlížeče

1. Přejděte do `about:config` a nastavte hodnotu `toolkit.legacyUserProfileCustomizations.stylesheets` na `true`

2. Ve složce s profilem vytvořte podsložku s názvem `chrome`
   * Do složky s profilem se lze dostat přes `about:support` – kolonka _Složka s profilem_

3. Ve složce `<profile>\chrome` vytvořte soubor `userChrome.css` a do něj napište:

```css
/* Move Find Bar above the page */
.browserContainer > findbar {
    /*Do verze 113*/
    /*-moz-box-ordinal-group: 0;*/
    
    /*Od verze 113*/
    order: -1 !important;
}
```
