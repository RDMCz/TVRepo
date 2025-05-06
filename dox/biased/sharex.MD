# ShareX jako náhrada za  `Shift` + `Win` + `S`

## Vypnutí výchozího `Shift` + `Win` + `S`

Vytvořte soubor s příponou `.reg` a tímto obsahem:

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced]
"DisabledHotkeys"="S"
```

Soubor otevřete a potvrďte změnu v registru. Restartujte počítač.

## Nastavení ShareX (16.1.0)

Z <https://getsharex.com/downloads> stáhněte ShareX a program spusťte. V nastavení změňte:

* _Application settings_
  * _General_
    * Zde lze vypnout akce, které se stanou při kliknutí na ikonku v trayi (_None_)
    * _Automatically check for updates_
  * _Integration_
    * _Run ShareX when Windows starts_
* _Task settings_
  * _Notifications_
    * _Play sound after caption is made_
    * _Play sound after task is completed_
    * _Show toast notification after task is completed_
  * _Region capture_
    * Zde lze vypnout akce, které se stanou při stisknutí tlačítek na myši (_Do nothing_)
    * _Detect window regions and allow cursor hover capture_
    * _Show position and size info_
    * _Show magnifier near cursor_
  * _Advanced_
    * _RegionCaptureDisableAnnotation_ (vypne horní toolbar během region capture)
* _Hotkey settings_
  * _Capture region_ změňte na `Shift` + `Win` + `S`
  * Ostatní funkce lze smazat pomocí _Remove_

Animace rámečku výběru lze vypnout pouze v horním toolbaru během region capture, tudíž je pro jejich vypnutí nutné dočasně nastavit  _RegionCaptureDisableAnnotation_ na _False_.
