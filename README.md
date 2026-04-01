

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

$$V_{\text{redundant\_mongo}} = (0.85N + 170) + (1.65N + 330) + 62N = 64.5N + 500$$

---

**Доля логической избыточности:**

$$R_{\text{logical\_mongo}} = \frac{V_{\text{redundant\_mongo}}}{V_{\text{mongo}}} = \frac{64.5N + 500}{6605N + 9180}$$

При больших $N$:

$$R_{\text{logical\_mongo}} \to \frac{64.5}{6605} \approx 0.98\%$$

---
