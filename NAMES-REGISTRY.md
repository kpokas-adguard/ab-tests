# Библиотека имён тестов — все продукты

Все `experiment_name`, когда-либо зарегистрированные в сервисе A/B.
**Включая VPN** — сервис один на все продукты, и это единственное место,
где VPN-тесты видны рядом с блокировщиком.

Источник — две бэк-задачи в ADG на каждый тест:

| Задача | Заголовок | Что значит статус |
| ------ | --------- | ----------------- |
| регистрация | `Add information about Test AG-XXXXX to the service` | закрыта → тест зарегистрирован, идёт или шёл |
| закрытие | `Close Test AG-XXXXX in the service` / `Close A/B Test AG-XXXXX_…` | закрыта → тест остановлен на бэке |

Состояние теста на бэке выводится из двух статусов:

| Регистрация | Закрытие | Состояние |
| ----------- | -------- | --------- |
| открыта | — | `не зарегистрирован` |
| закрыта | нет задачи | `идёт` |
| закрыта | открыта | `закрывается` |
| закрыта | закрыта | `закрыт` |

Обновляется командой `/ab-sync`. Для чего нужен:

- **проверка уникальности** нового имени — по `NAMING.md`
- **что реально идёт на бэке** — независимо от того, что записано в трекере
- **какие тесты закрыты, а какие висят** — по всем продуктам сразу

## Реестр

<!-- ab-sync пишет строки ниже; формат не менять — его читает дашборд -->

| experiment_name | Продукт | Платформа | Регистрация | Закрытие | Состояние |
| --------------- | ------- | --------- | ----------- | -------- | --------- |
| AG-51010-Limitations-browser | AdGuard | Extension | ADG-11722 · закрыта | ADG-12170 · закрыта | закрыт |
| AG-51013-protection-screen | AdGuard | Android | ADG-12828 · закрыта | — | идёт |
| AG-51019-Advanced-settings | AdGuard | macOS | ADG-11711 · закрыта | ADG-12473 · закрыта | закрыт |
| AG-52622-general-settings-promo | AdGuard | Extension | ADG-11932 · закрыта | ADG-12617 · закрыта | закрыт |
| AG-52740-rule-limits | AdGuard | Extension | ADG-12312 · закрыта | — | идёт |
| AG-53163-paywall-no-slider | AdGuard | Android | ADG-12065 · закрыта | — | идёт |
| AG-57632-home-screen-protection-level | AdGuard | Android | ADG-12912 · закрыта | — | идёт |
| AG-58835-onboarding-paid-features | AdGuard | macOS | ADG-13030 · открыта | — | не зарегистрирован |
| AG-49792_data_limit_screens | VPN | Extension | ADG-11714 · закрыта | ADG-12742 · закрыта | закрыт |
| AG-50274_paywall_screens | VPN | Windows | ADG-11709 · закрыта | ADG-12576 · закрыта | закрыт |
| AG-51219_onboarding | VPN | iOS | ADG-11727 · закрыта | — | идёт |
| AG-51223-paywall-screens | VPN | macOS | ADG-11873 · закрыта | ADG-12579 · закрыта | закрыт |
| AG-51229_onboarding | VPN | Android | ADG-11730 · закрыта | — | идёт |
| AG-52105-onboarding | VPN | Windows | ADG-12995 · закрыта | — | идёт |
| AG-54372_onboarding | VPN | iOS | ADG-12263 · закрыта | — | идёт |
| AG-54840-onboarding-login-order | VPN | macOS | ADG-12418 · закрыта | — | идёт |
| AG-55378-personalized-onboarding-paywall | VPN | Extension | ADG-12447 · закрыта | — | идёт |
| AG-56889_personalized_onboarding | VPN | Windows | ADG-12713 · закрыта | — | идёт |
| AG-56890_personalized_onboarding | VPN | macOS | ADG-12713 · закрыта | — | идёт |
| AG-58106-purchase-screen | VPN | iOS | ADG-12907 · закрыта | — | идёт |
