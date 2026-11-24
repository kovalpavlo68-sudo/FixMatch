#  FixMatch: Nowoczesny System Zleceń
### Dynamiczna platforma łącząca transparentny biznes z natychmiastową odpowiedzią na potrzeby klientów.

Aplikacja rewolucjonizuje rynek usług lokalnych i zdalnych poprzez **dwukierunkowy model dopasowania (Two-Way Marketplace)**. Zamiast jednostronnego przeszukiwania bazy, platforma oferuje dwa zintegrowane strumienie ogłoszeń, które maksymalnie skracają czas od publikacji do rozpoczęcia współpracy.



###  Strumień Zleceniobiorców (Wirtualna Wizytówka Fachowca)
Moduł dedykowany firmom oraz freelancerom do **pasywnego pozyskiwania klientów** za pomocą precyzyjnie sprofilowanej oferty.

* **Dla kogo:** Hydraulicy, ekipy remontowe, korepetytorzy, programiści, kosmetologiczne usługi mobilne.
* **Kluczowe elementy profilu:**
  * **Matryca kompetencji:** Szczegółowy katalog świadczonych usług z podziałem na kategorie i tagi.
  * **Galeria realizacji:** Dynamiczne portfolio (zdjęcia „przed/po”) weryfikowane przez system pod kątem autentyczności.
  * **Zasięg operacyjny:** Integracja z mapami określająca dokładny promień dojazdu do klienta.
  * **Social Proof:** System ocen i gwiazdek powiązany wyłącznie ze zrealizowanymi przez aplikację zleceniami.

### 📋 Strumień Zleceniodawców (Giełda Zadań Klienta)
Moduł pozwalający konsumentom na **aktywne zgłaszanie zapotrzebowania** i odwrócenie procesu zakupowego – to fachowcy walczą o klienta.

* **Dla kogo:** Osób prywatnych i firm szukających szybkiego rozwiązania konkretnego problemu.
* **Kluczowe elementy zgłoszenia:**
  * **Kreator intencji:** Intuicyjny formularz ułatwiający precyzyjne opisanie problemu (np. *„Wymiana paneli podłogowych, 25m², salon, materiał po mojej stronie”*).
  * **Kontekst wizualny:** Możliwość załączenia wideo lub zdjęć usterki/miejsca pracy w celu dokonania zdalnej wyceny.
  * **Budżet i ramy czasowe:** Określenie widełek cenowych (lub opcja „do negocjacji”) oraz wskaźnik pilności (np. *„Na wczoraj”*, *„W ciągu 2 tygodni”*).
  * **System Ofertowania:** Zgłoszenia klientów trafiają na tablicę fachowców, którzy licytują stawkami i terminami.

# FixMatch – lista tabel bazy danych

| Lp. | Nazwa tabeli | Opis |
|---:|---|---|
| 1 | `user` | Konta użytkowników, dane osobowe i role |
| 2 | `contractor_profile` | Profile fachowców i firm świadczących usługi |
| 3 | `category` | Hierarchiczny katalog kategorii usług |
| 4 | `service_offer` | Usługi oferowane przez fachowców |
| 5 | `tag` | Tagi opisujące kompetencje i specjalizacje |
| 6 | `service_offer_tag` | Powiązania usług z tagami |
| 7 | `job_request` | Ogłoszenia zleceń publikowane przez klientów |
| 8 | `attachment` | Metadane przesłanych zdjęć, filmów i dokumentów |
| 9 | `job_attachment` | Powiązania załączników ze zleceniami |
| 10 | `portfolio_item` | Portfolio i historia realizacji fachowców |
| 11 | `portfolio_attachment` | Zdjęcia i inne materiały przypisane do portfolio |
| 12 | `bid` | Oferty składane przez fachowców na zlecenia |
| 13 | `contract` | Zaakceptowane oferty i realizowane zlecenia |
| 14 | `review` | Oceny i opinie po zakończeniu zlecenia |
| 15 | `conversation` | Konwersacje pomiędzy klientami a fachowcami |
| 16 | `message` | Wiadomości w ramach konwersacji |

## Tabele dodatkowe

| Lp. | Nazwa tabeli | Opis |
|---:|---|---|
| 17 | `notification` | Powiadomienia o ofertach, wiadomościach i zmianach statusów |
| 18 | `job_status_history` | Historia zmian statusów zleceń |
| 19 | `contract_status_history` | Historia zmian statusów realizacji |
| 20 | `contractor_verification` | Weryfikacja kont fachowców i firm |
| 21 | `report` | Zgłoszenia naruszeń regulaminu i nieodpowiednich treści |


# FixMatch – struktura bazy danych

Proponowany model relacyjnej bazy danych dla aplikacji **FixMatch – Nowoczesny System Zleceń**.

Przyjęto następujące założenia:

- Klucze główne `id` mają typ `BIGINT`.
- Klucze obce oznaczono jako `FK`.
- Pola `created_at` i `updated_at` przechowują daty utworzenia i modyfikacji rekordów.
- Pola opcjonalne mogą przyjmować wartość `NULL`.
- Kwoty przechowywane są w typie `DECIMAL(12,2)`.
- Daty i czas zdarzeń przechowywane są jako `TIMESTAMP`.
- Nazwy tabel i kolumn zapisano w konwencji `snake_case`.

# 1. Tabele podstawowe

## 1.1. `user` – użytkownicy

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator użytkownika |
| `email` | VARCHAR(180), UNIQUE | Adres e-mail |
| `password_hash` | VARCHAR(255) | Hash hasła |
| `first_name` | VARCHAR(100) | Imię |
| `last_name` | VARCHAR(100) | Nazwisko |
| `phone` | VARCHAR(20) | Numer telefonu |
| `roles` | JSON | Role użytkownika |
| `is_active` | BOOLEAN | Czy konto jest aktywne |
| `created_at` | TIMESTAMP | Data rejestracji |
| `updated_at` | TIMESTAMP | Data aktualizacji |

## 1.2. `contractor_profile` – profile fachowców

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator profilu |
| `user_id` | BIGINT, FK, UNIQUE | Właściciel profilu |
| `display_name` | VARCHAR(150) | Nazwa fachowca lub firmy |
| `description` | TEXT | Opis działalności |
| `business_type` | VARCHAR(20) | Osoba prywatna / firma |
| `city` | VARCHAR(100) | Miasto |
| `latitude` | DECIMAL(10,7) | Szerokość geograficzna |
| `longitude` | DECIMAL(10,7) | Długość geograficzna |
| `service_radius_km` | INTEGER | Promień świadczenia usług |
| `verification_status` | VARCHAR(20) | Status weryfikacji |
| `created_at` | TIMESTAMP | Data utworzenia |
| `updated_at` | TIMESTAMP | Data aktualizacji |

## 1.3. `category` – kategorie usług

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator kategorii |
| `parent_id` | BIGINT, FK, NULL | Kategoria nadrzędna |
| `name` | VARCHAR(150) | Nazwa kategorii |
| `slug` | VARCHAR(160), UNIQUE | Identyfikator tekstowy |
| `description` | TEXT, NULL | Opis kategorii |
| `is_active` | BOOLEAN | Czy kategoria jest aktywna |

## 1.4. `service_offer` – usługi fachowców

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator usługi |
| `contractor_id` | BIGINT, FK | Profil fachowca |
| `category_id` | BIGINT, FK | Kategoria usługi |
| `title` | VARCHAR(200) | Nazwa usługi |
| `description` | TEXT | Szczegółowy opis |
| `price_from` | DECIMAL(12,2), NULL | Cena początkowa |
| `price_unit` | VARCHAR(20) | Godzina / m² / usługa |
| `service_mode` | VARCHAR(20) | Lokalnie / zdalnie / obie |
| `is_active` | BOOLEAN | Czy oferta jest aktywna |
| `created_at` | TIMESTAMP | Data utworzenia |
| `updated_at` | TIMESTAMP | Data aktualizacji |

## 1.5. `tag` – tagi kompetencji

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator tagu |
| `name` | VARCHAR(100) | Nazwa tagu |
| `slug` | VARCHAR(120), UNIQUE | Identyfikator tekstowy |

## 1.6. `service_offer_tag` – powiązania usług z tagami

| Pole | Typ | Opis |
|---|---|---|
| `service_offer_id` | BIGINT, FK | Identyfikator usługi |
| `tag_id` | BIGINT, FK | Identyfikator tagu |

Klucz główny złożony: (`service_offer_id`, `tag_id`).

## 1.7. `job_request` – zlecenia klientów

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator zlecenia |
| `client_id` | BIGINT, FK | Zleceniodawca |
| `category_id` | BIGINT, FK | Kategoria zlecenia |
| `title` | VARCHAR(200) | Tytuł zlecenia |
| `description` | TEXT | Szczegółowy opis |
| `budget_min` | DECIMAL(12,2), NULL | Minimalny budżet |
| `budget_max` | DECIMAL(12,2), NULL | Maksymalny budżet |
| `is_negotiable` | BOOLEAN | Czy cena podlega negocjacji |
| `location_city` | VARCHAR(100), NULL | Miasto realizacji |
| `latitude` | DECIMAL(10,7), NULL | Szerokość geograficzna |
| `longitude` | DECIMAL(10,7), NULL | Długość geograficzna |
| `service_mode` | VARCHAR(20) | Lokalnie / zdalnie |
| `urgency` | VARCHAR(20) | Stopień pilności |
| `preferred_deadline` | DATE, NULL | Preferowany termin |
| `status` | VARCHAR(20) | Status zlecenia |
| `created_at` | TIMESTAMP | Data publikacji |
| `updated_at` | TIMESTAMP | Data aktualizacji |

## 1.8. `attachment` – załączniki

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator pliku |
| `storage_path` | VARCHAR(500) | Ścieżka do pliku |
| `original_name` | VARCHAR(255) | Oryginalna nazwa |
| `mime_type` | VARCHAR(100) | Typ MIME |
| `size_bytes` | BIGINT | Rozmiar pliku |
| `uploaded_by_id` | BIGINT, FK | Użytkownik przesyłający |
| `created_at` | TIMESTAMP | Data przesłania |

## 1.9. `job_attachment` – załączniki zleceń

| Pole | Typ | Opis |
|---|---|---|
| `job_id` | BIGINT, FK | Identyfikator zlecenia |
| `attachment_id` | BIGINT, FK | Identyfikator załącznika |

Klucz główny złożony: (`job_id`, `attachment_id`).

## 1.10. `portfolio_item` – realizacje fachowców

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator realizacji |
| `contractor_id` | BIGINT, FK | Profil fachowca |
| `title` | VARCHAR(200) | Tytuł realizacji |
| `description` | TEXT | Opis wykonanej pracy |
| `completed_date` | DATE, NULL | Data realizacji |
| `created_at` | TIMESTAMP | Data dodania |

## 1.11. `portfolio_attachment` – zdjęcia portfolio

| Pole | Typ | Opis |
|---|---|---|
| `portfolio_item_id` | BIGINT, FK | Identyfikator realizacji |
| `attachment_id` | BIGINT, FK | Identyfikator pliku |
| `image_role` | VARCHAR(20) | Przed / po / inne |
| `sort_order` | INTEGER | Kolejność wyświetlania |

Klucz główny złożony: (`portfolio_item_id`, `attachment_id`).

## 1.12. `bid` – oferty składane na zlecenia

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator oferty |
| `job_id` | BIGINT, FK | Zlecenie |
| `contractor_id` | BIGINT, FK | Fachowiec |
| `proposed_price` | DECIMAL(12,2) | Proponowana cena |
| `proposed_date` | DATE, NULL | Proponowany termin |
| `message` | TEXT | Opis oferty |
| `status` | VARCHAR(20) | Status oferty |
| `created_at` | TIMESTAMP | Data złożenia |
| `updated_at` | TIMESTAMP | Data aktualizacji |

## 1.13. `contract` – realizowane zlecenia

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator kontraktu |
| `job_id` | BIGINT, FK, UNIQUE | Zlecenie |
| `accepted_bid_id` | BIGINT, FK, UNIQUE | Zaakceptowana oferta |
| `agreed_price` | DECIMAL(12,2) | Uzgodniona cena |
| `scheduled_date` | DATE, NULL | Planowany termin realizacji |
| `status` | VARCHAR(20) | Status realizacji |
| `started_at` | TIMESTAMP, NULL | Data rozpoczęcia |
| `completed_at` | TIMESTAMP, NULL | Data zakończenia |
| `created_at` | TIMESTAMP | Data zawarcia kontraktu |

## 1.14. `review` – oceny i opinie

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator opinii |
| `contract_id` | BIGINT, FK, UNIQUE | Zrealizowane zlecenie |
| `author_id` | BIGINT, FK | Autor opinii |
| `rating` | SMALLINT | Ocena od 1 do 5 |
| `comment` | TEXT, NULL | Treść opinii |
| `created_at` | TIMESTAMP | Data wystawienia |

## 1.15. `conversation` – konwersacje

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator konwersacji |
| `job_id` | BIGINT, FK | Zlecenie |
| `client_id` | BIGINT, FK | Zleceniodawca |
| `contractor_id` | BIGINT, FK | Fachowiec |
| `created_at` | TIMESTAMP | Data rozpoczęcia |

Zalecane ograniczenie unikalności: (`job_id`, `contractor_id`).

## 1.16. `message` – wiadomości

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator wiadomości |
| `conversation_id` | BIGINT, FK | Konwersacja |
| `sender_id` | BIGINT, FK | Nadawca |
| `content` | TEXT | Treść wiadomości |
| `created_at` | TIMESTAMP | Data wysłania |
| `read_at` | TIMESTAMP, NULL | Data odczytania |

# 2. Tabele dodatkowe

## 2.1. `notification` – powiadomienia

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator powiadomienia |
| `user_id` | BIGINT, FK | Odbiorca |
| `type` | VARCHAR(50) | Typ powiadomienia |
| `title` | VARCHAR(200) | Tytuł |
| `content` | TEXT | Treść |
| `related_entity_type` | VARCHAR(50), NULL | Typ powiązanego obiektu |
| `related_entity_id` | BIGINT, NULL | Identyfikator obiektu |
| `is_read` | BOOLEAN | Czy przeczytane |
| `created_at` | TIMESTAMP | Data utworzenia |
| `read_at` | TIMESTAMP, NULL | Data odczytania |

## 2.2. `job_status_history` – historia statusów zleceń

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator wpisu |
| `job_id` | BIGINT, FK | Zlecenie |
| `old_status` | VARCHAR(20), NULL | Poprzedni status |
| `new_status` | VARCHAR(20) | Nowy status |
| `changed_by_id` | BIGINT, FK | Użytkownik dokonujący zmiany |
| `changed_at` | TIMESTAMP | Data zmiany |
| `comment` | TEXT, NULL | Uzasadnienie |

## 2.3. `contract_status_history` – historia realizacji

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator wpisu |
| `contract_id` | BIGINT, FK | Kontrakt |
| `old_status` | VARCHAR(20), NULL | Poprzedni status |
| `new_status` | VARCHAR(20) | Nowy status |
| `changed_by_id` | BIGINT, FK | Użytkownik dokonujący zmiany |
| `changed_at` | TIMESTAMP | Data zmiany |
| `comment` | TEXT, NULL | Komentarz |

## 2.4. `contractor_verification` – weryfikacja fachowców

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator weryfikacji |
| `contractor_id` | BIGINT, FK | Profil fachowca |
| `status` | VARCHAR(20) | Oczekująca / zaakceptowana / odrzucona |
| `verification_type` | VARCHAR(50) | Rodzaj weryfikacji |
| `verified_by_id` | BIGINT, FK, NULL | Administrator |
| `submitted_at` | TIMESTAMP | Data zgłoszenia |
| `verified_at` | TIMESTAMP, NULL | Data decyzji |
| `notes` | TEXT, NULL | Uwagi administratora |

## 2.5. `report` – zgłoszenia naruszeń

| Pole | Typ | Opis |
|---|---|---|
| `id` | BIGINT, PK | Identyfikator zgłoszenia |
| `reporter_id` | BIGINT, FK | Zgłaszający |
| `reported_user_id` | BIGINT, FK, NULL | Zgłoszony użytkownik |
| `job_id` | BIGINT, FK, NULL | Zgłoszone zlecenie |
| `reason` | VARCHAR(100) | Powód zgłoszenia |
| `description` | TEXT | Opis problemu |
| `status` | VARCHAR(20) | Nowe / rozpatrywane / zamknięte |
| `resolved_by_id` | BIGINT, FK, NULL | Administrator |
| `created_at` | TIMESTAMP | Data zgłoszenia |
| `resolved_at` | TIMESTAMP, NULL | Data rozpatrzenia |

# 3. Podsumowanie

| Obszar | Tabele |
|---|---|
| Użytkownicy | `user`, `contractor_profile` |
| Katalog usług | `category`, `service_offer`, `tag`, `service_offer_tag` |
| Zlecenia | `job_request`, `attachment`, `job_attachment` |
| Portfolio | `portfolio_item`, `portfolio_attachment` |
| Ofertowanie | `bid`, `contract` |
| Opinie | `review` |
| Komunikacja | `conversation`, `message` |
| Powiadomienia | `notification` |
| Historia zmian | `job_status_history`, `contract_status_history` |
| Administracja | `contractor_verification`, `report` |

**Łącznie: 21 tabel (16 podstawowych i 5 dodatkowych).**

# 4. Najważniejsze ograniczenia integralności

Przy implementacji należy uwzględnić:

1. Unikalność adresu `user.email`.
2. Jeden profil fachowca na użytkownika (`contractor_profile.user_id`).
3. Unikalność tagów i kategorii według pola `slug`.
4. Brak duplikatów w tabelach łączących relacje wiele-do-wielu.
5. Dodatnie ceny ofert i usług oraz nieujemne budżety.
6. Warunek `budget_min <= budget_max`, jeśli obie wartości zostały podane.
7. Oceny wyłącznie w zakresie 1–5.
8. Możliwość wystawienia opinii wyłącznie przez uprawnionego klienta po zakończeniu zlecenia.
9. Akceptację oferty wyłącznie dla opublikowanego, dostępnego zlecenia.
10. Tylko jeden kontrakt dla danego zlecenia.
11. Zgodność `contract.accepted_bid_id` ze zleceniem wskazanym w `contract.job_id`.
12. Składanie ofert wyłącznie przez aktywnych, uprawnionych fachowców.
13. Dostęp do wiadomości wyłącznie dla uczestników konwersacji.
14. Kontrolę dopuszczalnych zmian statusów zleceń, ofert i kontraktów.

Ograniczenia wymagające sprawdzenia wielu rekordów lub uprawnień użytkowników powinny być egzekwowane w logice biznesowej aplikacji. Operacje akceptowania ofert należy wykonywać transakcyjnie, z odpowiednią kontrolą współbieżności.
