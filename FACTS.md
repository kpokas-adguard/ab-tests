# Справочник окружения

Факты, которые не меняются от теста к тесту. **Смотри сюда прежде, чем выяснять
что-то через API** — это экономит десятки вызовов.

Если факт здесь есть, доверяй ему. Если нашёл расхождение с реальностью —
допиши правду и пометь дату.

**Что здесь НЕ хранить:** факты о конкретных тестах (имя, слот, baseline,
конфликты) — для них `tests/<ключ>.md`. Состояние слотов на дату — оно
устаревает за день, смотреть `ab-plausible slots`. Здесь только то, что
не меняется от теста к тесту.

Перед любым `jira_create_issue`: если нужный проект, тип, компонент и ID
здесь есть — **не вызывай** `Get Tool Schema` и `jira_get_create_meta`.
Нет — вызови один раз и допиши сюда.

---

## Jira: проекты

| Что | Проект |
|---|---|
| Дизайн-задача | `AG` |
| Разработческая задача (клиент) | `AG` |
| Задача на выпиливание | `AG` |
| Бэк: регистрация эксперимента | `ADG` |
| Бэк: закрытие теста | `ADG` |

## Jira: обязательные поля при создании

| Проект | Обязательно |
|---|---|
| `AG` | `components`, `issuetype`, `project`, `summary` |
| `ADG` | то же + **`due date`** |

Проверено 2026-09-10 через `jira_get_create_meta`.

## Jira: компоненты

| Платформа | Компонент |
|---|---|
| Mac Mini | `Mac: AdGuard Mini` (новые задачи), `Sciter: Safari App` (старые) |
| Android | `Android: AdGuard` |
| Extensions | `Extensions: AdGuard`, `Extensions: VPN` |
| Windows VPN | `Windows: VPN` |
| macOS VPN | `Mac: VPN` |
| UI-слой десктопов (Sciter) | `Sciter: VPN` |
| Дизайн | `Design` |
| Бэкенд | `BackendJava` |

## Jira: ID компонентов и пользователей

Jira принимает компонент только как `{"id": "<число>"}`, ассайни — по
username. ID не меняются — записать один раз, дальше не искать.

| Компонент | ID |
| --------- | -- |
| Product: AdBlocker | 13090 |
| Mac: AdGuard Mini | 12690 |
| Android: AdGuard | 11198 |
| Extensions: AdGuard | 11299 |
| BackendJava | _в проекте ADG, ID не проверен_ |
| Design | 11294 |

| Пользователь | username | Роль |
| ------------ | -------- | ---- |
| _PM_ | `config.local.yml → pm.jira_username` | PM |
| Anna Khimchenko | _не проверено_ | дизайн |
| Alexey Gorbatko | _не проверено_ | бэк |

**Компонент продуктовых задач — `Product: AdBlocker`**, всегда.

## Jira: статусы

| Статус | Смысл |
|---|---|
| `Сделать` | To Do (открыта, не в работе) |
| `Готово` | Done |
| `Code Review` | в ревью |
| `Открытый` | Open |

Задача закрытия теста на бэке: заголовок `Close Test AG-XXXXX in the service`
или `Close A/B Test AG-XXXXX_...`.

## VPN: единственное исключение

VPN-тесты не попадают в трекер, дашборд и статистику. Но сервис A/B один
на все продукты, поэтому при проверке уникальности `experiment_name`
смотреть **все** бэк-задачи ADG, включая VPN. См. `NAMING.md`.

## Plausible: VPN-продукты

`site_ids` в `config.yml` покрывают AdGuard-продукты (`adg.android.app`,
`adg.extension.app` и т.д.). Данные тестов VPN-продуктов (Extensions: VPN,
Windows/macOS VPN) в этих сайтах не находятся — у VPN, видимо, свои site_id.
Проверено 2026-09-10 на AG-54840 и AG-55378: за 12 месяцев ни одного визита
в известных сайтах, при том что задачи выкачены.

## Jira: значения по умолчанию

| Поле | Значение |
|---|---|
| Assignee | `config.yml → pm.jira_username` |
| Метка | `Split-Testing` во всех задачах теста |
| Метка cleanup-задачи | дополнительно `cleanup` |
| Приоритет в `ADG` | `P2: High` |
| Due date в `ADG` | ~7 дней от создания (бэк блокирует разработку) |
| Ревьюер в дизайн-задаче | `vozersky` |
| Fix Version, спринт | **не проставлять** — ставится руками |

## Типы задач

| Задача | Тип |
|---|---|
| Дизайн | `Design` |
| Остальные | `Задача` |

## Plausible

Базовый адрес и site_id по платформам — в `config.yml`, секция `plausible`.
Токен — в `~/.plausible-key`.

Сборки Python с python.org не видят системные сертификаты: `bin/ab-plausible`
берёт корни из `certifi`. Если появится ошибка TLS — причина, скорее всего, в этом.

**Периоды (`period`):** принимаются `30d`, `6mo`, `12mo` и т.п.; **`90d` не
поддерживается** — ошибка `invalid period`, использовать `6mo`. Проверено 2026-09-11.

## Jira: связи задач (issuelinks)

Поле `issuelinks` **не принимается API** — ни при создании
(`jira_create_issue`), ни при редактировании (`jira_edit_issue`):
`Field does not support update 'issuelinks'`. Связь `Relates to` ставится
руками. Проверено 2026-09-11 (AG-58895 → AG-58893).

---

## Схемы MCP-инструментов — не запрашивать повторно

`Get Tool Schema` нужен один раз на инструмент. Схемы не меняются.
Инструмент есть в таблице — вызывай сразу с этими аргументами.
Нет — запроси схему один раз и допиши строку.

| Инструмент | Сервер | Аргументы |
| ---------- | ------ | --------- |
| jira_get_issue | aiguard | `issueIdOrKey` |
| jira_get_issue_comments | aiguard | `issueIdOrKey`, `maxResults` |
| jira_search_issues | aiguard | `jql`, `maxResults`, `startAt` |
| jira_create_issue | aiguard | _заполнить: components по id, assignee username, duedate, priority, labels_ |
| jira_edit_issue | aiguard | _заполнить_ |
| jira_get_create_meta | aiguard | `projectKeyOrId`, `issuetypeName` |
| notion-fetch | notion | `id` |
| notion-search | notion | `query`, `page_size` |
| notion-create-pages | notion | _заполнить_ |
| notion-update-page | notion | _заполнить_ |

## Notion: Research Board

**ID базы:** `25daa56b-7730-8073-8ffd-d206f71275b0`

Схема ниже — кэш. **Не загружай схему заново**, если раздел заполнен.
Если запись упала из-за поля, которого здесь нет, — перечитай один раз
и обнови.

⚠️ Раздел был потерян при одновременной записи 2026-09-11 — точную схему
из fetch нужно записать заново: data source id, полные списки значений
select/multi_select, типы полей, ID базы релизов, user id.

| Свойство | Тип | Значения / формат |
| -------- | --- | ----------------- |
| Name | title | `<ключ продуктовой задачи>. <Название>` |
| Type | multi_select | `Preference test`, `10-sec test`, `Survey`, `A/B`, `Research`, `Usability test`, `CustDev`, `Analytics` (проверено 2026-09-16, скрин) |
| Research Area | multi_select | `Activation`, `Revenue`, _остальные — заполнить_ |
| Product | multi_select | `AdGuard`, _остальные — заполнить_ |
| Platform | multi_select | `Mini`, `Mac`, `Android`, `Windows`, `iOS`, `Extension` — _уточнить_ |
| Jira | url | подписанные ссылки |
| Notion release | url | _заполнить_ |
| 📅 Release | relation | ID базы релизов — _заполнить_ |
| Hypothesis | select | `No data yet`, _остальные — заполнить_ |
| Assignee, Stakeholder | people | `config.yml → pm.notion_user_id` |
| Status | status | `Incoming`, `Prepare`, _остальные — заполнить_ |
| Start Date, End Date | date | |
| Purpose | select | `Generative`, `Confirmatory` |
| Google Document | url | |

### Значения при создании A/B-карточки

Type `A/B` · Product `AdGuard` · Assignee и Stakeholder — PM из `config.yml` ·
Status `Prepare` · Start Date сегодня · Hypothesis `No data yet` ·
Purpose `Confirmatory` · Platform и Research Area — по тесту.
Не заполнять: Notion release, Release, End Date, Google Document.

### Синхронизированные блоки

Шаблон «АБ тестирование» содержит synced-блок — через API не применять.
Карточка создаётся обычными блоками по `templates/research-card.md`.

## Прод vs задачи (на 2026-09-11, ab-plausible slots)

- **extension experiment_1 — ГРЯЗНЫЙ**: два эксперимента одновременно —
  AG-51010-limitations-browser (закрыт 28.04, не выпилен из кода, cleanup-задачи
  нет) и AG-52740-rule-limits (AG-54586, идёт). Оба результата под вопросом.
- **extension experiment_2** — занят AG-52622-general-settings-promo (закрыт,
  хвост старых версий). experiment_3 свободен.
- **android experiment_1** — AG-53163-paywall. Имя в проде ≠ бэк-имя
  `AG-53163-paywall-no-slider` (ADG-12065) — где-то живёт сборка со старым именем.
  Плюс 2 визита `AG-57632-home-basic-level-b` (dev/QA-сборка AG-57632, план —
  experiment_3, имя в бэке AG-57632-home-screen-protection-level).
- **android experiment_2** — AG-51013-protection-screen (совпадает с бэком)
  + 2 визита старой сборки `AG-51013-protection`. experiment_3 свободен.
- **macmini experiment_1** — AG-51019-advanced-settings (закрыт, хвост);
  experiment_2 и experiment_3 свободны.
- Закрытые тесты, видимые в проде (известное ограничение finished): AG-51010,
  AG-52622, AG-51019.

## VPN: что известно

Компоненты Jira: `Windows: VPN`, `Mac: VPN`, `Extensions: VPN`, `Sciter: VPN`.
Проект — тот же `AG` для клиента, `ADG` для бэка.

Неизвестно, добывает агент Setup при первом VPN-пользователе:
- компонент продуктовых задач (аналог `Product: AdBlocker`)
- значение `Product` в Research Board
- площадки Plausible (`site_id`) — у VPN свои, в `adg.*` их нет
  (проверено 2026-09-10: AG-54840 и AG-55378 за год без визитов в adg.*)

Всё это записывается в `config.yml` → `product_map.vpn` и `plausible.site_ids`.

## Журнал изменений

Дописывай сюда, когда факт добавлен или исправлен.

- 2026-09-10 — файл заведён по итогам первого прогона `/ab-tasks`
- 2026-09-10 — дополнено после `/ab-sync`: компоненты всех платформ,
  смысл статусов, про закрытие на бэке и site_id VPN-продуктов
- 2026-09-11 — добавлены ID компонентов и пользователей, правило
  «сначала сюда, потом API»; объединён с JIRA-REFERENCE.md
- 2026-09-11 — раздел Research Board: ID базы, схема свойств, значения
  при создании; правило «не загружать схему, если раздел заполнен»
- 2026-09-11 — Research Board: точная схема из fetch (data source id,
  полные списки select/multi_select, Type/Product/Platform/Research Area —
  multi_select, Purpose — Generative/Confirmatory, Jira — url, Notion
  release — url, 📅 Release — relation + ID базы релизов, user id
  PM); заполнены ID компонентов AG
- 2026-09-11 — Plausible: `90d` не поддерживается (использовать 6mo);
  Jira: `issuelinks` не принимается API, связь руками; состояние слотов
  macmini на дату
- 2026-09-11 — `/ab-sync`: прод vs задачи по слотам (грязный extension
  experiment_1: AG-51010+AG-52740; имя AG-53163-paywall ≠ бэк-имени;
  AG-57632-home-basic-level в проде); backend_finished_at для AG-51010
  (28.04), AG-51019 (23.06), AG-52622 (14.07)
- 2026-09-11 — восстановлены разделы «Схемы MCP-инструментов» и «Notion:
  Research Board» после потери при одновременной записи; схему Research
  Board из fetch нужно записать повторно
