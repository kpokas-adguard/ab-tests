---
# ═══ Шаг 0 — Входящая. Дальше не смотри, это осознанно. ═══
id: __ID__
title:
platform: __PLATFORM__
product:              # adblocker | vpn — определяется по платформе, можно не заполнять
created: __CREATED__
stage: incoming
author:
research_card:         # ссылка на карточку в Research Board
prior_art:             # что уже пробовали по теме: ссылка на history/<тема>.md + вывод в 1–2 строки

scored_by_pm:
incoming_comment:      # что сказали инициатору: берём/нет/когда

# ═══ Шаг 1 — Карточка теста ═══
problem:
problem_source:        # откуда знаем о проблеме: телеметрия / комментарий разработки / поддержка
hypothesis:            # если X, то Y, потому что Z
primary_metric:
secondary_metric:
segment:               # кого и где проверяем
extrapolation:         # на кого экстраполируем результат
no_diff_choice:        # если разницы нет — берём "дефолт" или "новый"
extra_approved:        # только если у платформы есть extra_approver в config.yml

# ═══ Шаг 2 — Дизайн-синк ═══
design_sync_at:
branches_agreed:
meeting_notes:

# ═══ Шаг 3 — Реализуемость ═══
feasibility:           # yes | no | conditional
feasibility_notes:

# ═══ Шаг 4 — Планирование. ТОЧКА НЕВОЗВРАТА ═══
current_conversion:
expected_lift:
sample_size_per_variation:
planned_duration_days:
target_release:
fix_version:           # напр. Android: AdGuard v4.15
code_freeze:           # YYYY-MM-DD
slot:                  # experiment_1 | experiment_2 | experiment_3
slot_checked:          # true когда слот свободен И чистый
analysts_reviewed_design: # аналитики глянули дизайн эксперимента свежим взглядом
stop_conditions:       # статзначимость / потери в деньгах
worst_case:            # переживём ли месяц без возможности отката

# ═══ Шаг 5 — Разработка ═══
experiment_name:       # напр. AG-57632-home-screen-protection-level
variants:
  # a_def: текущий UI (контроль)
  # b: что меняется
telemetry_events: []   # напр. клик на Upgrade to 100% с pageview = home_screen
tasks:
  design:              # AG-XXXXX
  dev:                 # AG-XXXXX
  backend:             # ADG-XXXXX — регистрация в сервисе A/B
  cleanup:             # AG-XXXXX — заводится СЕЙЧАС
  backend_close:       # ADG-XXXXX — заводится на шаге 9, когда известен победитель
design_review_done:

# ═══ Шаг 6 — QA ═══
qa_scenarios_sent:
qa_passed:

# ═══ Шаг 7 — Запуск ═══
release_version:
started_at:
planned_stop:
research_team_thread:
analysts_deadline_in_jira: # аналитики занесли сроки обсчёта в Jira

# ═══ Шаг 8 — Анализ ═══
stopped_at:
analyst_reviewed:
results_reviewed_by_analysts: # отправили карточку с результатами аналитикам
contamination_checked:        # проверен хвост предыдущего теста на этом слоте
conclusion:                   # победитель | нет эффекта | вред
confidence:
side_effects:

# ═══ Шаг 9 — Выкат/откат ═══
decision:              # ship | rollback | inconclusive
decision_at:
cleanup_release:
hypothesis_predicted:  # предсказали или нет — проперти Hypothesis в карточке

# ═══ Шаг 10 — Закрытие хвоста. Слот грязный, пока это не закрыто ═══
backend_close_task:       # ADG-XXXXX, закрытие теста в A/B-сервисе
backend_finished_at:
code_removed_version:
code_removed_released_at:
communicated_at:
---

# __ID__

## Проблема


**Откуда знаем:**

## Гипотеза


## Ветки

| Вариант | version_name | Что видит пользователь |
|---|---|---|
| control | `<name>-a_def` | текущий UI, без изменений |
| B | `<name>-b` | |

Слот: `experiment_N`

## Метрики

- Основная:
- Дополнительная:
- Текущая конверсия / ожидаемое изменение / выборка:

## Результаты

_Шаг 8._

## Решение и выводы

_Шаги 9-10._
