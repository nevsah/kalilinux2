#!/bin/bash
palindromlarDizisi=() #palindomların birleşeceği bos dizi
say=0
saa=-1
izo=()
IzoKontrol()
{
	for((i=1;i<=`expr length "$word"`;i++)) #kelimenin uzunlugunu bulu ve gezer
        do
                a=`expr substr "$word" $i 1` #kelimeyi harflerine ayırmak için substr kullandım
                dizi=(${dizi[@]} $a) #ust satırda buldugum degerleri diziye attım

        done
	for g in ${dizi[@]} #g değişkeniyle kelimenin harflerinden oluşan diziyi gezer
	do
		((saa++)) #sayac degişkenini artırırız donguye son girdiginde harf sayısını bulmak için
		say=0
		for h in ${dizi[@]}
		do
			if [ $h = $g ] #harfler dizisini tekrar gezip aynı harf varsa sayacı artırırız
			then
				((say++))
#				echo $say
			fi
			if(($say==2)) #burada 2 aynı harf varsa dedim çünkü bütün diziyi gezeceği için kendisine denk geldigi zaman da artıracaktır
			then
#				say=0
				izoSonuc=0
				break
			fi
		done
		izo=(${izo[@]} $g) 
		if (($say==2))
		then
#			say=0
			break
		fi

#		izo=(${izo[@]} $g)
	done
#echo ${izo[@]}
izo=()
}

Palindrom()
{

	for((i=1;i<=`expr length "$word"`;i++)) #kelimeyi harf harf diziye atar
        do
                a=`expr substr "$word" $i 1`
                dizi=(${dizi[@]} $a)

        done
        for j in ${#dizi[@]}
        do
                for((k=${#dizi[@]}-1;k>=0;k--)) #diziyi tersten gezmek için değişkeni azaltarak ilerler
                do
                        dizitersi=(${dizitersi[@]} ${dizi[k]}) #dizinin sonundan baslayarak baska bir diziye atarak tersini olustururz
                done
#                echo ${dizi[@]}
#                echo ${dizitersi[@]}
                for((l=0;l<${#dizi[@]};l++))
                do
                        ((sayac++))
                        if [ "${dizi[l]}" != "${dizitersi[l]}" ] #dizilerdeki harfleri kontrol ederek tersinin duzune esit olup olmdıgını kontrol eder
                        then
                                sayac=0
				sonuc=0
                                break
                        else
                                ((sayac2++))
                        fi
                done
                if [ $sayac2 -eq $sayac ] 
                then
                        if [ $sayac -ne 0 ]
                        then
				sonuc=1 #fonksiyonu cagırdıgımızda kullanabilmek için sonuc degiskenını 1 yapar
#				palindromlarDizisi=(${palindromlarDizisi[@]} $word)
#                                ((palindromSayaci++))
                        fi
                fi
                sayac2=0
        done
        dizitersi=()
        dizi=()
}
if [ $# -ne 1 ] #girilen arguman sayısı 1 degilse hata vercektir ve proggram çalışmayacaktır
then
	echo "yanlış arguman sayısı "
elif [ ! -f $1 ] #girilen dosyanın var olup olmadıgını kontrol eder
then
	echo "dosya yok"
else
	izogramlarDizisi=()
	izoKontrol=()
	izoSonuc=1
	toplamKelime=0
	izoSayac=0
	izoSayac2=0
	palindromlarDizisi=()
	dizi=()
	dizitersi=()
	palindromSayaci=0
	sayac=0
	sonuc=0
	sayac2=0
	uzunP=""
	uzunI=""
	uzunlukP=0
	a=0
	b=0
	uzunlukI=0
	while read -ra line; #satır satır gezer dosyayı
	do
    		for word in "${line[@]}"; #satırları kelime kelime gezer
    		do
        		((toplamKelime++)) #kelime sayısını bulmak için sayacı artırırız
#palindrom izogram
			Palindrom $word #polindrom fonksiyonunu cagırız
			izoSonuc=1
			IzoKontrol $word #ızogram fonksiyonunu cagırırız
			if (( $sonuc == 1 )) #polindrom sonucu 1 se kelimeyi diziye atar
			then
				((palindromSayaci++))
				palindromlarDizisi=(${palindromlarDizisi[@]} $word)
				b=`expr length "$word"`
				if(($b>$uzunlukP)) #uzunlugu en yuksek olanı bulmak için kosula soktu
				then
					uzunlukP=$b
					uzunP=$word
				fi
			fi
			if ((izoSonuc==1)) #izogram sonucu 1 ise diziye atar
			then
				izogramlarDizisi=(${izogramlarDizisi[@]} $word) 
				a=`expr length "$word"`
                		if(($a>$uzunlukI)) #uzunlugunun en yuksek oldugu kelimeyı bulur
                		then
                        		uzunlukI=$a
                        		uzunI=$word
                		fi
			fi
			dizitersi=()
        		dizi=()
     		done
	done < $1
	echo "palindrom sayısı:" $palindromSayaci
	echo "toplam kelime sayısı:" $toplamKelime
	#echo "palindrom kelimeler:" ${palindromlarDizisi[@]}
	echo ${#palindromlarDizisi[@]}
	echo "izogram sayısı:" ${#izogramlarDizisi[@]}
#	echo "izogram kelimeler:" ${izogramlarDizisi[@]}
#IzoKontrol $word
#	echo "uzunlugu pal:" $uzunlukP
	echo "En Uzun Palindrom Kelime:" $uzunP
	echo "uzunlugu izo:" $uzunlukI
	echo "En Uzun Izogram Kelime:" $uzunI
fi
