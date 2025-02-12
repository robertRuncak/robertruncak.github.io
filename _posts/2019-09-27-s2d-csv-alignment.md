---
title: "S2D Zarovnanie virtuálnych serverov s diskovým úložiskom CSV"
layout: post
---

Cluster Shared Volumes (CSV) je klastrovaný súborový systém vyvinutý spoločnosťou Microsoft, ktorý bol prvýkrát predstavený v systéme Windows Server 2008 R2. CSV prešiel vývojom a dočkal sa niekoľkých vylepšení vo verziách operačného systému Windows Server 2012, 2012 R2, 2016 a aktuálne 2019. Tento blog sa venuje problému CSV v prípade použitia technológie S2D a vačším počtom virtuálnych serverov (VM).


V prípade služby Storage Spaces Direct sú všetky IO metadáta, konkrétne zápisy, presmerované do uzla klastra (cluster node), ktorý vlastní zdieľaný zväzok klastra. Ak sa na danom zväzku používa súborový systém NTFS, zväzok bude v režime "Block Redirected Mode", ak sa použije systém ReFS, zväzok bude v režime "File System Redirected Mode". Toto je možné zistiť pomocou Powershell-u Get-ClusterSharedVolumeState. 

{% highlight ruby %}
Get-ClusterSharedVolume -Name "Cluster Virtual Disk (MIRROR1)" | Get-ClusterSharedVolumeState

BlockRedirectedIOReason      : NotBlockRedirected
FileSystemRedirectedIOReason : FileSystemReFs
Name                         : Cluster Virtual Disk (MIRROR1)
Node                         : SP1HPVR11
StateInfo                    : FileSystemRedirected
VolumeFriendlyName           : MIRROR1
VolumeName                   : \\?\Volume{fe0c0f0f-cbc3-43f6-8816-1e80724fa157}\

BlockRedirectedIOReason      : NotBlockRedirected
FileSystemRedirectedIOReason : FileSystemReFs
Name                         : Cluster Virtual Disk (MIRROR1)
Node                         : SP1HPVR14
StateInfo                    : FileSystemRedirected
VolumeFriendlyName           : MIRROR1
VolumeName                   : \\?\Volume{fe0c0f0f-cbc3-43f6-8816-1e80724fa157}\

BlockRedirectedIOReason      : NotBlockRedirected
FileSystemRedirectedIOReason : FileSystemReFs
Name                         : Cluster Virtual Disk (MIRROR1)
Node                         : SP1HPVR12
StateInfo                    : FileSystemRedirected
VolumeFriendlyName           : MIRROR1
VolumeName                   : \\?\Volume{fe0c0f0f-cbc3-43f6-8816-1e80724fa157}\

BlockRedirectedIOReason      : NotBlockRedirected
FileSystemRedirectedIOReason : FileSystemReFs
Name                         : Cluster Virtual Disk (MIRROR1)
Node                         : SP1HPVR13
StateInfo                    : FileSystemRedirected
VolumeFriendlyName           : MIRROR1
VolumeName                   : \\?\Volume{fe0c0f0f-cbc3-43f6-8816-1e80724fa157}\
{% endhighlight %}

Za predpokladu, že sa pre službu Storage Spaces Direct používa odporúčaný súborový systém ReFS, môže na S2D klastri nastať táto situácia:

![S2D-CSV-alignment](https://user-images.githubusercontent.com/11541025/136854742-e1f8d72a-4192-4885-b302-35b73793b875.png)

V tomto príklade ide o 4-uzlový klaster S2D so štyrmi CSV, ktoré používajú technológiu troj-cestného zrkadlenia (three-way mirror).

S2D Node1 vlastní CSV1 a sú na ňom spustené 3 virtuálne servery. S2D Node2 vlastní CSV2. Hneď ako VM2, ktorý má uložené disky na CSV2, začne robiť zápisy, všetky IO operácie budú presmerované do uzla klastra, ktorý vlastní CSV, kde sú uložené disky VM2. Jednoducho povedané: Všetky IO operácie typu zápis pre VM2 musia preplávať na S2D Node2, než bude možné ich potvrdiť. To isté platí aj pre VM3.

Toto určite nebude problém pre jeden virtuálny server. Ale v prípade, že v klastri bežia stovky virtuálnych serverov, kde veľká časť virtuálnych serverov a ich IO operácie musia preplávať k vlastníkovi CSV, na ktorom sa nachádzajú disky VM, to už môže byť pre IO operácie doručované cez sieť veľký problém.

IO operácie sú citlivé na latenciu (teda čas kedy vznikla IO operácia a kedy bola táto IO operáca vybavená) a plávanie medzi vlastníkmi CSV (teda skákanie v sieti) to ešte nezlepšuje. Takisto táto činnosť zaberá určitú šírku sieťového pásma a ďalšie cykly CPU na danom klastri - hypervízore.

To je tiež dôvod, prečo jeden z nástrojov pre testovanie výkonu S2D [VMfleet][vmfleet-github] vyrovnáva virtuálne servery s CSV, kde sú uložené disky VM, s hypervízorom, na ktorých sú tieto VM spustené. Dôvod je jednoduchý - zistiť čo samotný klaster dokáže pri najnižších vstupných nákladoch.

Čo s tým? Zarovnať VM -> CSV -> HYPER-V

Ako na to? Vďaka Darryl van der Peijl je možné použiť napríklad powershell skript [Align-VMsWithStorage.ps1][align-github]

Zdroj: [Darryl van der Peijl Blog][zdroj-darryl], [Microsoft Failover Cluster Blog][zdroj-microsoft]

[vmfleet-github]: https://github.com/microsoft/diskspd/tree/master/Frameworks/VMFleet
[align-github]: https://github.com/DarrylvanderPeijl/Align-VMsWithStorage
[zdroj-darryl]:https://www.darrylvanderpeijl.com
[zdroj-microsoft]:https://techcommunity.microsoft.com/t5/failover-clustering/understanding-the-state-of-your-cluster-shared-volumes/ba-p/371889

