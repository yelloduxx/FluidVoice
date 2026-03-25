# CLAUDE.md

Этот файл содержит инструкции для Claude Code при работе с репозиторием FluidVoice.

## О проекте

FluidVoice — полностью открытое macOS-приложение для голосового ввода текста с AI-улучшением.
Поддерживаемые модели: Parakeet v3 & v2, Apple Speech, Whisper.

Репозиторий автора: https://github.com/altic-dev/FluidVoice

## Структура проекта

```
Sources/Fluid/
├── Analytics/
├── Models/
├── Networking/
├── Persistence/        # SettingsStore.swift — хранение настроек
├── Resources/
├── Services/           # TranscriptionSoundPlayer.swift и другие сервисы
├── UI/                 # SettingsView.swift и другие экраны
├── Views/
├── Theme/
├── AppDelegate.swift
├── ContentView.swift
└── fluidApp.swift
```

## Зависимости

- **FluidAudio** — Swift-фреймворк для локальной обработки аудио (ASR, диаризация, VAD)
  - Его CLAUDE.md находится в `.build/index-build/checkouts/FluidAudio/CLAUDE.md`

## Проектные соглашения (не баги)

### `DictationAIPostProcessingGate.isConfigured()` — `hasCustomPrompt` bypass

```swift
let hasCustomPrompt = settings.selectedPromptID(for: .dictate) != nil
guard settings.enableAIProcessing || hasCustomPrompt else { return false }
```

**Это намеренное поведение, не баг.** Если пользователь выбрал кастомный промпт — AI должен работать, даже если мастер-toggle "AI Enhancement" выключен. Логика: выбор промпта = явный сигнал "хочу AI-обработку". Toggle управляет только default-обработкой (без промпта). Это касается всех путей: обычная диктовка, reprocess, prompt-mode хоткей, тест промпта в настройках.

## Правила разработки

- Перед изменением любого файла — прочитать его
- Не делать git push без явной просьбы пользователя
- Не коммитить без явной просьбы
- Не добавлять лишнего: фичи, рефакторинг, комментарии — только то, что просят
- Читать `memory.md` в начале каждой сессии

## Текущая ветка

`feature/transcription-sound-volume`
