# Firefox – _Najít na stránce..._ ve vrchní části prohlížeče

Do [zprovozněného souboru userChrome.css](./userchrome.MD) připište:

```css
/* Move Find Bar above the page */
.browserContainer > findbar {
    /*Do verze 113*/
    /*-moz-box-ordinal-group: 0;*/
    
    /*Od verze 113*/
    order: -1 !important;
}
```

Soubor uložte a restartujte prohlížeč.
