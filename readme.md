# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
		select o.ograd,o.ogrsoyad, i.atarih 
		from ogrenci as o, islem as i
		where o.ogrno=i.ogrno;
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		select kitap.kitapadi, tur.turadi 
		from kitap, tur
		where kitap.turno=tur.turno and tur.turadi in("Fıkra","Hikaye");
	

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

		select o.sinif,o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi
		from ogrenci as o, islem as i, kitap as k
		where o.sinif in("10B","10C")
		and o.ogrno=i.ogrno
		and i.kitapno=k.kitapno
		order by o.sinif;

		
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

		select o.ograd,o.ogrsoyad, i.atarih
		from ogrenci as o
		inner join islem as i 
		on o.ogrno=i.ogrno;
	
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		select kitap.kitapadi, tur.turadi
		from kitap
		inner join tur
		on kitap.turno=tur.turno 
		where tur.turadi in ("Fıkra","Hikaye");

	
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
		select o.sinif,o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi
		from islem as i
		inner join ogrenci as o
		on o.ogrno=i.ogrno
		inner join kitap as k
		on i.kitapno=k.kitapno
		where o.sinif in("10B","10C")
		order by o.sinif;

	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

		select o.ograd,o.orsoyad, i.atarih 
		from islem as i
		right join ogrenci as o
		on i.orno=o.ogrno;
		
	
	
	8) Kitap almayan öğrencileri listeleyin.

		select *
		from ogrenci
		left join islem
		on ogrenci.ogrno=islem.ogrno
		where islem.ogrno is null;
	
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

		select k.kitapno,k.kitapadi, count(i.kitapno) as "total"
		from kitap as k
		inner join islem as i
		on i.kitapno=k.kitapno
		group by i.kitapno
		order by i.kitapno;
	
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		select k.kitapno, k.kitapadi,count(k.kitapno) as "total"
		from islem as i
		right join kitap as k
		on i.kitapno=k.kitapno
		group by k.kitapno;
		


	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

		select o.ograd,o.ogrsoyad,k.kitapadi
		from islem as i
		inner join ogrenci as o
		on i.ogrno=o.ogrno
		inner join kitap as k
		on i.kitapno=k.kitapno;
		
		
	
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

		select o.ograd,o.ogrsoyad,k.kitapadi,y.yazarad, y.yazarsoyad, t.turadi, i.atarih
		from ogrenci as o
		left join islem as i
		on i.ogrno=o.ogrno
		left join kitap as k
		on i.kitapno=k.kitapno
		left join yazar as y
		on y.yazarno=k.yazarno
		left join tur as t
		on k.turno=t.turno;
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

		select o.ograd,o.ogrsoyad,count(i.ogrno) as "total"
		from ogrenci as o
		inner join islem as i
		on i.ogrno=o.ogrno
		where o.sinif in ("10A","10B")
		group by o.ograd,o.orgsoyad;

	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

		select avg(kitap.sayfasayisi) from kitap;

	 
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

		select * from kitap as k
		where k.sayfasayisi>(select avg(kitap.sayfasayisi) from kitap);
	
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin

		select count(o.ogrno) from ogrenci as o;
	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

		select count(o.ogrno) as toplam_sayi from ogrenci as o;
	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

		select distinct o.ograd from ogrenci o;
	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

		select k.sayfasayisi from kitap as k
		where k.sayfasayisi=(select max(k.sayfasayisi) from kitap as k);
	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		select k.kitapadi, k.sayfasayisi from kitap as k
		where k.sayfasayisi=(select max(k.sayfasayisi) from kitap as k);
	
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		select k.sayfasayisi from kitap as k
		where k.sayfasayisi=(select min(k.sayfasayisi) from kitap as k);

	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		select k.kitapadi, k.sayfasayisi from kitap as k
		where k.sayfasayisi=(select min(k.sayfasayisi) from kitap as k);
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

		select k.sayfasayisi from kitap as k
		inner join tur as t
		on t.turno=k.turno
		where t.turadi="Dram"
		order by k.sayfasayisi desc
		limit 1;
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

		select sum(k.sayfasayisi) as "toplam sayfa sayısı" from islem as i
		inner join ogrenci as o
		on i.ogrno=o.ogrno
		inner join kitap as k
		on k.kitapno=i.kitapno
		where o.ogrno=15;
	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		select o.ograd, count(o.ograd) from ogrenci as o
		group by o.ograd;

	
	26) Her sınıftaki öğrenci sayısını bulunuz.

		select o.sinif, count(o.ogrno) from ogrenci as o
		group by o.sinif;
	
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

		select o.sinif, o.cinsiyet, count(o.cinsiyet) from ogrenci as o
		group by o.sinif, o.cinsiyet;
	
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

		select o.ograd,o.ogrsoyad,sum(k.sayfasayisi) from islem as i
		inner join ogrenci as o 
		on i.ogrno=o.ogrno
		inner join kitap as k
		on k.kitapno=i.kitapno
		group by o.ograd,o.ogrsoyad;
	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		select o.ograd,o.ogrsoyad,count(i.islemno) as "okuduğu kitap sayısı" from ogrenci as o
		inner join islem as i
		on i.ogrno=o.ogrno
		group by o.ograd,o.ogrsoyad;
