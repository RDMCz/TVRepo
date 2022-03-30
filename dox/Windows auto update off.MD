# Windows 10 Pro – Vypnutí automatického stahování a instalace aktualizací

1. Otevřeme _Editor místních zásad skupiny_
   * Např. pomocí `Ctrl` + `R` a spuštěním `gpedit.msc` nebo vyhledáním výrazu _Upravit zásady skupiny_

2. Navigujeme do _Konfigurace počítače_ > _Šablony pro správu_ > _Součásti systému Windows_ > _Windows Update_

3. Zde rozklikneme položku _Konfigurace automatických aktualizací_

4. Přepneme stav z _Není nakonfigurováno_ na _Povoleno_

5. V drop-down listu vybereme možnost _2 – Upozorňovat na stažení a automatickou instalaci_

* V nastavení v sekci Windows Update by nyní mělo být napsáno _*Některá nastavení určuje vaše organizace._

* Windows nyní nebude automaticky stahovat aktualizace po jejich nalezení, ale pošle upozornění typu _Potřebujete určité aktualizace_. Kliknutí na toto upozornění otevře Windows Update v nastavení, kde uživatel může (a nemusí) stažení a instalaci potvrdit

* Jakmile jsou nalezeny nějaké aktualizace, další už se nehledají. Tzn., že pokud jsou nalezené aktualizace nainstalovány až po delší době, je dobré po jejich instalaci spustit ruční vyhledávání
