# Skystore — каталог товаров на Django

Учебный проект: каталог товаров и блог. PostgreSQL, админка, фикстуры, контроллеры на классах (CBV), формы с проверками.

## Быстрый старт (Windows)

1. Создайте базу в PostgreSQL (через pgAdmin или psql):

   ```sql
   CREATE DATABASE skystore_db;
   ```

2. Скопируйте `.env.sample` в `.env` и впишите свой пароль от PostgreSQL в `POSTGRES_PASSWORD`.
3. Запустите `setup.bat` двойным щелчком или из терминала. Скрипт установит зависимости, создаст таблицы, загрузит тестовые товары и запустит сервер.
4. Откройте http://127.0.0.1:8000/

Суперпользователь для админки (http://127.0.0.1:8000/admin/):

```bash
poetry run python manage.py createsuperuser
```

## Запуск вручную

```bash
poetry install
poetry run python manage.py migrate
poetry run python manage.py add_products
poetry run python manage.py runserver
```

Все команды пишутся через `poetry run`: так они выполняются в виртуальном окружении проекта, где установлен Django.

## Страницы

| Адрес | Что там |
|---|---|
| `/` | список товаров |
| `/products/create/` | новый товар |
| `/products/<pk>/` | страница товара |
| `/products/<pk>/update/` | редактирование товара |
| `/products/<pk>/delete/` | удаление товара |
| `/contacts/` | контакты и форма обратной связи |
| `/blogs/` | список опубликованных статей |
| `/blogs/create/` | новая статья |
| `/blogs/<pk>/` | статья (при каждом открытии растёт счётчик просмотров) |
| `/blogs/<pk>/update/` | редактирование статьи, после сохранения открывается статья |
| `/blogs/<pk>/delete/` | удаление статьи |

## Проверки в форме товара

`ProductForm` в `catalog/forms.py`:

- в названии и описании нельзя использовать слова из списка `FORBIDDEN_WORDS` (регистр не важен);
- цена не может быть отрицательной (`clean_price`);
- изображение — только JPEG или PNG, не больше 5 МБ (`clean_image`).

Стили Bootstrap всем формам добавляет `StyleFormMixin` в методе `__init__`.

## Тестовые данные

`add_products` удаляет все категории и товары и загружает фикстуры из `catalog/fixtures/`. Загрузить их вручную:

```bash
poetry run python manage.py loaddata category.json
poetry run python manage.py loaddata product.json
```

Выгрузить данные в фикстуры (`-Xutf8` нужен на Windows, чтобы сохранилась кириллица):

```bash
poetry run python -Xutf8 manage.py dumpdata catalog.Category --indent 4 -o catalog/fixtures/category.json
poetry run python -Xutf8 manage.py dumpdata catalog.Product --indent 4 -o catalog/fixtures/product.json
```

## Структура

- `config/` — настройки проекта
- `catalog/` — товары, категории, контакты: модели, формы, контроллеры, шаблоны, фикстуры, команда `add_products`
- `catalog/templates/catalog/base.html` — базовый шаблон; `includes/` — подшаблоны (меню, поля формы, изображение товара)
- `blog/` — приложение блога
- `screenshots/` — скриншоты работы в Django shell
