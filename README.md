Integrasi WhatsApp yang Sesungguhnya:

Menambahkan node HTTP Request untuk terhubung dengan API WhatsApp Business atau Twilio
Menambahkan autentikasi yang diperlukan


Penyimpanan Percakapan:

Menambahkan node untuk menyimpan percakapan di database (seperti MongoDB atau MySQL)
Memungkinkan AI untuk memiliki konteks percakapan sebelumnya


Penanganan Error:

Menambahkan node Error Catcher untuk menangani kesalahan
Memberikan respons yang ramah saat terjadi error


Fitur Tambahan:

Penerjemah bahasa
Pencarian informasi online
Pengingat dan pengatur jadwal



Tutorial Setup N8N dalam Bahasa Indonesia
1. Langkah Awal - Instalasi dan Akses N8N

Buka N8N dashboard

Jika belum memiliki n8n, unduh dan instal dari website resmi n8n
Atau gunakan hosting seperti Railway atau Digital Ocean


Buat Workflow Baru

Klik tombol "+ Workflow baru" di dashboard n8n
Beri nama "MCP WhatsApp Integration"



2. Mengimpor JSON Workflow

Import Workflow

Klik ikon tiga titik (⋮) di pojok kanan atas
Pilih "Import from JSON"
Salin dan tempel JSON yang saya berikan sebelumnya
Klik "Import"



3. Konfigurasi Kredensial

Setup Kredensial OpenRouter API (untuk AI Assistant)

Buat akun di OpenRouter jika belum memilikinya
Dapatkan API key dari dashboard OpenRouter
Di n8n, klik pada node "AI Assistant"
Di bagian "Credentials", klik "Create New"
Pilih "API Key"
Masukkan API key dari OpenRouter
Simpan kredensial tersebut


Setup WhatsApp Business API (opsional untuk integrasi WhatsApp sesungguhnya)

Daftar untuk WhatsApp Business API melalui Meta atau partner resmi
Atau gunakan Twilio untuk integrasi WhatsApp yang lebih mudah
Buat kredensial baru di n8n untuk layanan yang Anda pilih
Masukkan detail autentikasi yang diperlukan



4. Konfigurasi Webhook

Setup Webhook MCP Server

Klik pada node "MCP Server Trigger"
Catat URL Webhook (biasanya seperti http://localhost:5678/webhook/mcp-server)
Pastikan URL ini dapat diakses dari internet jika ingin digunakan dengan WhatsApp sungguhan
Untuk pengembangan lokal, Anda dapat menggunakan ngrok untuk meneruskan port


Update URL pada "Send to MCP Server"

Klik pada node "Send to MCP Server"
Perbarui URL ke alamat webhook aktual Anda
Format: http://alamat-n8n-anda:5678/webhook/mcp-server



5. Personalisasi Nodes

Penyesuaian Tools Router

Klik pada node "Tools Router"
Sesuaikan kata kunci deteksi jika diperlukan
Misalnya tambahkan kata kunci Bahasa Indonesia seperti "hitung", "kalkulator", "asisten", dll.


Penyesuaian AI Assistant

Klik pada node "AI Assistant"
Sesuaikan System Message untuk memberikan karakter atau persona tertentu
Tambahkan instruksi dalam Bahasa Indonesia


Penyesuaian WhatsApp

Jika menggunakan simulator, Anda bisa tetap menggunakan kode yang ada
Jika mengintegrasikan dengan WhatsApp sungguhan, ganti dengan node HTTP Request
Konfigurasikan sesuai dokumentasi API WhatsApp yang Anda gunakan



6. Pengujian Workflow

Gunakan Node Test Input

Klik kanan pada salah satu node test input (WhatsApp Test, Calculator Test, AI Test)
Pilih "Execute Node"
Pantau alur eksekusi melalui workflow
Periksa output di node "Show Output"


Uji Melalui Webhook Langsung

Gunakan Postman atau curl untuk mengirim permintaan POST ke webhook Anda
Format body: {"message": "kalkulator 2 + 3 * 4"}
Periksa respons yang dikirim kembali



7. Aktivasi Workflow

Aktifkan Workflow

Klik tombol toggle "Active" di pojok kanan atas
Workflow sekarang akan merespons permintaan webhook secara real-time


Pantau Eksekusi

Klik tab "Executions" untuk melihat riwayat eksekusi
Pantau kesalahan atau masalah yang mungkin muncul
Gunakan informasi debug untuk memperbaiki masalah



8. Integrasi dengan WhatsApp Sungguhan
Untuk menghubungkan dengan WhatsApp Business API:

Buat Meta App di Facebook Developer Portal

Daftar di Facebook Developer Portal
Buat aplikasi baru dengan kemampuan WhatsApp
Dapatkan Token Akses dan ID Nomor Telepon


Update Node WhatsApp

Ganti node "WhatsApp" dengan node "HTTP Request"
Konfigurasi sebagai berikut:

Method: POST
URL: https://graph.facebook.com/v17.0/NOMOR_TELEPON_ANDA/messages
Authentication: "Bearer Token"
Bearer Token: TOKEN_AKSES_ANDA
Content-Type: application/json
Body:
json{
  "messaging_product": "whatsapp",
  "to": "{{$node["Tools Router"].json["to"]}}",
  "type": "text",
  "text": { "body": "{{$node["Tools Router"].json["body"]}}" }
}






Tips Tambahan

Logging dan Monitoring

Tambahkan node Code untuk logging
Contoh kode: console.log('Debug:', $input.item); return $input.item;
Sisipkan di antara node untuk debug


Keamanan

Tambahkan autentikasi pada Webhook
Enkripsi data sensitif dalam kredensial
Batasi akses ke workflow hanya untuk admin


Skalabilitas

Gunakan database eksternal untuk penyimpanan
Implementasikan batasan rate untuk menghindari penyalahgunaan
Pertimbangkan untuk memisahkan workflow menjadi beberapa subworkflow untuk pemeliharaan yang lebih mudah
