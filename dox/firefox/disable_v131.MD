# Firefox – Vypnutí novinek z verze 131

## Tlačítko _Seznam všech panelů_

Do [zprovozněného souboru userChrome.css](./userchrome.MD) připište:

```css
/* Disable List all tabs button */
#alltabs-button { display: none !important; }
.titlebar-spacer[type="post-tabs"] {  display: none !important; }
```

Soubor uložte a restartujte prohlížeč.

## Náhledy panelů

Přejděte do `about:config` a nastavte hodnotu `browser.tabs.hoverPreview.enabled` na `false`.
