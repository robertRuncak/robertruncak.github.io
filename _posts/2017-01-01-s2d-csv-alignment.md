---
title: "S2D Zarovnanie virtuálnych serverov s diskovým úložiskom CSV"
layout: post
---

Cluster Shared Volumes (CSV) je klastrovaný súborový systém vyvinutý spoločnosťou Microsoft, ktorý bol prvýkrát predstavený v systéme Windows Server 2008 R2. CSV prešiel vývojom a dočkal sa niekoľkých vylepšení vo verziách operačného systému Windows Server 2012, 2012 R2, 2016 a aktuálne 2019. Tento blog sa venuje problému CSV v prípade použitia technológie S2D a vačším počom virtuálnych serverov (VM).

V prípade služby Storage Spaces Direct sú všetky IO metadáta, konkrétne zápisy, presmerované do uzla klastra (cluster node), ktorý vlastní zdieľaný zväzok klastra. Ak sa na danom zväzku používa súborový systém NTFS, zväzok bude v režime "Block Redirected Mode", ak sa použije systém ReFS, zväzok bude v režime "File System Redirected Mode". Toto je možné zistiť pomocou powershell-u Get-ClusterSharedVolumeState. 

{% highlight powershell %}
PS C:\\> Get-ClusterSharedVolume -Name "Cluster Virtual Disk (MIRROR1)" | Get-ClusterSharedVolumeState

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

