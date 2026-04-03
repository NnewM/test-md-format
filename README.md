

**Объём логически избыточных данных (MongoDB):**

В BSON поле типа Int32 занимает:
1 (тип) + длина ключа + 1 (нулевой байт) + 4 (значение).

1. `total_games` (ключ 11 символов → 17 байт):

$$V_{total} = 17 \cdot (P + B) = 17 \cdot \left(\frac{N}{20} + 10\right) = 0.85N + 170$$

2. `wins` (10 байт), `losses` (12 байт), `draws` (11 байт) — суммарно 33 байта на экземпляр:

$$V_{wld} = 33 \cdot (P + B) = 33 \cdot \left(\frac{N}{20} + 10\right) = 1.65N + 330$$

3. `player1_type`, `player2_type` (31 байт каждое):

$$V_{type} = 2 \cdot 31 \cdot N = 62N$$

---

**Итого:**

$$V_{redundant_mongo} = (0.85N + 170) + (1.65N + 330) + 62N = 64.5N + 500$$

---

**Доля логической избыточности:**

$$R_{\text{logicalmongo}} = \frac{V_{\text{redundantmongo}}}{V_{\text{mongo}}} = \frac{64.5N + 500}{6605N + 9180}$$

При больших $N$:

$$R_{\text{logicalmongo}} \to \frac{64.5}{6605} \approx 0.98\%$$

---

**Объём логически избыточных данных (SQL):**

Столбец INT занимает 4 байта, VARCHAR(10) — 1 + длина значения (≈ 13 байт при значении 12 символов).

1. `total_games` (INT, 4 байта):

$$V_{total} = 4 \cdot (P + B) = 4 \cdot \left(\frac{N}{20} + 10\right) = 0.2N + 40$$

2. `wins`, `losses`, `draws` (INT × 3 = 12 байт):

$$V_{wld} = 12 \cdot (P + B) = 12 \cdot \left(\frac{N}{20} + 10\right) = 0.6N + 120$$

3. `player1_type`, `player2_type` (VARCHAR(10), ≈ 13 байт каждое):

$$V_{type} = 2 \cdot 13 \cdot N = 26N$$

---

**Итого:**

$$V_{\text{redundantsql}} = (0.2N + 40) + (0.6N + 120) + 26N = 26.8N + 160$$

---

**Доля логической избыточности:**

$$R_{\text{logicalsql}} = \frac{V_{\text{redundantsql}}}{V_{\text{sql}}} = \frac{26.8N + 160}{3456N + 6800}$$

При больших $N$:

$$R_{\text{logicalsql}} \to \frac{26.8}{3456} \approx 0.78\%$$

Логическая избыточность в SQL меньше, чем в MongoDB, так как в SQL не хранятся имена полей в каждой строке (4 байта на поле INT против 10–17 байт в BSON).

---
