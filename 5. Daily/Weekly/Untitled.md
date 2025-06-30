---
tags: [weekly, review]
---

# Неделя 27-W

## 📅 Даты
- [[2025-06-24]]
- [[2025-06-25]]
- [[2025-06-26]]
- [[2025-06-27]]
- [[2025-06-28]]
- [[2025-06-29]]
- [[2025-06-30]]

---

## ⚖️ Вес по дням

```dataview
table file.link as "День", regexmatch("^## Вес: (.+)", text)[0] as "Вес"
from "5. Daily/2025"
where date(file.name) >= date(today) - dur(7 days)
sort file.name asc
```

---

## ✅ Привычки по дням

```dataview
table file.link as "День", length(rows.where(r => contains(r.text, "- [x]"))) as "Выполнено привычек"
from "5. Daily/2025"
where date(file.name) >= date(today) - dur(7 days)
sort file.name asc
```

---

## ✍️ Итоги недели

- Главное достижение:
- Что не получилось:
- Что улучшить на следующей неделе:
