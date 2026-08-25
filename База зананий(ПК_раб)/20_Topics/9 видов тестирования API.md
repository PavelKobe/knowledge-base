# 9 видов тестирования API

Фраза «у нас есть тесты» не описывает стратегию. Полноценное покрытие API включает три семейства: **correctness**, **performance** и **safety**.

> [!tip] Интерактивный аудит покрытия
> [Открыть дашборд «9 видов тестирования API»](api-testing-dashboard/index.html)
>
> Отмечайте внедрённые типы тестов: дашборд сохранит состояние и покажет слепые зоны по каждому семейству.

## Correctness — делает ли API правильную вещь

1. **Smoke:** критические endpoints отвечают после deploy.
2. **Functional:** бизнес-логика возвращает правильные значения и состояния.
3. **Contract:** schema соответствует ожиданиям consumers.
4. **Integration:** границы сервисов, БД, очередей и внешних API работают вместе.
5. **Regression:** ранее исправленное поведение не сломалось в новой сборке.

## Performance — выдерживает ли API нагрузку

6. **Load:** система держит целевой steady-state traffic и latency SLO.
7. **Stress:** известна точка насыщения и способ деградации за пределом capacity.

## Safety — можно ли API злоупотребить

8. **Security:** authn, authz, object-level access и другие угрозы проверяются явно.
9. **Fuzz:** malformed, oversized, unexpected и граничные входы не приводят к 500 и утечкам.

## Минимальная стратегия

- На каждый deploy: smoke и критические functional/contract checks.
- На каждый PR: unit/functional, contract и ограниченный fuzz.
- Регулярно: integration и regression suite.
- Перед крупным релизом: load, stress и security testing.
- В production: synthetic smoke, SLO и алерты — это дополнение, не замена pre-release testing.

## Связи

- [[REST API]]
- [[Pytest]]
- [[12 концепций проектирования API]]
- [[Ruff Mypy pre-commit]]

## Источник

- [AlgoInsight, «9 types of API testing»](https://www.instagram.com/reel/Db8rJChAh1y/), 12 августа 2026.

