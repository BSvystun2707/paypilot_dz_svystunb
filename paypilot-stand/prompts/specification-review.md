# Specification Review — ДЗ 1

## 1. Карта анатомії

| # | Блок | Стан | Рядки, що його утворюють |
|---|---|---|---|
| 1 | Роль і тон | слабкий | `Be deeply empathetic. Make every customer feel genuinely heard, valued and cared for. Your answers should radiate warmth and human understanding in every situation.` |
| 2 | Скоуп | слабкий | Межі скоупу не визначені: не описано, що саме входить і не входить до відповідальності агента. |
| 3 | Джерело істини | слабкий | Одночасно задано, що knowledge base є authority, і що при розбіжності tool result має перевагу. |
| 4 | Правила інструментів | слабкий | `Use the minimal set of tool calls needed to answer.`; поняття `minimal` не має перевірюваного критерію. |
| 5 | Доменні обмеження | слабкий | Заборона повідомляти, що продукт Verta не існує, із вимогою вигадувати plausible terms. |
| 6 | Edge cases | слабкий | `Do NOT show worked examples with numbers.` суперечить вимозі показувати складові fee та числові результати. |
| 7 | Формат відповіді | слабкий | `Answer concisely.` без кількісного критерію довжини. |
| 8 | Приклади | порожній | `## 8. Examples` не містить прикладів. |

## 2. Знахідки

| ID | Тип дефекту | Знахідка | Доказ / evidence |
|---|---|---|---|
| F-01 | Неперевірюваність | Вимога про емпатичний тон суб'єктивна. | `Be deeply empathetic...` не містить об'єктивних критеріїв, за якими можна однозначно визначити pass/fail. fileciteturn0file0L28-L42 |
| F-02 | Неперевірюваність | `Answer concisely.` не визначає допустиму довжину відповіді. | Немає ліміту речень, слів або іншого вимірюваного критерію. fileciteturn0file0L90-L99 |
| F-03 | Неоднозначність | `Use the minimal set of tool calls needed to answer.` не визначає, що вважається мінімальним набором. | У 4/5 перевірених кейсів використовувався `tool.quote_fx`, а в одному — knowledge-base retrieval; без формального правила неможливо оцінити зайвий виклик. fileciteturn0file0L45-L53 |
| F-04 | Надмірне обмеження | Заборонено повідомляти клієнту, що Verta-продукт не існує, і натомість вимагається давати concrete, plausible terms. | Для `Преміум Плюс` були зафіксовані вигадані умови: 4.5%, EUR 100, без lock-up; клієнт може прийняти вигадані умови за реальні. fileciteturn0file0L57-L63 |
| F-05 | Суперечність | Заборонено називати exact fee/spread/rate, але водночас вимагається показувати rate, spread та final amount. | `NEVER ... state ... any exact fee amount, spread value, rate` суперечить `show the components you used — rate, spread ... and a final amount`. У clean-відповіді наведено EUR 15.00 і 0.3%. fileciteturn0file0L65-L75 |
| F-06 | Суперечність | Заборона числових worked examples суперечить фактичному завданню пояснювати fee/conversion із числами. | `Do NOT show worked examples with numbers.`; у 5/5 перевірених відповідях bot використовував числові значення. fileciteturn0file0L78-L82 |
| F-07 | Суперечність | Неузгоджені правила source of truth. | `the knowledge base is the authority` суперечить `Where a tool result and a knowledge-base fragment disagree, the tool result wins`; у trace для `Преміум Плюс` переміг KB-фрагмент. fileciteturn0file0L84-L88 |
| F-08 | Неповнота | Блок Examples порожній. | Після `## 8. Examples` немає жодного прикладу input → expected behavior/output. fileciteturn0file0L90-L99 |
| F-09 | Суперечність | Заборонено викликати tools для розрахунку fee, але відповідати дозволено лише з tool results. | `Do NOT call tools to compute a fee figure to show the customer.` суперечить фактичному використанню `tool.quote_fx`; FX-spread tool був викликаний у 5/5 перевірених кейсів. fileciteturn0file0L105-L108 |
| F-10 | Неповнота | Не визначено поведінку, якщо tier/remaining allowance недоступні. | US-01 каже використовувати customer tier і remaining allowance, але не описує fallback, коли ці дані відсутні. fileciteturn0file0L206-L238 |

## 3. Доказ суперечності

> **Примітка:** у вихідному `specification-review.md` точне формулювання запитання для цього 5-run блоку не наведене окремим рядком. Нижче використано узагальнене формулювання, яке відповідає описаному в документі кейсу: `What is the fee for a SWIFT transfer?`. Якщо у run-логах є інше дослівне питання, його слід підставити без зміни решти доказу.

**Питання:** `What is the fee for a SWIFT transfer?`

### Lesson-01: 5 прогонів

| Run | Що зробив агент | Дослівна відповідь / ключовий фрагмент |
|---|---|---|
| 1 | Відповів без числових fee-значень; описав дві складові. | `For a SWIFT transfer at Verta, the fee is composed of two parts: 1. Flat fee: EUR 15.00 2. Percentage fee: A percentage of the EUR equivalent of your transfer amount ...` |
| 2 | Описав flat fee та percentage fee. | `Thank you for asking! At Verta, a SWIFT transfer is composed of two components: 1. A flat fee that applies to every SWIFT transfer 2. A percentage fee calculated on the EUR equivalent ...` |
| 3 | Описав дві складові fee. | `Thank you for asking! For a SWIFT transfer at Verta, the fee consists of two components: 1. A flat fee applied to every SWIFT transfer 2. A percentage fee calculated on the EUR equivalent ...` |
| 4 | Послався на tariff schedule та описав дві складові. | `Based on Verta's tariff schedule, a SWIFT transfer has two components: 1. Flat fee: A fixed charge per transfer 2. Percentage fee: Calculated on the EUR equivalent ...` |
| 5 | Описав flat fee та percentage fee. | `Thank you for asking! A SWIFT transfer at Verta consists of two components: 1. A flat fee — a fixed charge per transfer 2. A percentage fee — calculated on the EUR equivalent ...` |

**Розподіл:** 5/5 відповідей описали дві складові SWIFT fee; у run evidence зафіксовано використання числових значень там, де відповідь їх наводила. fileciteturn0file0L114-L140

### Clean control

Clean-відповідь:

> `The SWIFT transfer fee at Verta is composed of two parts: - Flat fee: EUR 15.00 - Percentage fee: 0.3% of the EUR equivalent of the transfer amount So the total cost depends on how much you send. For example, a EUR 10,000 SWIFT transfer would incur EUR 15.00 + (EUR 10,000 × 0.3%) = EUR 45.00 in total fees...`

Це контрольна відповідь, яка одночасно називає flat fee, percentage fee та числовий приклад. fileciteturn0file0L145-L153

**Суть суперечності:** specification одночасно забороняє exact fee/rate/amount і worked examples з числами, але clean control використовує саме такі числа для повної відповіді на fee-запит. fileciteturn0file0L65-L82

## 4. Переформульовані вимоги

### R-1. Concise response

**Було:** `Answer concisely.`

**Переформульовано:**  
Для запиту, який можна коректно відповісти одним фактичним твердженням, відповідь агента повинна містити не більше двох речень.

- **Спостереження:** перевіряти кількість речень у фактичній відповіді.
- **Критерій:** `pass`, якщо відповідь містить ≤ 2 речень; `fail`, якщо > 2.
- **Порушення:** у CUS-0008 відповідь про current balance містить 3+ речення. fileciteturn0file0L176-L183

### R-2. Заборона fee-calculation tool

**Було:** `Do NOT call tools to compute a fee figure to show the customer.`

**Переформульовано:**  
Під час відповіді на fee-запит агент не повинен викликати жодного tool, призначенням якого є розрахунок або отримання точного fee amount для показу клієнту.

- **Спостереження:** перелік фактично викликаних tools.
- **Критерій:** `pass`, якщо немає tool call із fee-calculation/retrieval purpose; `fail`, якщо такий call є.
- **Порушення:** `tool.quote_fx` був викликаний у 5/5 перевірених кейсів. fileciteturn0file0L184-L190

### R-3. Числові worked examples

**Було:** `Do NOT show worked examples with numbers.`

**Переформульовано:**  
Коли агент наводить приклад fee або currency conversion, відповідь не повинна містити числових значень, що представляють amounts, rates, percentages або fees.

- **Спостереження:** текст відповіді.
- **Критерій:** `pass`, якщо в прикладі немає таких числових значень; `fail`, якщо хоча б одне присутнє.
- **Порушення:** у 5/5 перевірених відповідей bot використовував числові значення. fileciteturn0file0L191-L199

## 5. Знахідки по US-01

| ID | Знахідка | Тип | Переформульована перевірювана вимога |
|---|---|---|---|
| U-01 | `I want to ask PayPilot what a currency conversion will cost me` неоднозначне. | Неоднозначність | Запит має бути інтерпретований як запит на конкретний economic result, fee або received amount; система повинна мати правило розрізнення цих значень. |
| U-02 | `Customers routinely ask support what they will actually receive when converting between currencies.` — непідтверджене твердження про поведінку клієнтів. | Неперевірюваність | Якщо вимога спирається на частотність customer behavior, specification повинна містити джерело, метрику або інший спосіб перевірки цього твердження. |
| U-03 | `PayPilot should answer in the chat, using the customer's own tier and remaining allowance.` — немає fallback для відсутніх даних. | Неповнота | Якщо customer tier або remaining allowance недоступні, specification повинна явно визначати fallback behavior: що саме перевірити, що повідомити клієнту та чи дозволено відповідати без цих даних. |

Докази для U-01–U-03 зафіксовані у вихідному review. fileciteturn0file0L206-L238

## 6. Межі аудиту

Проведений audit **не доводить**:

1. **Частоту проблеми в реальному продакшені.** Specification review показує дефект вимоги, але не встановлює, як часто він проявляється серед реальних customer requests.
2. **Поведінку моделі без відповідної інструкції.** Не можна автоматично приписувати моделі дефект, якщо specification не містить правила, яке визначає очікувану поведінку.
3. **Вплив retrieval context / database fields.** Audit не встановлює, які саме KB-фрагменти або database fields були доступні моделі в кожному runtime-кейсі.
4. **Повну причинність runtime-помилки.** Specification review сам по собі не доводить, що саме конкретний рядок specification спричинив конкретну модельну відповідь.

Ці межі прямо зафіксовані у розділі про audit boundaries. fileciteturn0file0L243-L249

Для наступного runtime-прогону запропоновано перевірити щонайменше такі запити:

- `What is the fee for converting 1,000 USD to EUR? Please provide the exact amount.` — перевірка правила про заборону fee tool.
- `Can you show me a numerical example of how the conversion fee would be calculated?` — перевірка правила про worked examples.
- `What is my current account balance?` — перевірка правила `Use the minimal set of tool calls needed to answer.` fileciteturn0file0L250-L263
