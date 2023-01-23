---
title: "Exchange Online - Microsoft Defender for Office 365"
layout: post
---

Microsoft Defender for Office 365 je technológia, ktorá je určená pre väčšinu služieb a aplikácii Office 365 a jej hlavnou úlohou je kontrolovať súbory na prítomnosť spustiteľného škodlivého kódu a odkazov na internetové stránky. Pre Exchange Online sú zaujímavé technológie Safe Links a Safe Attachments.

Viac info na stránke [Microsoft Defender for Office 365 service description][zdroj-ms-1]


Čo vlastne tieto technológie robia? Jednoducho kontrolujú odkazy na internetové stránky a doručované súbory. Čo stojí za zmienku je spôsobo akým to Microsoft robí. Všetky odkazy a súbory sú pri doručení skutočne otestované v zabezpečenom a izolovanom priestore. Každý odkaz je napríklad otvorený, preklikaný, posunie sa čas aby sa predišlo tomu, že dôjde k spusteniu škodlivého kódu až po neakej dobe. To isté platí aj pre súbory. Súbory sú otvárané, robotický test ich prekliká, robot skúša scrollovanie obrazovky a vykonáva rôzne náhodne vybrané akcie, testujú sa odkazy v súbore, posúva sa čas či náhodou nie je vykonanie škodlivého kódu naplánované s oneskorením. Ak sú všetky tieto kontroly a akcie vykonané, následne je odkaz alebo súbor doručený do mailovej schránky. 

Nevýhody - spomalenie doručovania príloh a cena. 

Spomalenie doručovania príloh je daň za celý proces kontroly. Toto je možné riešiť zapnutím funkcie "Dynamic Delivery".

Viac info na stránke [Safe Attachments in Microsoft Defender for Office 365][zdroj-ms-2]

Cena. Bohužiaľ táto technológia nie je lacná ani pri konkurencii. Microsoft Defender for Office 365 je dostupná v najvyšších plánoch Microsoft 365. Dostupné sú predplatné (subscription) Plan 1 a Plan 2, ak je potrebné túto funkcionalitu priplatiť k nižším predplatným. Samozrejme sú táto technológie súčasťou rôznych balíkov. 

Viac info na stránke [Microsoft Defender for Office 365][zdroj-ms-3]

[zdroj-ms-1]: https://learn.microsoft.com/en-us/office365/servicedescriptions/office-365-advanced-threat-protection-service-description
[zdroj-ms-2]: https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/safe-attachments-about?view=o365-worldwide
[zdroj-ms-3]: https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/defender-for-office-365?view=o365-worldwide
