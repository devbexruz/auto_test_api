# AutoTestAI Backend

`auto_test_api` — bu loyihaning Django + Django REST Framework asosidagi backend qismi.

## Umumiy ma'lumot

Backend O'zbekiston haydovchilik guvohnomasi testlarini boshqarish uchun mo'ljallangan. U quyidagi asosiy funksiyalarni taqdim etadi:

- Foydalanuvchi autentifikatsiyasi va sessiya boshqaruvi
- Mavzular, biletlar, testlar va variantlarni CRUD operatsiyalari
- Testlarni boshlash va yechish jarayonini boshqarish
- Natijalar va statistikani hisoblash
- Admin panel API orqali ma'lumotlarni boshqarish
- Media fayllarini saqlash va toza o'chirish

## Texnologiyalar

- Python 3.11 yoki 3.12
- Django 5.2.7
- Django REST Framework 3.16.1
- drf-spectacular
- django-cors-headers
- SQLite (standart)
- python-dotenv

## Arxitektura

### Asosiy modellari

- `User`: `AbstractUser` dan meros olgan custom model, `full_name`, `role`, `ruxsat` maydonlari mavjud.
- `Theme`: test mavzulari.
- `Ticket`: bilet turlari.
- `Test`: test savollari, rasm va to'g'ri javobga bog'langan.
- `Variant`: test javob variantlari.
- `Result`: foydalanuvchi natijasi, to'g'ri/nota'g'ri javoblar, test turi va vaqt.
- `TestSheet`: natija uchun individual test va variantlar ketma-ketligi.
- `UserSession`: token asosida foydalanuvchi sessiyasi va qurilma ma'lumotlari.
- `Data`: umumiy konfiguratsion kalit-qiymatlar.

### Muhit sozlamalari

`config/settings.py` faylida quyidagi parametrlar mavjud:

- `SECRET_KEY`
- `DEBUG`
- `ALLOWED_HOSTS`
- `CSRF_TRUSTED_ORIGINS`
- `CORS_ALLOW_ALL_ORIGINS = True`
- `MEDIA_ROOT` va `MEDIA_URL`
- `AUTH_USER_MODEL = 'api.User'`
- `REST_FRAMEWORK` bilan token va session autentifikatsiyasi

## API endpointlar

### Foydalanuvchi qismi

- `POST /api/auth/login/`
- `POST /api/auth/logout/`
- `GET /api/profile/`
- `GET /api/themes/`
- `GET /api/tickets/`
- `POST /api/start_tests/`
- `POST /api/solve_tests/`
- `GET /api/result/<result_id>/tests/`
- `GET /api/result/<result_id>/statistics/`
- `GET /api/results/`
- `GET /api/statistics/`

### Admin qismi

- `GET, POST /api/admin/user/`
- `GET, PUT, DELETE /api/admin/user/<pk>/`
- `GET, POST /api/admin/theme/`
- `GET, PUT, DELETE /api/admin/theme/<pk>/`
- `GET, POST /api/admin/ticket/`
- `GET, PUT, DELETE /api/admin/ticket/<pk>/`
- `GET, POST /api/admin/test/`
- `GET, PUT, DELETE /api/admin/test/<pk>/`
- `GET, POST /api/admin/test/<test_id>/variant/`
- `GET, PUT, DELETE /api/admin/test/variant/<pk>/`
- `PATCH /api/admin/test/variant/<pk>/true/`
- `GET /api/admin/statistics/`
- `GET /api/admin/all_users_stats/`
- `GET /api/admin/user_statistics/<user_id>/`

### Jamoat qismi

- `POST /api/public/connection/`

### Hujjatlashuv

- Swagger: `/swagger-ui/`
- OpenAPI: `/api/schema/`
- Redoc: `/api/redoc/`

## Lokal ishga tushirish

### 1. Virtual muhit yaratish

```bash
cd auto_test_api
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 2. `.env` faylini sozlash

`.env` faylida kamida quyidagilar bo'lishi kerak:

```env
SECRET_KEY=replace-with-secret-key
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,localhost
```

### 3. Migratsiyalar

```bash
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
```

### 4. Serverni ishga tushirish

```bash
python manage.py runserver 0.0.0.0:8000
```

## Foydalanish va testlash

- Backend uchun `api/urls.py` ichidagi endpointlar React ilovasi tomonidan ishlatiladi.
- Ma'lumotlar bazasi `db.sqlite3` faylida saqlanadi.
- Media fayllar `media/images/` papkasiga yuklanadi.
- `Test` modeli yangi rasm o'rnatilganda eski rasmni avtomatik o'chiradi.
- `TestSheet` saqlanish vaqtida `variant_orders` ni random tarzda yaratadi.

## Tavsiyalar

- Productionda `DEBUG=False` va `CORS_ALLOW_ALL_ORIGINS=False` qo'ying.
- SQL ma'lumotlar bazasini PostgreSQL yoki MySQL ga o'zgartiring.
- `ALLOWED_HOSTS` va `CSRF_TRUSTED_ORIGINS` ni real domenlarga moslang.
- `STATIC_ROOT` va `MEDIA_ROOT` ni Nginx orqali xizmat ko'rsatish uchun sozlang.

## Arxitekturaviy qarorlar

- Django REST Framework foydalanuvchi va admin resurslarini ajratib boshqaradi.
- `DefaultRouter` faqat `start_tests` va `solve_tests` viewsetlari uchun ishlatiladi.
- Boshqa endpointlar klassik `APIView` va `GenericAPIView` bilan yozilgan.
- `AUTH_USER_MODEL` orqali custom user model saqlangan.
- `drf_spectacular` yordamida API hujjatlari avtomatik yaratiladi.

## Qo'shimcha manbalar

- `api/models.py`
- `api/urls.py`
- `config/settings.py`
- `config/urls.py`

Bu README backendning funksionalligi, deploy qoidalari va kengaytirilishi haqida senior darajadagi to'liq tavsif beradi.