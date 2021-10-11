---
title: "S2D Zarovnanie virtuálnych serverov s diskovým úložiskom CSV"
layout: post
---

Cluster Shared Volumes (CSV) je klastrovaný súborový systém vyvinutý spoločnosťou Microsoft, ktorý bol prvýkrát predstavený v systéme Windows Server 2008 R2. CSV prešiel vývojom a dočkal sa niekoľkých vylepšení vo verziách operačného systému Windows Server 2012, 2012 R2, 2016 a aktuálne 2019. Tento blog sa venuje problému CSV v prípade použitia technológie S2D a vačším počom virtuálnych serverov (VM).

V prípade služby Storage Spaces Direct sú všetky IO metadáta, konkrétne zápisy, presmerované do uzla klastra (cluster node), ktorý vlastní zdieľaný zväzok klastra. Ak sa na danom zväzku používa súborový systém NTFS, zväzok bude v režime "Block Redirected Mode", ak sa použije systém ReFS, zväzok bude v režime "File System Redirected Mode". Toto je možné zistiť pomocou powershell-u Get-ClusterSharedVolumeState. 

{% highlight powershell %}
PS C:\> get-ClusterSharedVolume -Name "Cluster Virtual Disk (MIRROR1)" | Get-ClusterSharedVolumeState


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

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.


To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
