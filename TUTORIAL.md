# Открытка для Ярослава → GitHub Pages

В папке:
- `index.html` — вся открытка в одном файле (HTML + CSS + JS, шрифты грузятся с Google Fonts).
- `TUTORIAL.md` — этот файл.

Итог: ссылка вида `https://<твой-ник>.github.io/yaroslav/`, бесплатно и навсегда.

---

## Что нужно заранее

1. Аккаунт на github.com.
2. Установленные `git` и `gh` (GitHub CLI): https://cli.github.com
3. Один раз залогиниться: `gh auth login` → GitHub.com → HTTPS → Login with a web browser.
4. Claude Code, запущенный **в этой папке** (`cd путь/к/yaroslav-card` → `claude`).

---

## Вариант А — отдать всё Claude Code

Открой Claude Code в папке с `index.html` и вставь этот промпт целиком:

```
Задеплой эту папку на GitHub Pages.

1. Проверь, что я залогинен: `gh auth status`. Если нет — остановись и скажи мне выполнить `gh auth login`.
2. Если здесь ещё нет git-репозитория: `git init -b main`, добавь index.html (TUTORIAL.md тоже можно), сделай коммит "Открытка для Ярослава".
3. Создай ПУБЛИЧНЫЙ репозиторий `yaroslav` и запушь:
   `gh repo create yaroslav --public --source=. --remote=origin --push`
   Если имя занято — предложи другое и спроси меня.
4. Включи GitHub Pages из ветки main, корень папки:
   `gh api -X POST repos/{owner}/yaroslav/pages -f "source[branch]=main" -f "source[path]=/"`
   (owner возьми из `gh api user --jq .login`). Если Pages уже включены — пропусти.
5. Подожди, пока сайт соберётся: каждые 15 секунд проверяй
   `gh api repos/{owner}/yaroslav/pages --jq .status` до значения `built` (максимум ~3 минуты).
6. Проверь, что страница отвечает: `curl -sI https://{owner}.github.io/yaroslav/ | head -1` — должно быть 200.
7. Дай мне итоговую ссылку.

Не меняй содержимое index.html.
```

Через пару минут Claude Code выдаст ссылку — её и отправляешь Ярославу.

---

## Вариант Б — руками (5 команд)

```bash
cd путь/к/yaroslav-card
git init -b main
git add index.html
git commit -m "Открытка для Ярослава"
gh repo create yaroslav --public --source=. --remote=origin --push
```

Дальше в браузере: репозиторий `yaroslav` → **Settings** → **Pages** →
Source: **Deploy from a branch** → Branch: **main**, папка **/ (root)** → **Save**.

Через 1–2 минуты сверху на той же странице появится ссылка
`https://<ник>.github.io/yaroslav/`.

Без `gh`: создай пустой репозиторий `yaroslav` на сайте, затем
```bash
git remote add origin https://github.com/<ник>/yaroslav.git
git push -u origin main
```
и включи Pages так же через Settings.

---

## Если что-то поменял в открытке

```bash
git add index.html
git commit -m "Правки"
git push
```
Сайт обновится сам за минуту (иногда надо обновить страницу с Ctrl+F5 — кэш).

---

## Перед отправкой проверь

- Открой ссылку **с телефона через мобильный интернет**, без VPN — так её увидит Ярослав.
- Прокрути вниз: слова должны догружаться, счётчик внизу — расти.
- Отправь ссылку себе в Telegram — посмотри, как выглядит превью.

## Частые проблемы

| Симптом | Что делать |
|---|---|
| 404 сразу после включения | Подожди 2–3 минуты, Pages ещё собирается |
| 404 спустя время | Файл должен называться ровно `index.html` и лежать в корне репо |
| Pages нельзя включить | Репозиторий должен быть **public** (для бесплатного аккаунта) |
| Шрифты «обычные» | Google Fonts не загрузились — проверь интернет; страница всё равно работает |
| Хочется свой домен | Settings → Pages → Custom domain + CNAME-запись у регистратора домена |
