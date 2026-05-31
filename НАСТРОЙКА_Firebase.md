# Настройка на вход с Google за Summer Planner

Кодът за вход с Google и облачно запазване вече е добавен в `summer_planner_2026.html`.
Остават **6 стъпки**, които правиш веднъж в безплатната конзола на Firebase. Отнема около 10 минути.

Важно: входът с Google работи само когато сайтът е **качен онлайн** (https). Ако отвориш файла директно от компютъра (file://), бутонът няма да работи — затова стъпка 6 е качването в GitHub Pages.

---

## 1. Създай Firebase проект
1. Влез на https://console.firebase.google.com с твоя Google акаунт.
2. Натисни **Add project** (Добави проект) → дай име, напр. `summer-planner` → Continue.
3. Google Analytics може да изключиш (не е нужен) → **Create project**.

## 2. Регистрирай уеб приложение и копирай конфигурацията
1. На началния екран на проекта натисни иконата **`</>`** (Web).
2. Дай му псевдоним, напр. `planner-web` → **Register app**.
3. Ще се покаже блок `const firebaseConfig = { ... }`. **Копирай стойностите.**
4. Отвори `summer_planner_2026.html` и намери блока (към края на файла):

   ```js
   const firebaseConfig = {
     apiKey:            "PASTE_API_KEY",
     authDomain:        "PASTE_PROJECT.firebaseapp.com",
     ...
   };
   ```
   Замени всяка стойност `PASTE_...` със съответната от Firebase. (Тези ключове са безопасни за публикуване — те идентифицират проекта, не са парола.)

## 3. Включи вход с Google
1. В лявото меню: **Build → Authentication → Get started**.
2. Раздел **Sign-in method** → избери **Google** → превключи на **Enable**.
3. Избери support email → **Save**.

## 4. Създай Firestore база данни
1. В лявото меню: **Build → Firestore Database → Create database**.
2. Избери регион (напр. `eur3` / Europe) → Next.
3. Започни в **Production mode** → Enable.

## 5. Постави правилата за сигурност
В Firestore → раздел **Rules** → замени съдържанието с това и натисни **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /planners/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

Това гарантира, че **всеки вижда и променя само своя прогрес** — никой не може да чете чужд.

## 6. Качи сайта в GitHub Pages и оторизирай домейна
1. Създай нов repository на https://github.com → **New repository** → дай име (напр. `summer-planner`) → може да е **Public** (ключовете в `firebaseConfig` са безопасни за публикуване).
2. Качи файловете: на repo-то → **Add file → Upload files** → плъзни `summer_planner_2026.html` (по желание го преименувай на `index.html` за чист линк) → **Commit changes**.
3. Включи Pages: **Settings → Pages → Branch: `main` → / (root) → Save**. След минута получаваш адрес като `https://tvoeIme.github.io/summer-planner/`.
4. Върни се в Firebase: **Authentication → Settings → Authorized domains → Add domain** → добави `tvoeIme.github.io`.
5. Готово — отвори линка (`https://tvoeIme.github.io/summer-planner/summer_planner_2026.html`, или само `.../summer-planner/` ако файлът е `index.html`), влез с Google и тествай.

---

## Как работи за учениците
- Всеки ученик отваря линка и натиска **„Вход с Google"** със своя акаунт.
- Прогресът (математика, четене, писане, английски) се пази в облака под неговия профил.
- Може да влезе от телефон, таблет или компютър — вижда същия прогрес навсякъде.
- Бутонът **„Изход"** горе вдясно сменя профила.

## Често задавани
- **Безплатно ли е?** Да. Безплатният план на Firebase покрива спокойно цял клас.
- **Стар прогрес ще се загуби ли?** Не. При първото влизане наличният прогрес от браузъра се качва в облака.
- **Искам и другите планери (четене, английски, математика като отделни файлове) с вход.** Кажи ми — същата логика се добавя и в тях за минути.
