$$V_{\text{mongo}} = P \times 857 + N \times 6562 + B \times 918$$

Подставляя P = N/20, B = 10:

$$V_{\text{mongo}} = \frac{N}{20} \times 857 + N \times 6562 + 10 \times 918$$

$$V_{\text{mongo}} = 42.85N + 6562N + 9180$$

$$\boxed{V_{\text{mongo}} \approx 6605N + 9180 \text{ байт}}$$



**Объём логически избыточных данных (MongoDB):**

Учитываются только размеры самих значений полей, без накладных расходов формата BSON (типы, имена ключей, служебные байты).

1. `total_games` (Int32 — 4 байта):

$$V_{total} = 4 \cdot (P + B) = 4 \cdot \left(\frac{N}{20} + 10\right) = 0.2N + 40$$

2. `wins` (4), `losses` (4), `draws` (4) — суммарно 12 байт на экземпляр:

$$V_{wld} = 12 \cdot (P + B) = 12 \cdot \left(\frac{N}{20} + 10\right) = 0.6N + 120$$

3. `player1_type`, `player2_type` (строковое значение ~12 байт каждое):

$$V_{type} = 2 \cdot 12 \cdot N = 24N$$

---

**Итого:**

$$V_{\text{redundantMongo}} = (0.2N + 40) + (0.6N + 120) + 24N = 24.8N + 160$$

---

**Доля логической избыточности:**

$$R_{\text{logicalMongo}} = \frac{V_{\text{redundantMongo}}}{V_{\text{mongo}}} = \frac{24.8N + 160}{6605N + 9180}$$

При больших $N$:

$$R_{\text{logicalMongo}} \to \frac{24.8}{6605} \approx 0.38\%$$

---
