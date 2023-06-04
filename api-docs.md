# Payfren API Documentation
## API Key
API Key diperlukan untuk mengakses semua _endpoint_ yang ada pada layanan API Payfren. Kamu perlu menambahkan prameter `apiKey` di setiap _endpoint_ untuk dapat mengakses layanan yang tersedia.

**_Example_**
```
/api/payment?apiKey=647cdb8fd98ebc4ca4238a0b923820dcc509a6f75849b
```

## Make Payment and Generate QR
```
[POST]  /api/payment
```
### Request
**_Example Request_**
```
{
    "nominal": 100000,
    "tip": 0,
    "ref": "ID40001",
    "callback": "http://websitemu.site/payment/73121a4e-5dd9-423d-980b-0ace6c719b90",
    "expire": 5
}
```
**_Request Schema_**
| Field       | Type     | Description                                                                                                                                          |
| ----------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| nominal     | `int`    | Nilai pembayaran yang tercantum pada QR Code, customer hanya dapat melakukan pembayaran sesuai dengan nilai yang tertera setelah pemindaian QR Code. |
| tip         | `int`    | Nilai tip tambahan yang tertera setelah pemindaian QR Code.                                                                                          |
| ref         | `String` | ID transaksi unik yang dibuat oleh merchant, minimal 1 karakter, maksimal 20 karakter, case insensitive.                                             |
| callback    | `String` | URL untuk dapat menerima notifikasi pembayaran setelah pembayaran dilakukan oleh customer.                                                           |
| expire      | `int`    | Batas waktu kadaluarsa atau expired dalam menit semenjak QRIS ditampilkan.                                                                           |

### Response
**_Example Response_**
```
{
    "ref_id": 7,
    "url": "http://127.0.0.1:8000/payment?ref_id=7"
}
```
**_Response Schema_**
| Field       | Type     | Description                                                                                                                                          |
| ----------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| ref_id      | `int`    | Pengidentifikasi unik dari sebuah transaksi (berbeda dengan field `ref`).                                                                            |
| url         | `String` | URL untuk melakukan redirect ke halaman pembayaran dari Payfren.                                                                                     |                                                                                         |

## Check Payment Status
```
[POST]  /api/payment_detail
```
### Request
**_Example Request_**
```
{
    "ref_id": 7
}
```
**_Request Schema_**
| Field       | Type     | Description                                                                                                                                          |
| ----------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| ref_id      | `int`    | Pengidentifikasi unik dari sebuah transaksi.                                                                                                         |

### Response
**_Example Response_**
```
{
    "status": "active",
    "data": {
        "nominal": 120000,
        "tip": 0,
        "ref": "ID40003",
        "callback": "http://websitemu.site/payment/73121a4e-5dd9-423d-980b-0ace6c719b90",
        "created_at": "2023-06-04 20:53:00",
        "expired_at": "2023-06-04 20:58:00"
    }
}
```
**_Response Schema_**
| Field       | Type     | Description                                                                                                                                          |
| ----------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| status      | `String` | Nilai pembayaran yang tercantum pada QR Code, customer hanya dapat melakukan pembayaran sesuai dengan nilai yang tertera setelah pemindaian QR Code. |
| nominal     | `int`    | Nilai pembayaran yang tercantum pada QR Code, customer hanya dapat melakukan pembayaran sesuai dengan nilai yang tertera setelah pemindaian QR Code. |
| tip         | `int`    | Nilai tip tambahan yang tertera setelah pemindaian QR Code.                                                                                          |
| ref         | `String` | ID transaksi unik yang dibuat oleh merchant, minimal 1 karakter, maksimal 20 karakter, case insensitive (berbeda dengan field `ref`).                |
| callback    | `String` | URL untuk dapat menerima notifikasi pembayaran setelah pembayaran dilakukan oleh customer.                                                           |
| vreated_at  | `datetime` | Waktu pembayaran dibuat.                                                                                                                           |
| expired_at  | `datetime` | Batas waktu kadaluarsa atau expired semenjak pembayaran dibuat.                                                                                    |
