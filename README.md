# SoundRequestTwitch

**SoundRequestTwitch** — Windows-приложение для музыкальных заявок на Twitch через Channel Points.

Разработчик: **DeadHardGaming**  
Twitch: [DeadHardGamingPOE](https://www.twitch.tv/DeadHardGamingPOE)

## Возможности

- вход через Twitch OAuth;
- автоматическая настройка Channel Points;
- очередь заявок и ручная модерация;
- автоматическое воспроизведение следующего трека;
- OBS Browser Source Overlay;
- прозрачный и настраиваемый виджет;
- цветовые пресеты, анимации и пульсация;
- выбор шрифта и размера текста;
- отображение обложки, названия, длительности и прогресса;
- MP4/WebM-плеер без интерфейса YouTube;
- сохранение Twitch-сессии после перезапуска;
- профессиональная инструкция внутри приложения;
- портативный `.exe`, установка не требуется.

## Первый запуск

1. Запустите `SoundRequestTwitch 1.0.0.exe`.
2. Откройте **Настроить данные Twitch**.
3. Создайте приложение в [Twitch Developer Console](https://dev.twitch.tv/console/apps).
4. В Redirect URL укажите:

```text
http://localhost:3000/auth/twitch/callback
```

5. Введите Client ID и Client Secret в приложение.
6. Нажмите **Войти через Twitch**.
7. Разрешите доступ к Channel Points.

Client Secret хранится зашифрованно и не встраивается в EXE.
