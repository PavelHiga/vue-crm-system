# Vue CRM System

Современная CRM-система для управления пользователями и задачами, разработанная на Vue 3 с использованием TypeScript и современного стека технологий.

## 🚀 Описание проекта

Полнофункциональная CRM-система с расширенными возможностями управления пользователями, задачами и правами доступа. Проект демонстрирует навыки работы с современными фронтенд-технологиями, архитектурными паттернами и лучшими практиками разработки.

## ✨ Ключевые возможности

### Аутентификация и авторизация
- ✅ Регистрация и вход в систему
- ✅ JWT токены (Access + Refresh)
- ✅ Автоматическое обновление токенов
- ✅ Защищённые маршруты
- ✅ Восстановление пароля
- ✅ Сохранение состояния сессии

### Управление пользователями
- 👥 Просмотр всех пользователей системы
- 🔍 Поиск по имени и email
- 🎯 Фильтрация (все/заблокированные/активные)
- 📊 Пагинация с server-side обработкой
- 🔐 Управление правами (назначение/снятие админ-прав)
- 🚫 Блокировка/разблокировка пользователей
- 🗑️ Удаление пользователей
- 📝 Просмотр и редактирование профилей

### Управление задачами
- ✏️ Создание, редактирование и удаление задач
- ✅ Отметка выполнения/невыполнения
- 🎨 Фильтрация задач (все/в работе/завершённые)
- 📈 Статистика по задачам
- ⚡ Real-time обновление списка

### Профиль пользователя
- 👤 Просмотр и редактирование личных данных
- 📧 Обновление email, имени, телефона
- 🔑 Смена пароля

## 🛠 Технологический стек

### Frontend
- **Vue 3** - прогрессивный JavaScript-фреймворк
- **TypeScript** - строгая типизация
- **Vuetify 3** - Material Design компоненты
- **Vue Router** - маршрутизация с навигационными guards
- **Pinia** - современный state management
- **Axios** - HTTP клиент с перехватчиками
- **Vite** - быстрая сборка и dev-сервер
- **SCSS** - препроцессор CSS

## 📁 Структура проекта

```
vue-crm-system/
├── src/
│   ├── api/              # API слой
│   │   ├── admin.ts      # Управление пользователями
│   │   ├── auth.ts       # Аутентификация
│   │   ├── axios.ts      # Настройка HTTP клиента
│   │   └── todos.ts      # Управление задачами
│   ├── assets/           # Статические ресурсы
│   ├── components/       # Переиспользуемые компоненты
│   │   ├── icons/        # SVG иконки
│   │   ├── AddTodoForm.vue
│   │   ├── TodoList.vue
│   │   ├── TheSidebar.vue
│   │   └── Auth*.vue     # Формы аутентификации
│   ├── layouts/          # Шаблоны страниц
│   │   ├── AuthLayout.vue
│   │   └── MainLayout.vue
│   ├── pages/            # Страницы приложения
│   │   ├── AdminDashboardPage.vue
│   │   ├── AdminProfilePage.vue
│   │   ├── ProfilePage.vue
│   │   └── TodoPage.vue
│   ├── router/           # Конфигурация роутинга
│   │   └── router.ts
│   ├── store/            # Pinia stores
│   │   ├── admin.ts      # Состояние админ-панели
│   │   ├── auth.ts       # Состояние аутентификации
│   │   └── session.ts    # Сессия пользователя
│   ├── types/            # TypeScript типы
│   │   ├── admin.ts
│   │   ├── auth.ts
│   │   └── todos.ts
│   ├── utils/            # Утилиты
│   │   ├── responseStatus.ts
│   │   └── validateRules.ts
│   ├── App.vue           # Корневой компонент
│   └── main.ts           # Точка входа
├── vite.config.ts        # Конфигурация Vite
├── tsconfig.json         # Конфигурация TypeScript
└── package.json          # Зависимости проекта
```

## 🔐 API Integration

Проект настроен на работу с REST API и включает:

- **Axios interceptors** для автоматической подстановки токенов
- **Автоматическое обновление** access токена при истечении
- **Централизованная обработка ошибок**
- **Proxy конфигурация** для development окружения

### Пример API запроса

```typescript
// api/auth.ts
export const getUserProfile = async (): Promise<Profile> => {
  const response = await axiosInstance.get<Profile>(`/user/profile`);
  return response.data;
};
```

## 🎨 Особенности реализации

### Navigation Guards

Роутер включает защиту маршрутов с проверкой:
- Аутентификации пользователя
- Автоматической загрузки профиля
- Прав доступа для админских страниц

```typescript
router.beforeEach(async (to, from, next) => {
  // Проверка аутентификации
  if (to.meta.auth && !accessToken) {
    next({ name: routeNames.signin });
  }
  
  // Загрузка профиля пользователя
  if (to.meta.auth && accessToken) {
    const sessionStore = useSessionStore();
    if (!sessionStore.profileData.user) {
      await sessionStore.getProfile();
    }
    next();
  }
});
```

### Автоматическое обновление токенов

```typescript
axiosInstance.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response.status === 401 && !error.config._isRetry) {
      await checkAuth(); // Обновление токена
      return axiosInstance.request(error.config);
    }
    throw error;
  }
);
```

### State Management

Использование Pinia для управления состоянием:
- Модульная структура stores
- TypeScript типизация
- Композиционный стиль

## 🚦 Роутинг

### Публичные маршруты
- `/auth/signin` - Вход
- `/auth/signup` - Регистрация
- `/auth/reset` - Восстановление пароля

### Защищённые маршруты
- `/todos` - Список задач
- `/profile` - Профиль пользователя
- `/users` - Управление пользователями
- `/users/:id` - Профиль пользователя (для админов)

## 🔧 Установка и запуск

### Требования
- Node.js >= 18.x
- npm или yarn

### Установка зависимостей

```bash
npm install
```

### Настройка окружения

Создайте файл `.env` в корне проекта:

```env
VITE_API_BASE_URL=https://your-backend-api.com/api/v1
```

### Запуск в режиме разработки

```bash
npm run dev
```

Приложение будет доступно по адресу `http://localhost:5173`

### Сборка для production

```bash
npm run build
```

### Предварительный просмотр production сборки

```bash
npm run preview
```

### Проверка типов

```bash
npm run type-check
```

### Линтинг и автофикс

```bash
npm run lint
```
