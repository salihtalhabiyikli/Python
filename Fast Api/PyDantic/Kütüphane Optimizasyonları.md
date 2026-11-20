### Hafta 1: Temeller (FastAPI'siz)

- Type hint'lere hızlı tekrar: `list[int]`, `X | None`, `Optional`, `Literal`
- `BaseModel` oluşturma, alanlar, varsayılan değerler
- Tip zorlama (coercion): `"5"` → `5`, strict mode farkı
- `ValidationError`'ı okumak: `errors()`, `loc`, `msg`, `type`
- `model_dump()`, `model_dump_json()`, `model_validate()`, `model_validate_json()`
- İç içe modeller ve `list[Model]`

**Pratik:** Bir kullanıcı profili modeli yazın; bilerek hatalı veri verip hata çıktılarını inceleyin.

### Hafta 2: Doğrulama

- `Field()`: `min_length`, `max_length`, `ge`, `le`, `pattern`, `description`, `examples`
- `field_validator` (mode: `before` / `after`)
- `model_validator` (alanlar arası kontrol, örn. şifre eşleşmesi)
- `Annotated` ile yeniden kullanılabilir tipler (`AfterValidator`, `StringConstraints`)
- Hazır tipler: `EmailStr`, `HttpUrl`, `SecretStr`, `UUID`, `datetime`, `Enum`

**Pratik:** Kayıt formu modeli: e-posta, şifre kuralları, telefon, yaş aralığı.

### Hafta 3: FastAPI entegrasyonu

- Request body için Pydantic modeli kullanımı
- `response_model` ve yanıtta alan filtreleme
- Query / Path / Header parametrelerinde `Field` ve `Query`, `Path`
- Query parametreleri için Pydantic modeli (`Annotated[Model, Query()]`)
- Otomatik 422 hataları ve özel hata yanıtı için `RequestValidationError` handler'ı
- Swagger/OpenAPI'de `examples` ve `description` düzenlemek

**Pratik:** Basit bir CRUD API (ürünler); doğrulama hataları Türkçe ve düzenli dönsün.

### Hafta 4: Model tasarımı ve mimari

- Model ayrımı: `UserBase`, `UserCreate`, `UserUpdate`, `UserRead` (şifre çıktıya asla girmesin)
- Kısmi güncelleme (PATCH): `model_dump(exclude_unset=True)`
- `ConfigDict`: `from_attributes`, `extra`, `populate_by_name`, `str_strip_whitespace`
- Projeye özel `AppModel` temel sınıfı
- Alias ve `alias_generator` (camelCase ↔ snake_case)
- ORM (SQLAlchemy / SQLModel) ile çalışmak: `from_attributes=True`
- Pagination ve generic yanıt modelleri: `Generic[T]`

**Pratik:** Hafta 3'teki API'yi SQLAlchemy ile bağlayın; Create/Update/Read modellerini ayırın.

### Hafta 5: Serileştirme ve ileri konular

- `field_serializer`, `model_serializer`
- `computed_field`
- `model_dump` seçenekleri: `include`, `exclude`, `by_alias`, `exclude_none`
- Discriminated union: `Field(discriminator="type")`
- `TypeAdapter` ile modelsiz doğrulama
- Özel tipler: `__get_pydantic_core_schema__` (isteğe bağlı, ileri seviye)
- Performans: `model_validate_json`, `model_construct`, `strict` mod

**Pratik:** Farklı ödeme yöntemlerini (kart, havale, kapıda ödeme) discriminated union ile modelleyin.

### Hafta 6: Konfigürasyon, test ve bitirme projesi

- `pydantic-settings`: `.env`, `env_prefix`, iç içe ayarlar, `SecretStr`
- FastAPI'de `Depends` ile settings kullanımı ve `lru_cache`
- Pydantic modellerini `pytest` ile test etmek (geçerli/geçersiz veri senaryoları)
- Doğrulama hatalarını loglamak ve izlemek
- Pydantic v1 → v2 geçiş farkları (eski kodları okuyabilmek için)