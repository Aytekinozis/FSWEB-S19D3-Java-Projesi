# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	
	INSERT INTO yazar (yazarad,yazarsoyad)
	VALUES("KEMAL","UYUMAZ");
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	INSERT INTO  tur (turadi)
	VALUES("Biyografi");
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 
	INSERT INTO  ogrenci (ograd,ogrsoyad,sinif,cinsiyet)
 	VALUES("Çağlar","Üzümcü","10A","E")
	("Leyla","Alagöz","9B","K")
	("Ayşe","Bektaş","11C","K")
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
	INSERT INTO  yazar (yazarad,yazarsoyad)
 	VALUES (SELECT ograd,ogrsoyad FROM ogrenci
	ORDER BY RANDOM()
	LIMIT 1);
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
	INSERT INTO  yazar (yazarad,yazarsoyad)
	VALUES (SELECT ograd,ogrsoyad FROM ogrenci
	WHERE ogrno between 10 and 30);
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	INSERT INTO  yazar (yazarad,yazarsoyad)
	VALUES ("Nurettin","Belek")
	SELECT @IDENTITY as yazarno;
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
	UPDATE ogrenci
	SET sinif = "10C"
	WHERE ogrno = 3;
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	UPDATE ogrenci
	SET sinif = "10A"
	WHERE sinif = "9A";
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	UPDATE ogrenci
	SET puan = (puan+5);
	
	10) 25 numaralı yazarı silin.
	DELETE FROM yazar
	WHERE  yazarno = 25;

	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
	SELECT * FROM ogrenci
	WHERE dtarih is null;
	
	12) Doğum tarihi null olan öğrencileri silin. 
	DELETE FROM ogrenci
	WHERE dtarih is null;
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
	UPDATE kitap
	SET puan = puan+2
	WHERE kitapadi alike "a%";
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	INSERT INTO tur (turadi)
	VALUES ("kişisel gelişim");
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	UPDATE kitap
	SET turno = (select turno from tur  where turadi = "kişisel gelişim")
	WHERE kitapadi = "Başarı Rehberi";
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
	CREATE FUNCTION ogrencilistesi()
 	RETURNS SETOFF ogrenci
  	LANGUAGE "sql"
 	as $BODY$
	SELECT * FROM ogrenci
	$BODY$;
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
	CREATE PROCEDURE ekle(name character varying,surname character varying, gender character varying,birthday character varying, class character varying, point integer)
  	LANGUAGE "sql"
 	as $BODY$
	INSERT INTO ogrenci(ograd,ogrsoyad,cinsiyet,dtarih,sinif,puan)
	VALUES(name,surname,gender,birthday,class,point)
	$BODY$;
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	CREATE PROCEDURE sil(ogrencino integer)
	LANGUAGE "sql"
	as $BODY$
	DELETE FROM ogrenci where ogrno = ogrencinono
	$BODY$;
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
	CREATE PROCEDURE updateSinif(ogrencino integer,newsinif character varying)
	LANGUAGE "sql"
	as $BODY$
	UPDATE ogrenci
	SET sinif = newsinif
	where ogrno = ogrencino
	$BODY$;
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
	CREATE FUNCTION concat(fullname character varying)
 	RETURNS SETOFF ogrenci
	LANGUAGE "sql"
	AS $BODY$
	SELECT * FROM ogrenci
	WHERE CONCAT(ograd,ogrsoyad) ILIKE CONCAT("%",fullname,"%")
	$BODY$;
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	DROP FUNCTION IF EXISTS ogrencilistesi;
	DROP PROCEDURE IF EXISTS  ekle;
	DROP PROCEDURE IF EXISTS  sil;
	DROP PROCEDURE IF EXISTS  updateSinif;
	DROP FUNCTION IF EXISTS concat;
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	SELECT * FROM kitap
	WHERE turno = (SELECT turno FROM tur WHERE turadi = "DRAM")
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	SELECT * FROM kitap
	WHERE  yazarno = (SELECT yazarno FROM yazar  WHERE yazarad ILIKE "E%");
	
	24) Kitap okumayan öğrencileri listeleyiniz.
	SELECT * FROM ogrenci 
 	WHERE  ogrno NOT IN (SELECT ogrno FROM islem);
	
	25) Okunmayan kitapları listeleyiniz

	SELECT * FROM kitap 
 	WHERE kitapno NOT IN (SELECT kitapno FROM islem);
 
	26) Mayıs ayında okunmayan kitapları listeleyiniz.
 
	SELECT * FROM kitap 
 	WHERE kitapno NOT IN (SELECT kitapno FROM islem WHERE EXTRACT(MONTH FROM atarih::timestamp) != 5 AND EXTRACT (MONTH FROM vtarih::timestamp) != 5);

 
