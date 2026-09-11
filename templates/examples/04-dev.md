Проект: AG · Тип: Задача · Компоненты: Android: AdGuard
Исправить в версиях: Android: AdGuard v4.15 · Метки: Split-Testing
Assignee: Kseniia Pokas

ЗАГОЛОВОК
Implement new screen of main screen for A/B test — «Basic level on main screen»

── ОПИСАНИЕ ──────────────────────────────────────────────

## Проблема

Экран Main (home) как ключевой экран и точка входа недостаточно ориентирован
на продажу. Не отвечает на вопросы «в каком состоянии я сейчас» и «что я могу
сделать, чтобы его улучшить». Приложение не ведёт пользователя к нужным
для бизнеса шагам.

## Параметры эксперимента ⚠️ ЗАПОЛНИТЬ ДО ПОСТАНОВКИ

  experiment_name  AG-57632-home-screen-protection-level   ← как в ADG-12912
  Слот             experiment_3
  Контроль         AG-57632-home-screen-protection-level-a_def
  Вариант B        AG-57632-home-screen-protection-level-b

version_name передаётся в Plausible как есть, без преобразований.
Неизвестный version_name → локальный дефолт, значение не закрепляется,
experiment_* не отправляется.

## ToDo dev

1. Отрисовать 1 новый вариант экрана Main (home):
   — добавлен текст под заголовком «You're using basic protection.
     Upgrade to 100%». При клике открывается paywall. [Макет]
   — дефолтный вариант остаётся без изменений, это A/B-тест.
2. Связать с A/B-системой: поддержать experiment_name и оба version_name.
3. Замапить эксперимент на слот experiment_3.
4. Телеметрия: событие на клик по тексту с pageview = home_screen.
5. Передавать experiment_* во всех релевантных событиях после assignment.

## Телеметрия для AG-57632-home-screen-protection-level

Часть этой задачи, входит в Definition of Done.
  - событие: клик по тексту «You're using basic protection. Upgrade to 100%»,
    pageview = home_screen
  - experiment_3 = version_name во всех событиях после assignment
  - проверить в Plausible adg.android.app, что события приходят

## Ссылка на PR

PR #242 — AG-57632 / A/B test: basic protection text on home screen
Появляется в implemented by при открытии PR. Без неё — не в Code Review.

## Definition of Done

- ПР аппрувнули QA
- ПР помержен
- experiment_name в коде совпадает с именем в сервисе A/B-тестирования
- Слот совпадает с указанным в карточке
- experiment_* приходит в Plausible с правильным version_name
- При ошибке /session_start — локальный дефолт, experiment_* не отправляется

── СВЯЗИ ─────────────────────────────────────────────────
Is blocked by: ADG-12912 (бэк: регистрация)
Is blocked by: AG-50080 (дизайн)
Relates to:    AG-57633 (выпиливание)
