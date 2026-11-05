# Run evidence

- Tool: Cursor Agent
- Command: `npx playwright test tests/add-car.spec.ts --project=chromium`
- Timestamp: `2026-11-05T13:11:03Z`
- Environment: `macOS 14.5 (darwin arm64), Node v26.13.0, Playwright 1.62.1 Chromium, https://qauto.forstudy.space`
- Actual: `✓ 1 [chromium] › tests/add-car.spec.ts:3:5 › guest can add Audi TT to Garage (2.4s)` / `1 passed (3.3s)`, exit code 0

| Елемент | Джерело |
|---|---|
| setup | `specs/add-car.md` (старт через `baseURL`, гостьовий вхід як підготовка); Playwright CLI snapshot стартової сторінки: `button "Guest log in"` → `getByRole('button', { name: 'Guest log in' })`; URL після входу `/panel/garage` |
| locator/action | Playwright CLI snapshot + `generate-locator`: `getByRole('button', { name: 'Add car' })`, `getByRole('dialog')`, `getByLabel('Brand')` → `Audi`, `getByLabel('Model')` → `TT`, `getByRole('spinbutton', { name: 'Mileage' })` → `12000`, `getByRole('button', { name: 'Add' })` (у тесті `exact: true` в межах dialog); дані — `specs/add-car.md` |
| assertions | `specs/add-car.md` (URL Garage + видимий `Audi TT`); CLI snapshot після збереження: `list` → `paragraph: Audi TT` → `getByRole('list').getByText('Audi TT', { exact: true })`; `toHaveURL(/panel\/garage/)` |

- FACTS:
  - Тест пройшов з першої спроби (retry не знадобився), 1 passed, exit code 0.
  - Гостьовий Garage до додавання порожній («You don’t have any cars in your garage»), після — один запис `Audi TT` (CLI snapshot).
  - Змінено/створено лише `tests/add-car.spec.ts` і цей evidence-файл; config, dependencies, baseline не змінювались.
  - Для CLI і Test за дозволом людини встановлено Chromium (`npx playwright cli install-browser chromium`).
- ASSUMPTIONS:
  - Файл `cases/qauto/requirements/add-car.md`, на який посилається spec, у репозиторії відсутній; правила взято лише з `specs/add-car.md`.
  - Кожна гостьова сесія стартує з порожнім Garage, тому `Audi TT` у списку однозначний.
- RISKS:
  - Назва `Add` частково збігається з `Add car` / `Add fuel expense` — пом'якшено `exact: true` і scope `dialog`.
  - Тест залежить від доступності публічного стенду qauto.forstudy.space.
- HUMAN CORRECTION: дозвіл на встановлення Chromium після `CLI SETUP BLOCKER`.
- DECISION: `ACCEPT` — людина погодила результат 2026-11-05.
