# Avaliação por critério — Lorrane Santos

| Critério | Peso (pts) | Pontuação Máxima | Observações | Pontuação (Lorrane Santos) |
| --- | --- | --- | --- | --- |
| • Organização por funcionalidade (Controllers, DTOs, Services, Persistence) | 20 | 20 | Estrutura clara com separação Domain/Infrastructure/API/Tests, DTOs organizados por funcionalidade, repositórios bem implementados | 20 |
| • Uso correto de async/await e CancellationToken onde aplicável | 10 | 10 | Async/await usado corretamente em todos os controllers e repositórios, mas CancellationToken não foi utilizado em nenhum método de IO | 5 |
| • DTOs vs Entidades (não expor entities) e DataAnnotations | 15 | 15 | DTOs bem implementados, entidades nunca expostas diretamente, DataAnnotations presentes (Required, EmailAddress, Range, RegularExpression, StringLength) | 15 |
| • Consultas performance-friendly (AsNoTracking, transações onde necessário) | 15  | 15  | AsNoTracking usado consistentemente em operações de leitura (ClientsController, repositories), mas faltam transações explícitas onde seriam apropriadas | 12 |
| • Validações de negócio (idade ≥18; ano ≥2000) | 10 | 10 | Validação de idade (≥18) implementada corretamente em ClientsController e QuotesController, mas validação de ano do veículo (≥2000) não foi encontrada | 5 |
| • Sem segredos commitados; uso de user-secrets (bónus) | 10 | 10 | Chave JWT e connection string commitadas em appsettings.json, sem uso de user-secrets - problema de segurança crítico | 2 |
| • HealthCheck /healthz (bónus: Serilog/OpenTelemetry) | 8 | 8 | HealthCheck implementado em /health com DbContext check, mas sem Serilog ou OpenTelemetry | 5 |
| • Instruções para DB InMemory/SQLite | 5 | 5 | Usa SQL Server LocalDB (não InMemory/SQLite), mas README tem boas instruções e auto-migration configurada | 3 |
| • Onboarding ≤ 15 min | 5 | 5 | README claro com pré-requisitos, comando único npm start, auto-migration, exemplos de requests e Swagger configurado | 5 |
| • Métricas/Tracing simples | 5 | 5 | Sem implementação de métricas, tracing ou uso de ILogger nos controllers | 0 |

