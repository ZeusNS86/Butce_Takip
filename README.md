# Butce_Takip

# Günlük Harcama ve Bütçe Takip Uygulaması

## Proje Açıklaması
Bu uygulama, kullanıcıların aylık bütçesini belirlemesine ve harcamalarını kategori bazlı olarak kaydetmesine olanak tanır. Kullanıcı, yaptığı her harcamayı sisteme girerek toplam harcamasını ve kalan bütçesini görebilir.

## Özellikler
- Aylık bütçe tanımlama
- Harcama kaydı ekleme
- Tüm harcamaların listesi ve kalan bütçenin hesaplanması

## Kullanım
1. `butce_takip.py` dosyasını çalıştırın.
2. Menüden işlem seçin: Bütçe ayarlayın, harcama ekleyin veya geçmiş harcamaları görüntüleyin.
3. Tüm veriler `harcamalar.json` adlı dosyada saklanır.

## Gereksinimler
- Python 3

## Not
Bu uygulama terminal üzerinden çalışır. Basit düzeyde bir bütçe takip simülasyonu sağlar.


                     ------                     KOD KISMI                     ------


import json
import os

DATA_FILE = "harcamalar.json"

def yukle():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return {"butce": 0, "harcamalar": []}

def kaydet(veri):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(veri, f, indent=4, ensure_ascii=False)

def butce_ayarla(veri):
    veri["butce"] = float(input("Aylık bütçenizi girin: "))
    kaydet(veri)

def harcama_ekle(veri):
    kategori = input("Kategori: ")
    miktar = float(input("Tutar: "))
    aciklama = input("Açıklama: ")
    veri["harcamalar"].append({"kategori": kategori, "miktar": miktar, "aciklama": aciklama})
    kaydet(veri)

def harcamalari_goster(veri):
    toplam = 0
    print("\n--- Harcamalar ---")
    for h in veri["harcamalar"]:
        print(f"{h['kategori']} - {h['miktar']} TL - {h['aciklama']}")
        toplam += h["miktar"]
    print(f"Toplam harcama: {toplam} TL")
    print(f"Kalan bütçe: {veri['butce'] - toplam} TL\n")

def menu():
    veri = yukle()
    while True:
        print("1. Bütçe Ayarla")
        print("2. Harcama Ekle")
        print("3. Harcamaları Göster")
        print("4. Çıkış")
        secim = input("Seçiminiz: ")
        if secim == "1":
            butce_ayarla(veri)
        elif secim == "2":
            harcama_ekle(veri)
        elif secim == "3":
            harcamalari_goster(veri)
        elif secim == "4":
            break
        else:
            print("Geçersiz seçim.\n")

if __name__ == "__main__":
    menu()
