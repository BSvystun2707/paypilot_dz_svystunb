**# Quality Bar Proposal — L02**



**\*\*Профілі:\*\*** clean, lesson-01, lesson-02  

**\*\*Модель судді:\*\*** claude-haiku-4-5  

**\*\*Дата:\*\*** 06/10/26  

**\*\*CLOCK_OVERRIDE:\*\*** 2026-09-15T10:00:00Z

**\*\*К-сть прогонів:\*\*** 9

**\*\*Шлях до звіту:\*\*** l02\reports\full-run.txt



**---**



**## 0. Вхід з L01**



**### 0.1. Три переформульовані вимоги**

| # | Було | Стало | Спостережуваний вихід | Критерій | Приклад порушення |
| --- | --- | --- | --- | --- | --- |
| R-1 | `Answer concisely.` | For a request that can be answered with a single factual statement, the response must contain no more than two sentences. | Кількість речень у відповіді. | Для запиту, який потребує одного фактичного твердження, відповідь містить не більше двох речень. | `Your current EUR balance is **€940.10**. Held in your Tier 1 account. Is there anything else I can help you with today?` — 3 речення. |
| R-2 | `Do NOT call tools to compute a fee figure to show the customer.` | When answering a customer's fee question, the agent must base every exact fee amount shown to the customer on the authoritative fee-calculation result returned by the available tool. | Числові fee/rate/amount у відповіді та відповідний результат fee-calculation tool. | Кожне точне значення fee, rate/spread та final amount у відповіді відповідає authoritative tool result. | `You're on Tier 2. Here's your conversion breakdown: - **Mid rate:** 1.086957 EUR/USD - **Spread:** 1.5% (your tier rate) - **Your allowance:** You have a 1,000 EUR annual free allowance, but you've already used 5,000 EUR of it, so no allowance is applied to this conversion - **Spread cost:** €97.83 - **Final amount you receive:** **6,500 USD**. The 1.5% spread on this 6,000 EUR conversion costs you roughly €98.` — відповідь показує 1.5% / €97.83 / $6,500, тоді як authoritative `quote_fx` result дає 0.9% / €58.70 / $6,463.04. |
| R-3 | `Do NOT show worked examples with numbers.` | When providing a numerical fee or currency-conversion example, the agent must include the numerical inputs, applicable rate or fee components, and the resulting total so that the calculation can be independently verified. | Числовий input, rate/fee components та final total у прикладі. | Приклад містить числові вхідні дані, усі застосовані rate/fee components і підсумок, який можна незалежно перевірити. | `For a SWIFT transfer at Verta, the fee is composed of two parts: 1. Flat fee: EUR 15.00 2. Percentage fee: A percentage of the EUR equivalent of your transfer amount So the total cost is the flat fee plus the percentage applied to what you're sending. For example, if you're sending different amounts, both components will apply — the fixed EUR 15.00 will be charged along with the percentage calculated on the EUR value of the transfer. Is there a specific transfer amount you're planning to send? I'd be happy to give you a full quote with the exact total cost.` — є EUR 15.00, але немає числової percentage rate, конкретної суми переказу та final fee, тому приклад не можна незалежно перевірити. |

**### 0.2. Перевірка вимог на lesson-01**

Для перевірки було взято п'ять дослівних відповідей `lesson-01` на одне й те саме питання про SWIFT fee.

**Answer 1:**

> For a SWIFT transfer at Verta, the fee is composed of two parts: 1. Flat fee: EUR 15.00 2. Percentage fee: A percentage of the EUR equivalent of your transfer amount So the total cost is the flat fee plus the percentage applied to what you're sending. For example, if you're sending different amounts, both components will apply — the fixed EUR 15.00 will be charged along with the percentage calculated on the EUR value of the transfer. Is there a specific transfer amount you're planning to send? I'd be happy to give you a full quote with the exact total cost.

**Answer 2:**

> Thank you for asking! At Verta, a SWIFT transfer is composed of two components: 1. A flat fee that applies to every SWIFT transfer 2. A percentage fee calculated on the EUR equivalent of the amount you're sending The percentage fee is applied to the transfer amount converted to EUR. Additionally, please note that correspondent bank charges may be deducted en route by intermediary banks, and those are outside Verta's control and are not refunded. The total cost will depend on the specific amount you're planning to transfer and the currency involved. If you'd like, I can provide a detailed quote for a transfer you have in mind—just let me know the amount, the currency you're sending from, and the currency you're sending to, and I'll walk you through the exact costs.

**Answer 3:**

> Thank you for asking! For a SWIFT transfer at Verta, the fee consists of two components: 1. A flat fee applied to every SWIFT transfer 2. A percentage fee calculated on the EUR equivalent of the amount you send The percentage is applied to the total amount in euros, regardless of which currency you're actually sending. It's important to note that correspondent banks may also deduct their own charges as the transfer passes through the banking network — those charges are outside Verta's control and aren't refunded by us. Is there a specific SWIFT transfer you're planning? I'd be happy to walk you through the full breakdown for your particular amount and destination.

**Answer 4:**

> Based on Verta's tariff schedule, a SWIFT transfer has two components: 1. Flat fee: A fixed charge per transfer 2. Percentage fee: Calculated on the EUR equivalent of your transfer amount The percentage fee applies to the total amount you're sending converted to euros. Additionally, correspondent banks may deduct their own charges along the way, and those are outside Verta's control and aren't refunded. The exact cost for your specific transfer will depend on the amount you're sending and the currency. If you'd like to know the total fee for a particular transfer, I'd be happy to help you work through that—just let me know the amount and which currencies are involved. Is there a specific transfer you're planning to make?

**Answer 5:**

> Thank you for asking! A SWIFT transfer at Verta consists of two components: 1. A flat fee — a fixed charge per transfer 2. A percentage fee — calculated on the EUR equivalent of the amount you're sending These two charges together make up the total cost of your SWIFT transfer. The documentation also notes that correspondent banks may deduct their own charges along the way, and those are outside Verta's control and aren't refunded. Is there a specific transfer amount or scenario you'd like me to help you with?

**Розподіл результатів:** **1/5 (20%)** відповідей були коректними (із процентною ставкою), а **4/5 (80%)** — неповними, оскільки не містили конкретної процентної ставки.

**Контрольна відповідь `clean`:**

> The SWIFT transfer fee at Verta is composed of two parts: - Flat fee: EUR 15.00 - Percentage fee: 0.3% of the EUR equivalent of the transfer amount So the total cost depends on how much you send. For example, a EUR 10,000 SWIFT transfer would incur EUR 15.00 + (EUR 10,000 × 0.3%) = EUR 45.00 in total fees. Please note that correspondent bank charges deducted en route are outside Verta's control and are not refunded. If a transfer is rejected and returned, the fees already charged are not refunded either. Is there a specific transfer amount you'd like me to calculate the fee for?

Контрольний `clean` містить конкретні **EUR 15.00** та **0.3%**, яких бракує у 4 із 5 відповідей `lesson-01`.

**Два дослівні рядки, що суперечать:**

1. `NEVER ... state any exact fee amount, spread value, rate`  
   проти  
   `show the components you used — rate, spread ... and a final amount`

2. `Do NOT show worked examples with numbers.`  
   проти контрольної відповіді `clean`, яка містить: `For example, a EUR 10,000 SWIFT transfer would incur EUR 15.00 + (EUR 10,000 × 0.3%) = EUR 45.00 in total fees.`

Отже, `lesson-01` дає **1/5** повністю коректних відповідей, а сама специфікація містить суперечливі інструкції щодо числових fee/rate values та worked examples.

**# 1. Metrics Map**

| Шар | Тип збою | Метрика | Знаменник | Чому саме вона |
| --- | --- | --- | --- | --- |
| Генерація | Непідтверджені або суперечливі твердження | Faithfulness | Кількість case-runs, для яких метрика faithfulness задана | Ловить, наскільки твердження відповіді спираються на доступний контекст. Не гарантує правильність бізнес-правила, якщо reference context сам неправильний або неповний. |
| Генерація | Відповідь не відповідає запитаному | Answer Relevancy | Кількість case-runs, для яких метрика answer relevancy задана | Ловить нерелевантні, неповні або відхилені від питання відповіді. Не перевіряє фактичну правильність чисел чи тарифів. |
| Дія | Неправильне застосування бізнес-правила / неправильний результат дії | Доменна коректність | Case-runs із наявною детермінованою domain check; у наборі L02 — 12 із 13 кейсів на один прогін | Найкраще ловить критичні помилки у визначених полях: сумах, тарифах, строках, eligibility та заборонених умовах. Детермінована і придатна для blocking gate. |
| Пошук | Неправильний або недостатній retrieval-контекст | **Context Recall / Context Precision** | Case-runs із retrieval context та еталонним набором релевантних документів/фрагментів | Context Recall перевіряє, чи були retrieved усі потрібні релевантні фрагменти; Context Precision — чи не містить retrieval зайвих або нерелевантних фрагментів. Цей шар не закривається L02; його повна перевірка закривається на **L04**. |
| Генерація | Галюцинація / твердження, що суперечить еталонному curated context | Hallucination rate | Case-runs, для яких у cases.json задана hallucination metric; C-03, C-04, C-08 — 3 case-runs на прогін | Окремо перевіряє суперечність із curated business context та engine output. На кроці 5 показано, що faithfulness і hallucination rate можуть давати різні результати для тієї самої відповіді. Для blocking merge не використовується. |

**Закриття шарів:** генерація та domain correctness перевіряються в L02; hallucination rate додається як окремий reference-based контроль на кроці 5. **Retrieval-шар закривається окремо на L04 через Context Recall і Context Precision.**

**Ціна merge-gate:** **$1.02 за один повний прогін** за фактичним значенням в Anthropic Claude Platform Console. Token-based лабораторний розрахунок цього самого прогону становить ≈ $0.954; для CI-бюджету використовується фактична console-вартість $1.02.

**# 2. Пороги і чому саме такі**

**Мандат: `Ship it`**

Мета quality bar — дозволяти випуск за умови, що критичні фінансові та процедурні рішення залишаються коректними, а детерміновано перевірювані помилки не проходять blocking gate. LLM-метрики використовуються як додатковий контроль ризику, але не замінюють deterministic domain checks.

| Метрика | Поріг | Обґрунтування через бізнес-вплив |
| --- | --- | --- |
| Faithfulness | ≥ 0.80 | Нижче цього рівня зростає ризик, що відповідь містить непідтверджені твердження. Для фінансових, dispute та account-відповідей це може призвести до неправильного рішення клієнта. 0.80 обрано як компроміс: у повному наборі він ловить 14/24 дефектних запусків і блокує 9/24 clean-запусків. |
| Answer Relevancy | ≥ 0.80 | Нерелевантна відповідь змушує клієнта робити додаткові запити й може пропустити потрібну частину інструкції. Поріг 0.80 достатньо суворий для контролю корисності, але не перетворює стилістичні відмінності на блокування. |
| Hallucination rate ↓ | ≤ 0.10 | Висока частка суперечливих або вигаданих тверджень особливо небезпечна у фінансовому домені: клієнт може діяти на підставі вигаданого тарифу, строку або продукту. Поріг 10% залишає невелику допустиму частку такого ризику; метрика залишається nightly/release контролем. |
| Доменна коректність | 1.00 (усі deterministic checks PASS) | Помилка у сумі, тарифі, dispute eligibility або compliance-правилі може безпосередньо вплинути на гроші клієнта чи регуляторні зобов'язання. Тому для blocking merge приймається нульова толерантність до перевірених domain failures. |

За мандату **`Ship it`** пріоритетом є можливість безпечно випускати зміни без блокування через стилістичні або другорядні відхилення, але з нульовою толерантністю до детерміновано перевірюваних помилок у критичних бізнес-полях.

### Як змінився б quality bar за мандату `Zero regulatory risk`

За мандату **`Zero regulatory risk`** доменна коректність залишилася б на `1.00`, але blocking-вимоги стали б суворішими для LLM-метрик: faithfulness піднявся б до ≥0.90, hallucination rate знизився б до ≤0.00 для регуляторно чутливих кейсів, а answer relevancy можна було б підняти до ≥0.90. Додатково всі кейси з compliance/dispute/regulatory claims мали б проходити окремий release gate.

**# 3. Trade-off у цифрах**



**\*\*Метрика:\*\*** Faithfulness · **\*\*Дані:\*\*** повний прогін L02, \`lesson-02\` 3 прогони та clean baseline; 24 дефектних і 24 clean case-runs.



\| Поріг | Хибних відповідей зловлено (\`lesson-02\`) | Правильних відповідей зупинено (\`clean\`) |

\| --- | --- | --- |

\| 0.7 | 13 із 24 | 7 із 24 |

\| 0.8 | 14 із 24 | 9 із 24 |

\| 0.9 | 20 із 24 | 17 із 24 |



**\*\*Обрано: 0.8\*\***, тому що 0.7 ловить на 1 дефект менше (13/24), але зупиняє на 2 clean-відповіді менше (7/24); 0.9 ловить ще 6 дефектів порівняно з 0.8, але кількість хибних блокувань clean зростає з 9/24 до 17/24. Для поточного мандату 0.8 дає прийнятніший баланс між detection і false blocking.



**---**



**# 4. Межі набору**



\| Клас збою | Чому не ловиться | Ризик | Рішення |

\| --- | --- | --- | --- |

\| Помилка в реалізації tool / неправильна бізнес-логіка до генерації відповіді | LLM-метрики можуть оцінити відповідь як internally faithful, хоча сам tool уже повернув неправильне значення | Високий: неправильний тариф або сума може напряму вплинути на гроші клієнта | Прийнято частково: критичні поля закривати deterministic domain checks; tool-level unit/integration tests додати до наступного етапу |

\| Неправильний retrieval/source conflict, якщо помилка не проявилася в перевірюваному полі | Domain check бачить лише очікуваний вихід, а faithfulness може бути високою відносно отриманого контексту | Високий: агент може впевнено використати застарілий або неправильний документ | Відкладено до окремого retrieval evaluation: перевірка retrieved document/chunk і source priority |

\| Контекстна/пам'яттєва помилка без спеціального тесту | Faithfulness та relevancy не гарантують, що агент використав попередню репліку користувача | Середній/високий: може призвести до неправильного розрахунку або повторного запитування даних | Відкладено до L03 / окремого набору multi-turn cases |

\| Тон, емпатія або якість next-step без фактичної помилки | Поточні метрики орієнтовані на factuality/relevance/domain correctness | Середній: погіршує customer experience, але не обов'язково створює фінансову помилку | Прийнято як non-blocking; додати окрему rubric/metric для tone & acknowledgement |

\| Out-of-scope технічні проблеми, наприклад freeze мобільного застосунку | Це не Search/Generation/Action проблема chatbot і поточний evaluation harness її не вимірює | Низький для цього quality bar, але може бути критичним для продукту загалом | Відкладено до окремого product/e2e monitoring |



Зелений дашборд означає лише, що пройшли перевірки з цього набору; він не доводить відсутність retrieval, tool, memory, tone або product-level дефектів.



**---**



**# 5. Розклад прогонів**

| Частота | Що входить | Критерій поділу | Ціна |
| --- | --- | --- | --- |
| Кожен merge (блокує) | C-01, C-03–C-12, C-19; тільки deterministic Domain correctness | У blocking gate потрапляють дешеві, детерміновані перевірки критичних бізнес-правил: суми, тарифи, строки dispute, eligibility та заборонені умови. | **$1.02 за повний виміряний blocking-прогін** як фактична console-вартість; LLM-judge не є критерієм merge PASS/FAIL. |
| Nightly | Усі 13 кейсів; Domain correctness + Faithfulness + Answer Relevancy + Hallucination rate для кейсів, де вона задана | Повний LLM evaluation запускається регулярно, але не блокує кожен merge через вищу вартість і стохастичність судді. | Орієнтовно близько $0.19 за один профільний прогін за вимірюванням L02. |
| Перед релізом | Усі 13 кейсів; clean ×2 і lesson-02 ×3; усі доступні метрики | Повторний повний прогін потрібен для перевірки стабільності, baseline та дефектного профілю перед випуском. | Повний виміряний прогін — **$1.02 за Anthropic Claude Platform Console**; token-based оцінка — ≈ $0.954. |

**Критерій поділу:** merge = детерміновано + критичний бізнес-ризик; nightly = повніше LLM-оцінювання; pre-release = максимальне покриття та повторюваність перед релізом.

**# 6. Локалізація причини за 15 хвилин**

**Червоний кейс:** C-05  
**Дефекти:** D25 + D20  
**Шар для локалізації D25:** Generation

### Крок 1 — відтворити

Запустити C-05 на `clean` і `lesson-02` та порівняти trace, tool result і фінальну відповідь.

Очікуваний результат для `clean`: фінальна сума відповідає authoritative calculation tool.

Для `lesson-02`: deterministic domain check падає, оскільки фінальна сума у відповіді не збігається з очікуваним `final_amount`.

### Крок 2 — знайти розбіжність

У tool result authoritative `final_amount` становить приблизно **6463.04**.

У відповіді `lesson-02` агент замінює цей результат на окремо округлене значення — до сотень.

Отже, одна з проблем спостерігається безпосередньо на межі **tool result → generated answer**.

### Крок 3 — перевірити prompt / overlay

У `lesson-02` активний D25 містить правило, яке вимагає:

- завжди округлювати final amount вгору до найближчих 100;
- не перераховувати його з компонентів;
- не узгоджувати його з компонентами.

Це суперечить вимозі використовувати authoritative calculation result як фінальне значення.

Водночас для C-05 діє ще **D20**, який змінює FX spread залежно від сусіднього tier. Тому усунення D25 не усуває всі domain-проблеми цього кейсу.

### Крок 4 — локалізувати root cause

**Root cause D25 — prompt-level defect у Generation:** правило примусового округлення змінює `final_amount`, отриманий від authoritative calculation tool.

Окремо **D20 залишається domain-level defect** у tool logic. Тому C-05 не можна використовувати як доказ того, що після виправлення D25 весь `domain` перейде з `FAIL` у `PASS`.

### Крок 5 — сформулювати тестовану гіпотезу

**Гіпотеза:** якщо прибрати правило D25 про примусове округлення та заборону узгодження з authoritative result, а натомість вимагати, щоб фінальна сума відповіді дорівнювала `final_amount` з `quote_fx`, то саме D25-помилка у C-05 буде усунена: **фінальна сума у відповіді зрівняється з `final_amount` з `quote_fx`**. Однак `domain` може залишитися `FAIL`, оскільки на C-05 додатково діє D20.

**Переформульована вимога:**

> When presenting a fee or conversion breakdown, the final amount in the response must equal the exact `final_amount` returned by the authoritative `quote_fx` result, within the test tolerance; the agent must not replace it with a separately rounded value.

**Спостережуваний вихід:** числовий `final_amount` у відповіді та відповідний `final_amount` у результаті `quote_fx`.

**Критерій:** `abs(response.final_amount - quote_fx.final_amount) <= tolerance`.

**Очікуване зрушення:** після усунення D25 фінальна сума перестане бути окремо округленою і відповідатиме `quote_fx.final_amount`; це локалізує та усуває D25, але **не гарантує `domain PASS` для C-05 через залишковий D20**.

**# 7. Вартість повного прогону**



Повний виміряний прогін L02 включає **\*\*125 викликів основної моделі агента\*\*** та **\*\*465 викликів LLM-судді\*\***, тобто **\*\*590 викликів моделі загалом\*\***.



\| Показник | Значення |

\| --- | ---: |

\| Виклики агента | 125 |

\| Виклики LLM-судді | 465 |

\| Усього викликів моделі | **\*\*590\*\*** |

\| Input tokens агента | 305,206 |

\| Output tokens агента | 14,809 |

\| Середній input на виклик агента | **\*\*≈ 2,442 токени\*\*** |

\| Середній output на виклик агента | **\*\*≈ 118 токенів\*\*** |

\| Середній обсяг одного agent call | **\*\*≈ 2,560 токенів\*\*** |

\| Прайс агента, input | $1.00 / 1M tokens |

\| Прайс агента, output | $5.00 / 1M tokens |

\| Вартість агента | **\*\*≈ $0.379\*\*** |

\| Вартість LLM-судді | **\*\*≈ $0.575\*\*** |

\| **Повна вартість прогону (token-based)** | **≈ $0.954** |
| **Фактична вартість за Anthropic Claude Platform Console** | **$1.02** |



**### Формула**



Вартість викликів агента:



\\[

C\_{agent} =

rac{305206     imes \\$1.00 + 14809     imes \\$5.00}{1\\,000\\,000}

pprox \\$0.379

\\]



Додаємо фактичну вартість викликів судді:



\\[

C\_{total} = C\_{agent} + C\_{judge}

\= \\$0.379 + \\$0.575

pprox \mathbf{\\$0.954}

\\]



Отже, token-based лабораторний розрахунок одного повного L02-прогону становить **≈ $0.954**. Фактична вартість цього самого прогону за **Anthropic Claude Platform Console — $1.02**. Різниця становить **$0.066**, або приблизно **6.9%**. Основний драйвер вартості — додаткові **465 викликів LLM-судді**.



Для планування CI як reference використовується фактична console-вартість **$1.02 за один повний прогін**. Токенний розрахунок $0.954 залишається лабораторною оцінкою.
