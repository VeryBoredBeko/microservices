# OrderService — Техникалық тапсырма

## Жоба: Food To Go

### 1. Мақсат

`OrderService` сервис класын жасау, ол тапсырысты өңдеу бизнес-логикасын жүзеге асырады: тапсырыстың жалпы құнын есептеу, жеңілдіктерді қолдану, жеткізудің қолжетімділігін тексеру және жеткізу уақытын есептеу.

Класс таза болуы керек, дерекқормен, желімен немесе сыртқы сервистермен байланыссыз, толықтай юнит-тесттермен тексерілуі мүмкін.

---

### 2. Сервистің негізгі функциялары

#### 2.1. Тапсырысты есептеу

Метод: `double calculateTotal(List<String> itemNames)`

* Тапсырыстың жалпы құнын есептейді.
* Меню деректері жадта (`Map<String, Double>`) сақталады.
* Егер тағам менюде жоқ болса, `IllegalArgumentException` лақтырады.
* Егер жалпы сумма ≥ 5000₸ болса — 10% жеңілдік, ≥ 10000₸ болса — 15% жеңілдік қолданылады.
* Жеңілдік есептелген соңғы соманы қайтарады.

#### 2.2. Жеткізу мүмкіндігін тексеру

Метод: `boolean canDeliverTo(String city)`

* Қол жетімді қалалар тізімі: `Astana`, `Almaty`, `Shymkent`.
* Егер қала қол жетімді болса — `true`, әйтпесе — `false`.
* Салыстыру регистрге тәуелсіз болуы керек.

#### 2.3. Жеткізу уақытын есептеу

Метод: `int estimateDeliveryTime(String city, int itemCount)`

* Негізгі уақыт: Astana — 20 мин, Almaty — 30 мин, Shymkent — 40 мин.
* 3-тен көп тауар үшін әр элементке +5 минут қосылады.
* Қала қол жетімсіз болса — `IllegalArgumentException`.

#### 2.4. Жеңілдіктерді қолдану

Метод: `double applyDiscount(double total)`

* ≥ 5000₸ → 10% жеңілдік
* ≥ 10000₸ → 15% жеңілдік
* Басқа жағдайларда — жеңілдік қолданылмайды

---

### 3. Класс құрылымы

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

### 4. Тестілеу талаптары

* Юнит-тесттер моктар мен сыртқы тәуелділіктерсіз болуы керек.
* Тест класы: `OrderServiceTest`.

Міндетті сценарийлер:

| Метод                  | Тест сценарийлері                                                          |
| ---------------------- | -------------------------------------------------------------------------- |
| calculateTotal()       | Дұрыс есептеу, 10% жеңілдік, 15% жеңілдік, белгісіз тағам үшін қателік     |
| applyDiscount()        | Барлық жеңілдік диапазондарын тексеру                                      |
| canDeliverTo()         | Қала қол жетімді, қала қол жетімсіз, әртүрлі регистр                       |
| estimateDeliveryTime() | Әр негізгі қала, қол жетімсіз қала, қосымша элементтер үшін +5 мин есептеу |

---

### 5. Бағалау критерийлері

| Критерий                        | Балл |
| ------------------------------- | ---- |
| Бизнес-логиканың дұрыстығы      | 4    |
| Тесттермен толық қамтылуы       | 4    |
| JUnit 5 дұрыс қолданылуы        | 1    |
| Код тазалығы және әдіс атаулары | 1    |
| **Барлығы**                     | 10   |

---

### 6. Қосымша тапсырма (міндетті емес)

Метод: `String getOrderSummary(List<String> items, String city)`

* Мысалы, келесі жолды қайтарады:
  `Order to Almaty: 3 items, total = 5400₸ (10% discount applied)
  Estimated delivery time: 35 minutes`
* Реализованные методтар негізінде тестілеу керек.
