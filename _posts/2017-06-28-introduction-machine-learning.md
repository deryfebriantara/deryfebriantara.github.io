---
layout: post
title:  "Machine Learning Say 'Hello World'"
image: ''
date:   2017-06-28 00:06:31
tags:
- basic machine learning
description: ''
categories:
- Artificial Intelligence
serie: learn
---

<p class="music-read"><a href="spotify:track:5KsLlcmWDoHUoJFzRw14wD">Music for reading(spotify)</a></p>

<img src="https://media.giphy.com/media/cGkIpDxembJWU/giphy.gif">

## Machine Learning ? what ?

Take it easy guys, we are not trying to create an evil AI terminator by this time, we only will learn basic concept of machine learning. in this article/tutorial or whatever you call it, we will code only six line python script and you will be able to create the simplest machine learning to solve problem.<br>
FYI Machine Learning is subfield of Artificial Intelligence . <a target="_blank" href="https://id.wikipedia.org/wiki/Kecerdasan_buatan">more info click here</a>

## Machine Learning, why?

Kenapa kita harus menggunakan machine learning untuk memberikan solusi terhadap permasalahan kita? sebagai contoh kita mempunyai masalah untuk mengetahui jenis buah berdasarkan foto. kita bisa saja menulis kode manual dan memberikan kondisi/rule tertentu untuk mengetahui jenis buah berdasarkan warna atau bentuk buahnya, `thats work fine for simple images`. tetapi di dunia nyata bentuk dan warna buah tidak selalu sama, bagaimana jika fotonya hitam putih atau jenis buah tertentu tidak ada dalam kode yang kita buat kondisi/rule-nya? dan bagaimana jika kita menambah jenis buah? tentu saja kita harus mengulangi semua kode yang kita buat manual, cape kan?<br>
disitulah machine learning bisa membantu kita untuk memberikan solusi terhadap masalah kita tadi, karena machine learning dapat memberikan kondisi/rule ( classifier )  dengan cara mencari pattern terhadap data yang kita berikan atau nanti akan kita sebut dengan training data, tanpa kita harus menulisnya dengan manual. dan teknik ini dinamakan supervised learning. <br>
`supervised learning : create a classifier by finding pattern in examples`.

<img src="https://octodex.github.com/images/codercat.jpg" alt="">

## let's buckle up.

kita akan menggunakan `scikit learn` untuk membantu kita, go ahead and <a target="_blank" href="http://scikit-learn.org/stable/">install here </a>, saya menggunakan anaconda2 untuk menginstall scikit learn, dan berguna juga nanti jika ingin menginstall `tensorflow`. lebih mudah dan praktis menggunakan anaconda2.

### the goal!

kita akan mampu untuk menentukan jenis buah berdasarkan ciri-ciri dari buah tersebut<br>
sebagai contoh kita akan menentukan jenis buah `durian` atau `rambutan`.



###  are your scikit learn installed properly?

sesudah kita menginstall scikit learn pastikan scikit learn telah terinstall dengan baik dengan cara:

{% highlight javascript %}
[~] $: touch basic-machine-learning.py
[~] $: vim basic-machine-learning.py
{% endhighlight %}

lalu tulis di file `basic-machine-learning.py`
{% highlight javascript %}

import sklearn

{% endhighlight %}

save, exit dan jalankan dengan perintah: 

{% highlight javascript %}

[~] $: python basic-machine-learning.py

{% endhighlight %}

jika tidak ada error maka scikit learn telah terinstall dengan baik, lanjut... gan.<br>
selanjutnya kita akan membuat training data untuk menentukan buah durian dan rambutan, training data buah ini kita akan isi dengan ciri-ciri buah, `berat`, `texture` , dan `nama_buah`.<br>
<i>nb :kita akan menggunakan input console terlebih dahulu, in future article I will show how to using image classifier ;)</i>


<figure class="foto-legenda">
	<img src="{{ "/assets/img/machine-learning/table.png"}}" alt="">
	<figcaption> <p>berat dan texture kita akan sebut dengan <b>features </b> dan nama buah kita akan sebut dengan <b>label</b></p>
	</figcaption>
</figure>

###  let's code!

{% highlight javascript %}

from sklearn import tree
features = [[60, "berambut"], [1200, "berduri"], [50, "berambut"], [1500, "berduri"] ]
labels = ["rambutan", "durian", "rambutan", "durian"]

{% endhighlight %}

saya akan mempersimple kode dengan mengganti value dalam variable features `"berambut" = 0 ` dan `"berduri" = 1` , begitu juga dengan labels `"rambutan" = 0` dan `"durian" = 1` 

{% highlight javascript %}

from sklearn import tree
features = [[60, 0], [1200, 1], [50, 0], [1500, 1] ]
labels = [0, 1, 0, 1]

{% endhighlight %}

kita akan mengunakan decision tree untuk menentukan jenis buah nanti berdasarkan input dengan manambahkan method `DecisionTreeClassifier()` dan mentraining classifier dengan method `fit()` : 

{% highlight javascript %}

from sklearn import tree
features = [[60, 0], [1200, 1], [50, 0], [1500, 1] ]
labels = [0, 1, 0, 1]
clf = tree.DecisionTreeClassifier()
clf = clf.fit(features, labels)

{% endhighlight %}

dan pada akhirnya kita dapat menentukan jenis buah dengan cara meng-inputkan ciri-ciri buah dengan method `predict()` dan nanti akan mengembalikan label-nya, kita akan menginputkan berat baru dan texture-nya: 

{% highlight javascript %}

from sklearn import tree
features = [[60, 0], [1200, 1], [50, 0], [1500, 1] ]
labels = [0, 1, 0, 1]
clf = tree.DecisionTreeClassifier()
clf = clf.fit(features, labels)
print clf.predict([[70, 0]])

{% endhighlight %}

jalankan script dan kita akan melihat `[0]` akan di print itu berarti jenis buah rambutan.

{% highlight javascript %}
[~] $: python basic-machine-learning.py
[~] $: [0]
{% endhighlight %}

<img src="https://media.giphy.com/media/BdrSy2gqURFEk/giphy.gif">


