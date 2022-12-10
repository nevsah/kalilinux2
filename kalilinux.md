#!/bin/bash
#Projemize birinci arguman olarak verilen dosyanın ilk olarak satır sayısını buluruz 
#Sonra noktadan öncesi ve noktadan sonrası olarak uzantısından ayırırız.
#Projemize verilen arguman sayısını kontrol ederiz.2 tane değilse uyarı vererek sonlanacaktır.
#projeye birinci arguman olarak verilen dosyanın varlıgını ve dosya olup olmadıgını kontrol ederiz.
#Projeye ikinci arguman olarak verilen sayının pozitif tam sayı olup olmadığını kontrol ederiz.
#bütün şartları sağlıyorsa dosyayı satır satır okuyup çevrimsel şekilde dosyalara atıyacaktır.

sayac=1
ad=$1
alt="_"
nokta="."
satirSayisi=$(wc -l < $1)
dosyaSatirSayisi=$(($satirSayisi/$2))

echo "Oluşturulacak dosyaların satır sayısı: $dosyaSatirSayisi"
echo "Dosyanın satır sayısı: $satirSayisi"

uzanti=${ad#*.}
uzantisiz=${ad%.*}
echo "Dosyanın uzantısız hali:$uzantisiz"
echo "Dosyanın uzantısı:$uzanti"

if (( $# !=2))
then
	echo "yanlis arguman sayisi"
elif [ ! -f $1 ]
then
	echo "dosya yok"

elif (( $2 < 0 ))
then
	echo "pozitif degil"
else
	while read line
	do
		for((j=sayac;j<=$2+1;j++))
		do
			if((sayac>$2))
			then
				sayac=1
			fi
			for((i=0;i<$dosyaSatirSayisi;i++))
			do
				echo $line >> $uzantisiz$alt$sayac$nokta$uzanti
				((sayac++))
				break
			done
			break
		done
	done <$1
fi
