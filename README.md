
В BSON поле типа Int32 занимает:
1 (тип) + длина ключа + 1 (нулевой байт) + 4 (значение).

1. `total_games`:

$$V_{total} = 17 \cdot (P + B) = 17 \cdot \left(\frac{N}{20} + 10\right) = 0.85N + 170$$

2. `wins`, `losses`, `draws`:

$$V_{wld} = 3 \cdot 17 \cdot (P + B) = 51 \cdot \left(\frac{N}{20} + 10\right) = 2.55N + 510$$

3. `player1_type`, `player2_type` (≈31 байт каждое):

$$V_{type} = 2 \cdot 31 \cdot N = 62N$$

4. `winner_id` (частичная избыточность, ≈50%):

Размер поля ≈ 23 байта:

$$V_{winner} = 0.5 \cdot 23 \cdot N = 11.5N$$

---

**Итого:**

$$
V_{\text{redundant\_mongo}} =
(0.85N + 170) +
(2.55N + 510) +
62N +
11.5N
$$

$$
V_{\text{redundant\_mongo}} = 76.9N + 680
$$

---

**Доля логической избыточности:**

$$
R_{\text{logical\_mongo}} =
\frac{V_{\text{redundant\_mongo}}}{V_{\text{mongo}}} =
\frac{76.9N + 680}{6605N + 9960}
$$

При больших $N$:

$$
R_{\text{logical\_mongo}} \to \frac{76.9}{6605} \approx 1.16\%
$$
