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
