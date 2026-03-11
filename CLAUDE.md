# Multi-Agent Workflow

Bu projede 6 ajanlı bir iş akışı kullanılmaktadır. Ajanlar otomatik olarak tetiklenir veya `/ajan-adi` ile manuel çağrılabilir.

## Ajanlar

| Ajan | Skill Komutu | Görev |
|------|-------------|-------|
| **Supervisor** | (otomatik) | Tüm ajanları yönetir, isteği analiz edip uygun ajana yönlendirir |
| Designer | `/designer` | Mimari tasarım, UI/UX wireframe, DB şema, API dizayn |
| CodeWriter | `/code-writer` | Sıfırdan yeni kod dosyaları oluşturma |
| CodeEditor | `/code-editor` | Mevcut kodu düzenleme, refactoring, bug fix |
| ErrorChecker | `/error-checker` | Hata ve güvenlik denetimi (OWASP Top 10) |
| FileManager | `/file-manager` | Dosya okuma, arama, proje yapısı analizi |

## İş Akışı Kuralları

1. Yeni bir özellik tasarlanacaksa: **designer → error-checker → code-writer → error-checker**
2. Kod yazıldıktan sonra HER ZAMAN **error-checker** çalıştırılmalı
3. Error-checker CRITICAL bulursa: **code-editor** ile düzelt → tekrar **error-checker**
4. Sadece WARNING varsa: kullanıcıya bildir, karar onlara bırakılır

## Güvenlik Standartları

- Hardcoded secret/API key YASAK
- Kullanıcı girdileri her zaman valide edilmeli
- Veritabanı sorgularında parametrize query kullanılmalı (SQL, NoSQL, Firestore fark etmez)
- Dosya yollarında path traversal koruması olmalı
- Üretim ortamında debug mode KAPALI olmalı
