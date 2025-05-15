# Tutorial Setup Ultimate AI WhatsApp Agent di n8n

## 1. Prasyarat
- Akun n8n (bisa Railway, VPS, Docker, dsb)
- Akun OpenRouter/OpenAI (untuk LLM)
- WhatsApp API Provider (misal Waboxapp, Chat-API, Ultramsg, dll)
- (Opsional) Redis untuk chat memory

## 2. Import Workflow
1. Login ke n8n.
2. Klik **Create Workflow** > **Import**.
3. Paste seluruh isi file `ai-agent-wa-ultimate-n8n.json` di atas.
4. Klik **Import**.

## 3. Konfigurasi Credential
- **OpenRouter API:** Buat credential API Key pada menu Credential n8n.
- **WhatsApp Gateway:** Untuk node WA Send/Reply/Delete/List/Broadcast, ganti node-code dengan node HTTP Request dan sesuaikan endpoint, method, dan body dengan API WhatsApp provider pilihanmu (lihat dokumentasi API masing-masing).

## 4. Konfigurasi Webhook
- Pada node "WA Server Trigger", klik dan copy URL webhook.
- Gunakan URL ini sebagai endpoint dari aplikasi, bot, atau integrasi lain.

## 5. Contoh Payload Request
Kirim request POST ke webhook misal:
```json
{
  "message": "Kirim pesan ke grup keluarga: Selamat pagi semua!"
}
```
Atau:
```json
{
  "message": "Balas pesan dengan ID 123456: Terima kasih."
}
```
Atau:
```json
{
  "message": "Broadcast: Rapat mulai jam 10 pagi!"
}
```

## 6. Customisasi Node WhatsApp
- Ganti node "WA Send", dst, dengan **HTTP Request** sesuai API WhatsApp-mu.
- Contoh jika pakai HTTP Request (POST ke Ultramsg):
  - URL endpoint: `https://api.ultramsg.com/instanceXXXX/messages/chat`
  - Body: `{ "to": "nomor_grup", "body": "Isi pesan" }`
- Mapping parameter dari function call AI ke parameter body HTTP Request.

## 7. Uji Coba
- Kirim pesan ke webhook sesuai aktivitas WhatsApp harianmu (kirim, balas, hapus, broadcast).
- Lihat hasil response webhook (dan pesan masuk ke WhatsApp lewat API provider).

## 8. (Opsional) Tambah Memory Redis
- Ganti node "Redis Chat Memory" dengan node Redis asli di n8n jika ingin menyimpan chat history.

## 9. Scale Up
- Tambahkan intent baru (polling, reminder, dsb) di node "AI Router".
- Integrasikan tools lain (Google Calendar, Sheets, dsb) dengan pola serupa.

---

**Workflow ini memungkinkan kamu mengontrol aktivitas WhatsApp group harian via AI