**Команди:**
docker compose up --build -d - старт контейнера
docker compose exec stand python scripts/doctor.py - діагностика
docker compose run --rm eval --only C-19 - прогін одиночних (в приклАді С-19)
docker compose run --rm eval --profiles clean --baseline-runs 2 - прогін бейслайн
docker compose run --rm eval --profiles lesson-02 --runs 3 --baseline-runs 2 - прогін профіля з бейсланом
**Профілі:** clean, lesson-01, lesson-02  
**Модель судді:** claude-haiku-4-5 

