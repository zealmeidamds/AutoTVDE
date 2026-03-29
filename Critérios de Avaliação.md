📊 Rubrica de Avaliação de Qualidade de Código
1) Arquitetura & Estrutura do Repositório — 20 pts
Critérios (o que observar)

Separação de camadas clara: Domain, Application, Infrastructure, Server (API), Client (UI), Tests.
Dependências unidirecionais (Application → Domain; Infrastructure → Domain/Application; Server → Infrastructure/Application/Domain).
Ficheiros de engenharia presentes e configurados: .gitignore, .editorconfig, Directory.Build.props.
Nomes e namespaces consistentes; organização por funcionalidade (p.ex., Controllers, DTOs, Services, Persistence).

Como verificar

Abrir solução e inspecionar a estrutura, referências entre projetos e namespaces.
Confirmar que UI Hosted serve WASM e API na mesma origem (evita CORS).
Procurar Directory.Build.props com TargetFramework=net8.0, Nullable=enable.

Red flags

“God projects” (toda a lógica num único projeto ou controlador).
Dependências cíclicas; lógica de domínio dentro de controllers/API.
Ausência de .editorconfig e convenções básicas.

Pontuação

0–5: estrutura confusa, dependências invertidas.
6–12: estrutura razoável, alguns desvios.
13–20: estrutura limpa, coerente e escalável.


2) Qualidade de Código & Boas Práticas C# — 15 pts
Critérios

SOLID básico (Single Responsibility, Interface Segregation, Dependency Injection).
Naming e legibilidade (métodos curtos, sem side-effects ocultos).
Imutabilidade/encapsulamento mínimo nas entidades (evitar setters públicos indiscriminados).
Async/await e cancellation onde aplicável; evitar .Result/.Wait().

Como verificar

Amostra de classes: PricingEngine, Controllers, Services.
Ver se DI está bem configurado no Program.cs (scopes corretos, sem new espalhados).
Procurar CancellationToken nos métodos de IO (EF, HTTP, etc.) — bónus.

Red flags

Métodos longos (>100 linhas), múltiplas responsabilidades.
static abusivo, estado global.
Uso de var inconsistente, async void, exceções silenciosas.

Pontuação

0–5: violações claras de boas práticas.
6–10: aceitável, melhorias pontuais.
11–15: muito bom, consistente e idiomático.


3) Web API & Boas Práticas — 15 pts
Critérios

ProblemDetails (RFC7807) para erros de validação/negócio (400) e exceções (500).
Swagger/OpenAPI completo com Bearer Auth e exemplos.
Paginação consistente (items, total, page, pageSize) e AsNoTracking().
DTOs vs. Entidades (não expor entities diretamente).
Validação DataAnnotations nos DTOs (ex.: EmailAddress, Range, RegularExpression).

Como verificar

Inspecionar Program.cs (middlewares de exceções/validação).
Abrir /swagger e ver schemas e exemplos.
Examinar controllers: assinaturas, DTOs de entrada/saída, ModelState.

Red flags

Retorno de entidades EF diretamente.
Mensagens de erro genéricas; ausência de ValidationProblemDetails.
Endpoints sem Authorize onde era esperado.

Pontuação

0–5: API frágil, sem padronização.
6–10: API funcional, lacunas menores.
11–15: API bem desenhada, pronta para consumo.


4) Dados & EF Core — 10 pts
Critérios

AppDbContext com mapeamentos (comprimentos, required, índices únicos), seeds, RowVersion (concorrência).
Migrations presentes; auto‑migrate no arranque com fallback EnsureCreated().
Consultas com AsNoTracking() para leitura; transações onde aplicável.

Como verificar

OnModelCreating: HasIndex(...).IsUnique(), Property(...).IsRowVersion().
Pasta Migrations/; Program.cs chama Migrate() se existirem migrações.

Red flags

Ausência de índices/constraints (ex.: NIF único).
Entidades sem comprimentos/required; .Include() exagerado sem necessidade.

Pontuação

0–3: mapeamentos/migrations insuficientes.
4–7: razoável com alguns gaps.
8–10: sólido e previsível.


5) Regras de Negócio (Pricing) — 10 pts
Critérios

Regras implementadas no PricingEngine (isolado e testável).
Arredondamentos consistentes; breakdown correto; validações de negócio (idade ≥18; ano ≥2000).
Sem hardcode disperso nos controllers (centralização no motor).

Como verificar

Ler PricingEngine: aplica potência, idade, TVDE, cidade, NCB, opcionais.
Ver o exemplo numérico (Porto, NCB3, GLASS) ~€340,37.

Red flags

Regras duplicadas em múltiplos pontos.
Alterações do negócio misturadas com IO.

Pontuação

0–3: regras incompletas ou erradas.
4–7: corretas com pequenas inconsistências.
8–10: corretas, limpas e testáveis.


6) Segurança (Auth, Roles, Dados) — 10 pts
Critérios

JWT Bearer configurado (Issuer, Audience, Key), expiração definida; [Authorize] aplicado.
Roles (Admin, Mediator) efetivas em endpoints sensíveis.
Input sanitization básico; no secrets commitados (appsettings.* sem chaves reais).

Como verificar

Program.cs: AddAuthentication().AddJwtBearer(...) + AddAuthorization.
AuthController e tokens; credenciais dev in-memory.
Repositório: verificar segredos; dotnet user-secrets (bónus).

Red flags

Endpoints críticos sem autorização.
JWT incompleto; secret Key fraca/committed.

Pontuação

0–3: lacunas graves.
4–7: aceitável; pequenos ajustes.
8–10: robusto para MVP.


7) Logs & Observabilidade — 8 pts
Critérios

Logging com ILogger<T> (níveis: Information, Warning, Error).
Contexto nos logs (IDs, número de cotação/política).
HealthCheck (/healthz); (bónus) Serilog ou OpenTelemetry.

Como verificar

Procure ILogger<> nos controllers/serviços.
Endpoint /healthz responde 200.

Red flags

Console.WriteLine em vez de ILogger.
Logging ruidoso (PII em logs), ou inexistente.

Pontuação

0–2: sem logs/health.
3–5: mínimo aceitável.
6–8: bom nível para suporte/diagnóstico.


8) Testes (Unit & Integração — esqueleto) — 7 pts
Critérios

Unit tests do PricingEngine: cobrem faixas e o caso numérico.
Integração: WebApplicationFactory<Program> preparado (mesmo que Skip) com instruções para DB InMemory/SQLite.

Como verificar

tests/Mds.Insurance.Tests.Unit/*; dotnet test a verde.
tests/Mds.Insurance.Tests.Integration/* com fixtures e Skip=TODO.

Red flags

Sem unit tests; testes só de controller sem lógica de negócio.

Pontuação

0–2: ausência de testes.
3–5: unit ok; integração por fazer.
6–7: cobertura mínima bem pensada.


9) Documentação & DX — 5 pts
Critérios

README claro: pré‑requisitos, connection string, migrations, execução, credenciais dev, link Swagger, fluxo.
Requests/api.http com exemplos de chamadas.
Tempo de onboarding ≤ 15 min.

Como verificar

Seguir README; levantar API/UI; executar fluxo completo.

Red flags

README escasso; sem exemplos; passos omissos.

Pontuação

0–2: documentação insuficiente.
3–4: boa; poucos ajustes.
5: excelente, onboarding fácil.


10) Performance & Manutenibilidade (bónus) — 5 pts
Critérios

Caching (ex.: cotação por 15 min).
Configuração via IOptions<T>; separação appsettings.*.
Mensurabilidade: métricas simples; time-to-price e latências.

Pontuação bónus

0–2: poucos sinais.
3–5: melhorias claras e conscientes.