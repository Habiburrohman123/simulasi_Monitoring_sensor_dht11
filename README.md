// Definisikan ID template Blynk, nama template, dan token otentikasi

#define BLYNK_TEMPLATE_ID "TMPL6Q8eMQoOb"

#define BLYNK_TEMPLATE_NAME "uashabiburrohman"

#define BLYNK_AUTH_TOKEN "DVgdCxZA334aegoMRQ_1RnCwtNKtL1Pi"

// Definisikan output debug untuk Blynk akan dikirim ke SwSerial

#define BLYNK_PRINT SwSerial

// Sertakan library SoftwareSerial untuk komunikasi serial

#include <SoftwareSerial.h>

// Inisialisasi objek SoftwareSerial dengan pin 10 sebagai RX dan pin 11 sebagai TX

SoftwareSerial SwSerial(10, 11); // RX, TX

// Sertakan library Blynk dan DHT

#include <BlynkSimpleStream.h>

#include <DHT.h>

// Token otentikasi Blynk

char auth[] = BLYNK_AUTH_TOKEN;

// Definisikan pin untuk sensor DHT dan tipe sensornya

#define DHTPIN 2

#define DHTTYPE DHT11       // Menggunakan DHT 11

// #define DHTTYPE DHT22    // Opsi untuk menggunakan DHT 22, AM2302, AM2321

// #define DHTTYPE DHT21    // Opsi untuk menggunakan DHT 21, AM2301

// Inisialisasi objek DHT

DHT dht(DHTPIN, DHTTYPE);

// Inisialisasi objek BlynkTimer

BlynkTimer timer;

// Variabel untuk menyimpan nilai kelembaban dan suhu

int h;

int t;

// Fungsi untuk membaca data dari sensor dan mengirimkannya ke Blynk

void sendSensor() {

  // Baca kelembaban dari sensor DHT
  
  h = dht.readHumidity();
  
  // Baca suhu dari sensor DHT
  
  t = dht.readTemperature();
  
  // Kirim nilai kelembaban ke Virtual Pin V1 di Blynk
  
  Blynk.virtualWrite(V1, h);
  
  // Kirim nilai suhu ke Virtual Pin V0 di Blynk
  
  Blynk.virtualWrite(V0, t);

}

// Fungsi setup untuk inisialisasi

void setup() { 

  // Mulai komunikasi serial software dengan kecepatan baud 9600
  
  SwSerial.begin(9600);
  
  // Mulai komunikasi serial hardware dengan kecepatan baud 9600
  
  Serial.begin(9600);
  
  // Mulai koneksi Blynk menggunakan komunikasi serial dan token otentikasi
  
  Blynk.begin(Serial, auth);
  
  // Mulai sensor DHT
  
  dht.begin();
  
  // Atur interval timer untuk memanggil fungsi sendSensor setiap 1 detik
  
  timer.setInterval(1000L, sendSensor); // kirim tiap detik

}

// Fungsi loop yang berjalan terus menerus

void loop() {

  // Menjaga koneksi Blynk tetap berjalan
  
  Blynk.run();
  
  // Menjalankan fungsi timer
  
  timer.run();
  
  // Memanggil fungsi untuk mengirim data sensor (opsional, sudah dilakukan oleh timer)
  
  sendSensor();

}
