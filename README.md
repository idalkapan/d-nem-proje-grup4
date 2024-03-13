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

