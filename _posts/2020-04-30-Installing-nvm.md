---
layout: post
title:  "NVM, Gampang Ganti Versi Node Dalam Sekejap"
image: ''
date:   2020-04-30 00:06:31
tags:
- tools
description: ''
categories:
- node
serie: learn
---

<p class="music-read"><a href="spotify:track:7xQAfvXzm3AkraOtGPWIZg">Music for reading(spotify)</a></p>

<img src="/assets/img/nvm/nodejs.gif">

## NVM ? Apaan Sih itu ?

NVM adalah Node Version Manager, bash script sederhana untuk mengatur versi nodejs yg aktif, dan  di design untuk di install per-user dan per-shell. nvm juga bisa di gunakan di `POSIX compliant shell (sh, dash, ksh, zsh, bash)`. Terkhusus di platforms: unix, macOS, and windows WSL. 

## NVM, Buat apa ?

Saya sering sekali harus berpindah pindah versi node karena di haruskan untuk menjalankan angularjs( versi lama ) untuk tujuan maintenance dan sementara itu saya sedang melakukan development memakai angular(versi terbaru) dan keduanya itu memakai versi node yg bebeda, angularjs mengharuskan memakai versi node 9 dan angular versi terbaru mengharuskan untuk memakai versi node 10 keatas. terbayang kan jika tidak ada nvm ? saya harus install dan uninstall versi node berbeda setiap akan melakukan development pada aplikasi yg berbeda `jaman`. <br>
Dan disinilah nvm sebagai `game changer` di development environmnet node, Kita bisa memiliki beberapa versi node dan gampang untuk berpindah dari versi satu ke versi lainnya. 


## Install NVM.

Menginstall NVM sangatlah mudah, bisa juga di baca disini <a target="_blank" href="https://github.com/nvm-sh/nvm#installation-and-update"> Dokumentasi github</a> <br>
ada beberapa langkah menginstall NVM:
### Langkah 1
Langkah pertama sangat simple kita bisa memililih menggunakan curl atau wget.<br>
Curl command:
{% highlight javascript %}
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
{% endhighlight %}

Wget command:
{% highlight javascript %}
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
{% endhighlight %}

### Langkah 1.1 
Pastikan nvm telah terinstall dengan baik dengan command berikut.<br>
{% highlight javascript %}
[~] $: command -v nvm
{% endhighlight %}

jika terinstall maka akan seperti berikut:


<figure class="foto-legenda">
	<img src="{{ "/assets/img/nvm/nvm1.png"}}" alt="">
	<figcaption> 
	</figcaption>
</figure>

Jika Belum muncul `nvm` atau ada error lainnya bisa langsung saja di baca dokumentasi resmi dari nvm <a target="_blank" href="https://github.com/nvm-sh/nvm#installation-and-update"> Dokumentasi github</a> <br>

### Langkah 2

Tambahkan Directory Path nvm ke shell profile yaitu di file `~/.zshrc` atau di `~/.bashrc` dengan menambahkan path sebagai berikut di akhir file:

{% highlight javascript %}
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
{% endhighlight %}

lalu ulangi lagi command `command -v nvm` untuk memastikan instalasi terpasang dengan baik.

## Menggunakan NVM
menggunakan nvm sangat mudah, saya akan menunjukan beberapa command dari nvm

### Menampilkan Semua versi LTS nodejs
Dengan command `nvm ls-remote` kita bisa melihat semua versi LTS nodejs

<figure class="foto-legenda">
	<img src="{{ "/assets/img/nvm/nvm2.png"}}" alt="">
	<figcaption> 
	</figcaption>
</figure>

### Menampilkan versi nodejs yg terinstall

Dengan command `nvm ls ` menampilkan versi nodejs yg terinstall di local kita.
<figure class="foto-legenda">
	<img src="{{ "/assets/img/nvm/nvm3.png"}}" alt="">
	<figcaption> 
	</figcaption>
</figure>

### Menginstall nodejs versi tertentu

Menggunakan command `nvm install <SPECIFIC_NODE_VERSION>` kita bisa menginstall versi nodejs dengan versi tertentu.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/nvm/nvm4.png"}}" alt="">
	<figcaption> 
	</figcaption>
</figure>

### Menggunakan Nodejs Versi tertentu

Kita bisa berpindah dari versi nodejs satu ke versi lainnya dengan menggunakan command `nvm use node <SPECIFIC_NODE_VERSION>`.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/nvm/nvm5.png"}}" alt="">
	<figcaption> 
	</figcaption>
</figure>

## Kesimpulan

Versi Nodejs adalah sesuatu yang jarang kita pikirkan dalam membuat sebuah aplikasi sebelum itu menjadi masalah. apalagi ketika kita harus menggunakan beberapa versi sekaligus dalam satu laptop. nvm memudahkan kita untuk mengatur versi nodejs yg kita akan pakai dengan sangat mudah sekali.