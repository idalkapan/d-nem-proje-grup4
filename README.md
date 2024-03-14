#include <iostream>
#include <fstream>
#include <string>

// Caesar şifreleme algoritması kullanarak verilen metni şifreleyen fonksiyon
std::string caesarEncrypt(const std::string& text, int shift) {
    std::string encryptedText = "";
    for (char c : text) {
        if (isalpha(c)) {
            char shiftedChar = (c - 'a' + shift) % 26 + 'a';
            encryptedText += shiftedChar;
        } else {
            encryptedText += c;
        }
    }
    return encryptedText;
}

// Caesar şifreleme algoritması kullanarak verilen şifreli metni çözen fonksiyon
std::string caesarDecrypt(const std::string& text, int shift) {
    std::string decryptedText = "";
    for (char c : text) {
        if (isalpha(c)) {
            char shiftedChar = (c - 'a' - shift + 26) % 26 + 'a';
            decryptedText += shiftedChar;
        } else {
            decryptedText += c;
        }
    }
    return decryptedText;
}

int main() {
    std::string dosyaAdi;
    int kaydirmaMiktari;

    // Kullanıcıdan şifrelenecek dosyanın adını isteyen kısım
    std::cout << "Şifrelenecek dosyanın adını girin: ";
    std::cin >> dosyaAdi;

    // Kaydırma miktarını isteyen kısım
    std::cout << "Kaydırma miktarını girin: ";
    std::cin >> kaydirmaMiktari;

    // Dosyayı okumak için gerekli adımlar
    std::ifstream dosya(dosyaAdi);
    if (!dosya) {
        std::cout << "Dosya açılamadı!" << std::endl; // Dosya açma hatası kontrolü
        return 1;
    }

    std::string metin;
    std::getline(dosya, metin); // Dosyadan metni okuma

    // Metni şifreleyip ekrana yazdıran kısım
    std::string sifreliMetin = caesarEncrypt(metin, kaydirmaMiktari);
    std::cout << "Şifreli metin: " << sifreliMetin << std::endl;

    // Şifreli metni çözüp ekrana yazdıran kısım
    std::string cozulmusMetin = caesarDecrypt(sifreliMetin, kaydirmaMiktari);
    std::cout << "Çözülmüş metin: " << cozulmusMetin << std::endl;

    dosya.close();  // Dosyayı kapat
    return 0;       // main fonksiyonunu başarıyla bitir
}
--------------------------------
#include <iostream>
#include <string>
#include <sstream>

using namespace std;

string dogrusalsifreleme(const string& metin, int a, int b) {
    string sifrelimetin = "";

    for (char karakter : metin) {
        int sifrelikarakter = (a * (int)karakter) + b;
        sifrelimetin += to_string(sifrelikarakter) + " ";
    }

    return sifrelimetin;
}

string dogrusaldesifreleme(const string& sifrelimetin, int a, int b) {
    string asilmetin = "";

    istringstream iss(sifrelimetin);
    string sifreliDeger;

    try {
        while (iss >> sifreliDeger) {
            int sifrelikarakter = stoi(sifreliDeger);
            int asilkarakter = (sifrelikarakter - b) / a;
            asilmetin += (char)asilkarakter;
        }
    } catch (const std::out_of_range& e) {
        cout << "Hata: Tamsayı sınırları dışında bir değer var. Deşifreleme başarısız." << endl;
        return "";
    } catch (const std::invalid_argument& e) {
        cout << "Hata: Geçersiz bir giriş var. Deşifreleme başarısız." << endl;
        return "";
    }

    return asilmetin;
}

int main() {
    cout << "Şifreleme mi (1) Deşifreleme mi (2) yapmak istersiniz? ";
    int secim;
    cin >> secim;

    cin.ignore(); 

    cout << "İşlem yapmak istediğiniz metni giriniz: ";
    string metin;
    getline(cin, metin);

    int a = 3;
    int b = 8;

    if (secim == 1) {
        string sifrelenmismetin = dogrusalsifreleme(metin, a, b);
        cout << "Şifrelenmiş Metininiz: " << sifrelenmismetin << "\n";
    } else if (secim == 2) {
        string cozulmusmetin = dogrusaldesifreleme(metin, a, b);
        if (!cozulmusmetin.empty()) {
            cout << "Çözülmüş Metininiz: " << cozulmusmetin << "\n";
        }
    } else {
        cout << "Şifreleme (1) veya Deşifreleme (2) şeklinde cevap vermelisiniz. \n";
        return 1;
    }

    return 0;
}
---------------------------------------
#include <iostream>
#include <openssl/rsa.h>
#include <openssl/err.h>
#include <cstring>

std::string rsaEncrypt(const std::string& plaintext, RSA* rsaPublicKey) {
    int rsaLen = RSA_size(rsaPublicKey);
    unsigned char* encryptedText = new unsigned char[rsaLen];

    int result = RSA_public_encrypt(plaintext.length(), reinterpret_cast<const unsigned char*>(plaintext.c_str()), encryptedText, rsaPublicKey, RSA_PKCS1_OAEP_PADDING);

    if (result == -1) {
        ERR_load_crypto_strings();
        ERR_print_errors_fp(stderr);
        delete[] encryptedText;
        return "";
    }

    std::string text(reinterpret_cast<char*>(encryptedText), result);
    delete[] encryptedText;

    return text;
}

std::string rsaDecrypt(const std::string& text, RSA* rsaPrivateKey) {
    int rsaLen = RSA_size(rsaPrivateKey);
    unsigned char* decryptedText = new unsigned char[rsaLen];

    int result = RSA_private_decrypt(text.length(), reinterpret_cast<const unsigned char*>(text.c_str()), decryptedText, rsaPrivateKey, RSA_PKCS1_OAEP_PADDING);

    if (result == -1) {
        ERR_load_crypto_strings();
        ERR_print_errors_fp(stderr);
        delete[] decryptedText;
        return "";
    }

    std::string plaintext(reinterpret_cast<char*>(decryptedText), result);
    delete[] decryptedText;

    return plaintext;
}

int main() {
    RSA* rsaKeyPair = RSA_generate_key(2048, RSA_F4, nullptr, nullptr);

    std::string Text = "Şifreleme, RSA Encryption!";
    std::cout << "Original Text: " << Text << std::endl;

    std::string encryptedText = rsaEncrypt(Text, rsaKeyPair);
    std::cout << "Encrypted Text: " << encryptedText << std::endl;

    std::string decryptedText = rsaDecrypt(encryptedText, rsaKeyPair);
    std::cout << "Decrypted Text: " << decryptedText << std::endl;

    RSA_free(rsaKeyPair);

    return 0;
}
-------------------------------------
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

// Fonksiyon prototipleri

string sifrele(string metin);
string desifrele(string sifreli_metin);

void dosyaYaz(const string& dosya_yolu, const string& icerik) {

    ofstream dosya(dosya_yolu);
    if (!dosya.is_open()) {

        cerr << "Dosya olusturulamadi: " << dosya_yolu << endl;
        exit(1);

    }

    dosya << icerik;
    dosya.close();
}

int main() {
    int secim;

    cout << "Sifrelemek icin 1/Desifrelemek icin 2: ";
    cin >> secim;
    cin.ignore(); // \n karakterini temizle

    string dosya_yolu;
    string dosya_ismi;

    cout << "Lutfen dosya yolunu girin: ";
    getline(cin, dosya_yolu);

    cout << "Lutfen dosyanin ismini uzantisiyla giriniz: ";
    getline(cin, dosya_ismi);

    ifstream SifrelenecekDosya(dosya_yolu);
    if (!SifrelenecekDosya.is_open()) {

        cerr << "Dosya acilamadi: " << dosya_yolu << endl;
        return 1;

    }

    string satir;
    string duz_metin;

    while (getline(SifrelenecekDosya, satir)) {

        duz_metin.append(satir);

    }

    SifrelenecekDosya.close();

    string sonuc;
    if (secim == 1) {

        sonuc = sifrele(duz_metin);
        cout << "Dosya sifrelendi ve olusturuldu." << endl;

    }
    else if (secim == 2) {

        sonuc = desifrele(duz_metin);
        cout << "Dosya desifrelendi ve olusturuldu." << endl;

    }
    else {

        cerr << "Gecersiz secim. Program sonlandirildi." << endl;
        return 1;

    }

    dosyaYaz(dosya_ismi, sonuc);
    remove(dosya_yolu.c_str());
    rename(dosya_ismi.c_str(), dosya_yolu.c_str());


    cout << "Dosya basariyla yerine konuldu." << endl;

    return 0;
}

string sifrele(string metin) {

    float kolon = 5.0;
    int satir_sayisi = ceil(metin.length() / kolon);

    vector<vector<char>> sifrelenmis(satir_sayisi, vector<char>(kolon));

    int index = 0;

    for (int col = 0; col < kolon; col++) {

        for (int row = 0; row < satir_sayisi; row++) {

            if (index < metin.length()) {

                sifrelenmis[row][col] = metin[index];
                index++;

            }
            else {

                sifrelenmis[row][col] = ' ';

            }
        }
    }

    string sifreli_metin;

    for (int row = 0; row < satir_sayisi; row++) {

        for (int col = 0; col < kolon; col++) {

            sifreli_metin += sifrelenmis[row][col];

        }
    }

    return sifreli_metin;
}

string desifrele(string sifreli_metin) {

    float kolon = 5.0;
    int satir_sayisi = sifreli_metin.length() / kolon;

    vector<vector<char>> desifrelenmis(satir_sayisi, vector<char>(kolon));

    int index = 0;

    for (int row = 0; row < satir_sayisi; row++) {

        for (int col = 0; col < kolon; col++) {

            desifrelenmis[row][col] = sifreli_metin[index];
            index++;

        }
    }

    string duz_metin;

    for (int col = 0; col < kolon; col++) {

        for (int row = 0; row < satir_sayisi; row++) {

            duz_metin += desifrelenmis[row][col];

        }
    }

    return duz_metin;
}
