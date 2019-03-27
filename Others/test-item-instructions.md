# Инструкция по созданию и регистрации отдельного теста

Оисание системы тестирования см. в документе ["Инструкция по тестированию дистрибутива CoLiTec"](Others/test-instructions.md)

1. Подготовить [начальные данные](Others/test-instructions.md#данные), заархивировать их в zip-архив и разместить на Google Drive
2. Подготовить [верифицированные данные](Others/test-instructions.md#данные), заархивировать их в zip-архив и разместить на Google Drive
3. Подготовить [модуль сравнения](Others/test-instructions.md#модули-сравнения) и разместить его в «BinWindows64» и «BinUbuntu64»
4. Подготовить [файл задачи](Others/test-instructions.md#задачи) и разместить его в «test/tasks»
5. Добавить ссылку на файл задачи в [файл сценария](Others/test-instructions.md#сценарии)

Google Drive использовать свой, под аккаунтом @eosda (не забыть предоставить общий доступ)

Репозиторий для резмещения модулей сравнения, файлов задач и сенариев использовать [TEST](https://subversion.assembla.com/svn/colitecclosed.test/trunk)
