# dönem-proje-grup4
//sezar  şifreleme  her harf sıraya göre 3  kayar.

#include <iostream>
#include<cstdlib>
#include<cstring>
#include<clocale>

using namespace std;

class sezar{         //sınıftan nesne oluştururuz.
private:
    char metin[1024];
    char sifrelimetin[1024];  //kullanıcı metin girer biz şifreli hale getiririz.
    char desifrelimetin[1024];  //şifreli metni bu değişkende tutarız
    char alfabe[27]= "abcdefghijklmnopqrstuvywxz" ; //26 harf
public:
    void sifreli();
    void desifreli();
    
};

void sezar::sifreli()
{
    cout<<"metin:";  gets(metin);     //kullanıcıdan metni alır şifreleriz
    int n=strlen(metin);   // strlen ile metnin uzunluğunu buluruz.
    metin[n]='\0';   //gets ile aldığımız için metni null ile kapatırız.
    cout<<"metin:"<<metin<<endl;   //metni ekrana yazdırır.
    
    int i=0,j;
    
    while(i<n)      //metin uzunluğunca döndürürüz.dış kısımda
    {
        int sonuc=0;
        for(j=0;j<26;j++) //alfabedeki sırayı buluruz.
        {
            int uzunluk=j;
          if(metin[i]==alfabe[j])
            {
                sonuc=1;
                uzunluk+=3;   // 3 adım kayması için
                if(uzunluk>=26)
                uzunluk=uzunluk%26;   //eğer son kısma gelirse burayı uygulamış olur.
                
                sifrelimetin[i]= alfabe[uzunluk];
            }
            if(sonuc==0)           //kullanıcı 26 harf harici başka bir şey girmişse buraya gelir.
                sifrelimetin[i]= metin[i];   //metnin aynısını girmiş olur.
        }
        i++;         //i arttırılacaktır.
}
    cout<<"Şifreli metin ="<<sifrelimetin<<endl;
}

void sezar::desifreli()   //sifreli metni çözmek için tam tersini yaparız.
{
   
    int n=strlen(sifrelimetin);   // strlen ile şifreli metnin uzunluğunu buluruz.
    sifrelimetin[n]='\0';   //gets ile aldığımız için metni null ile kapatırız.
    cout<<"metin:"<<sifrelimetin<<endl;   //şifreli metni ekrana yazdırır.
    
    int i=0,j;
    
    while(i<n)      //metin uzunluğunca döndürürüz.dış kısımda
    {
        int sonuc=0;
        for(j=0;j<26;j++) //alfabedeki sırayı buluruz.
        {
            int uzunluk=j;
          if(sifrelimetin[i]==alfabe[j])
            {
                sonuc=1;
                uzunluk-=3;   // 3 adım geri kayması için
                if(uzunluk<0)
                uzunluk=uzunluk+26;   //eğer eksi kısma girerse bu işlem uygulanır.
                
                desifrelimetin[i]= alfabe[uzunluk];
            }
            if(sonuc==0)           //kullanıcı 26 harf harici başka bir şey girmişse buraya gelir.
                desifrelimetin[i]= metin[i];   //metnin aynısını girmiş olur.
        }
        i++;         //i arttırılacaktır.
}
    cout<<"deşifreli metin ="<<desifrelimetin<<endl;
}





int main()    // main fonksiyonu altında diğer fonksiyonları çağırırız.
{
    sezar s1;
    s1.sifreli();
    s1.desifreli();
    return 0;
}
