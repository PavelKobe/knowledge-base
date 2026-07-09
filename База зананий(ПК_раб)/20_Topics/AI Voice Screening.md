# AI Voice Screening

AI Voice Screening - автоматический первичный обзвон кандидатов с записью, расшифровкой, оценкой и передачей результата HR.

## Где используется

- [[VoiceScreen - голосовой AI-скрининг]]

## Типовой поток

```mermaid
flowchart LR
    HR[HR uploads candidates] --> Dispatch[Dispatch queue]
    Dispatch --> Call[Outbound call]
    Call --> STT[Speech to text]
    STT --> Dialog[Scenario dialog]
    Dialog --> Score[Final scoring]
    Score --> Report[Shortlist / report]
```

## Важные части

- телефония: [[Voximplant]];
- распознавание и синтез речи: [[Yandex SpeechKit]];
- сценарий вопросов: YAML/DB сценарии;
- финальная оценка: [[OpenRouter]];
- записи звонков: [[Yandex Object Storage]];
- фоновые задачи: [[Celery]] + [[Redis]].
