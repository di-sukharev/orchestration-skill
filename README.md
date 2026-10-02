# Orchestration

Обычно дорогая модель сама читает и пишет весь код задачи. Orchestration поручает чтение и написание кода дешёвой модели. Дорогая модель только планирует задачу и принимает работу. Перед коммитом и пушем код проверяет [Loop Code Review](https://github.com/di-sukharev/loop-code-review-skill).

## Установка

Отправьте агенту это сообщение.

```text
Установи скиллы глобально
https://github.com/di-sukharev/orchestration-skill
https://github.com/di-sukharev/loop-code-review-skill
```

## Запуск

Напишите `/orchestration <задача>`. В Codex напишите `$orchestration <задача>`. Если пуш не нужен, допишите «без пуша».

## Другие скиллы

- [Code Scout](https://github.com/di-sukharev/code-scout-skill) поручает поиск кода дешёвой модели.
- [Loop Tasks](https://github.com/di-sukharev/loop-tasks-skill) запускает для каждой задачи нового агента с чистым контекстом.
- [Loop Code Review](https://github.com/di-sukharev/loop-code-review-skill) отдаёт код новому ревьюеру без истории чата.
- [Refactoring](https://github.com/di-sukharev/refactoring-skill) меняет код, только если следующая задача станет проще.

[Инструкция для агента](orchestration/SKILL.md) · [Лицензия MIT](LICENSE)
