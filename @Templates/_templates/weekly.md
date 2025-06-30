---
tags: [weekly, review]
---

# Неделя <% tp.date.now("w-[W]") %>

## 📅 Даты
- [[<% tp.date.now("YYYY-MM-DD", -6) %>]]
- [[<% tp.date.now("YYYY-MM-DD", -5) %>]]
- [[<% tp.date.now("YYYY-MM-DD", -4) %>]]
- [[<% tp.date.now("YYYY-MM-DD", -3) %>]]
- [[<% tp.date.now("YYYY-MM-DD", -2) %>]]
- [[<% tp.date.now("YYYY-MM-DD", -1) %>]]
- [[<% tp.date.now("YYYY-MM-DD") %>]]

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
