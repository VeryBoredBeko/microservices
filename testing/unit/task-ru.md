# OrderService — Техническое задание

## Проект: Food To Go

### 1. Цель

Разработать сервисный класс `OrderService`, реализующий бизнес-логику обработки заказов: расчет итоговой стоимости, применение скидок, проверка доступности доставки и расчет времени доставки.

Класс должен быть чистым, без взаимодействия с базой данных, сетью или внешними сервисами, чтобы полностью покрываться юнит-тестами.

---

### 2. Основные функции сервиса

#### 2.1. Формирование заказа

Метод: `double calculateTotal(List<String> itemNames)`

* Рассчитывает итоговую стоимость заказа.
* Данные о меню хранятся в памяти (`Map<String, Double>`).
* Если блюдо отсутствует, выбрасывать `IllegalArgumentException`.
* Если сумма ≥ 5000₸ — скидка 10%, ≥ 10000₸ — скидка 15%.
* Возвращает сумму с учетом скидки.

#### 2.2. Проверка доступности доставки

Метод: `boolean canDeliverTo(String city)`

* Список доступных городов: `Astana`, `Almaty`, `Shymkent`.
* Возвращает `true`, если город доступен, иначе `false`.
* Сравнение должно быть регистронезависимым.

#### 2.3. Расчет времени доставки

Метод: `int estimateDeliveryTime(String city, int itemCount)`

* Базовое время: Astana — 20 мин, Almaty — 30 мин, Shymkent — 40 мин.
* За каждый элемент сверх 3-х добавлять +5 минут.
* Если город недоступен — `IllegalArgumentException`.

#### 2.4. Применение скидок

Метод: `double applyDiscount(double total)`

* ≥ 5000₸ → 10% скидка
* ≥ 10000₸ → 15% скидка
* Иначе — без скидки

---

### 3. Структура класса

```java
public class OrderService {
    private final Map<String, Double> menu;
    private final List<String> supportedCities;

    public OrderService() {
        menu = new HashMap<>();
        menu.put("Burger", 2000.0);
        menu.put("Pizza", 2500.0);
        menu.put("Soda", 500.0);
        menu.put("Fries", 700.0);

        supportedCities = List.of("Astana", "Almaty", "Shymkent");
    }

    public double calculateTotal(List<String> itemNames) { ... }
    public double applyDiscount(double total) { ... }
    public boolean canDeliverTo(String city) { ... }
    public int estimateDeliveryTime(String city, int itemCount) { ... }
}
```

---

### 4. Требования к тестированию

* Юнит-тесты без моков и внешних зависимостей.
* Тестовый класс: `OrderServiceTest`.

Обязательные сценарии:

| Метод                  | Тестовые сценарии                                                              |
| ---------------------- | ------------------------------------------------------------------------------ |
| calculateTotal()       | Корректный подсчет, скидка 10%, скидка 15%, исключение для неизвестного блюда  |
| applyDiscount()        | Проверка всех диапазонов скидки                                                |
| canDeliverTo()         | Город из списка, город не из списка, разный регистр                            |
| estimateDeliveryTime() | Каждый базовый город, недоступный город, расчёт +5 мин за дополнительные блюда |

---

### 5. Критерии оценки

| Критерий                         | Баллы |
| -------------------------------- | ----- |
| Корректность бизнес-логики       | 4     |
| Полное покрытие тестами          | 4     |
| Корректное использование JUnit 5 | 1     |
| Чистота кода и имена методов     | 1     |
| **Итого**                        | 10    |

---

### 6. Дополнительное задание (опционально)

Метод: `String getOrderSummary(List<String> items, String city)`

* Возвращает строку: `Order to Almaty: 3 items, total = 5400₸ (10% discount applied)
  Estimated delivery time: 35 minutes`
* Тестировать на основе реализованных методов.
