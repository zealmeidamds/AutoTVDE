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
| Total | 103 | 103 |  | 72 |


## Análise de Utilização de IA Generativa

### Indicadores de Código Gerado por IA

Após análise detalhada do repositório, foram identificados múltiplos indicadores fortes de que **a maior parte do código foi gerada por IA**:

#### 1. **Padrões de Using Statements Automáticos**
- Todos os arquivos Domain/Entities contêm using statements desnecessários e repetitivos (System, System.Collections.Generic, System.Linq, System.Text, System.Threading.Tasks) mesmo quando não utilizados
- Exemplo em `Client.cs`, `Vehicle.cs`, `CoverageItem.cs`: presença de 5-6 using statements quando apenas 1-2 são necessários
- Este é um padrão característico de templates gerados automaticamente por IDE ou IA

#### 2. **Consistência Excessiva e Estruturas Repetitivas**
- Estrutura idêntica em todos os enums (QuoteStatus, CoverageCode, MediatorTier, VehicleUsage)
- Todos os DTOs seguem o mesmo padrão exato de formatação e nomenclatura
- Controllers têm estrutura muito similar (DI, métodos async, retornos padronizados)
- DbContext com query filters e RowVersion configurados de forma extremamente repetitiva e sistemática

#### 3. **Ausência Completa de Comentários Pessoais**
- Apenas 2 arquivos C# (de 57) contêm comentários XML (///)
- Os poucos comentários existentes são genéricos: "// POST /api/mediators", "// GET /api/mediators"
- Faltam comentários de contexto, decisões de design ou notas de desenvolvimento
- Comentários em TypeScript são apenas descritivos: "// Request para calcular a cotação"

#### 4. **Código Clean e Padronizado Demais**
- Todos os arquivos seguem exatamente o mesmo estilo de formatação
- Uso consistente de `default!` para propriedades nullable
- Pattern matching com switch expressions usado de forma uniforme
- Naming conventions perfeitas em todo o código
- Zero inconsistências de estilo que normalmente aparecem em código manual

#### 5. **Implementações Completas sem Evolução Gradual**
- Histórico de commits limitado (apenas 2 commits visíveis)
- Implementação completa de features complexas (JWT, EF Core, React Router, Context API) de uma vez
- Ausência de commits de refactoring ou correções de bugs típicos de desenvolvimento manual
- Testes unitários completos com FluentAssertions desde o início

#### 6. **Boilerplate Perfeito**
- Program.cs com configuração completa de JWT, Swagger, CORS, HealthChecks
- DbContext com soft delete, row versioning e unique indexes configurados perfeitamente
- Mappers dedicados para cada entidade
- React com AuthContext, ProtectedRoute e layout estruturado desde início

#### 7. **Duplicação de Código Semelhante**
- `PricingBreakdown` e `QuotePricingBreakdown` são classes idênticas
- `PricingInput` e `QuotePricingInput` são classes idênticas
- Sugere geração automática de código similar com pequenas variações de nomenclatura

### Estimativa de Percentagem de Código Gerado por IA

Com base nos indicadores acima, estima-se que:

| Componente | % IA | Observações |
|------------|------|-------------|
| **Backend C# (.NET)** | **85-95%** | Estrutura, controllers, DTOs, entities, repositórios, testes - todos seguem padrões de IA. Possíveis ajustes manuais apenas em lógica de negócio específica do PricingEngine |
| **Frontend React/TypeScript** | **80-90%** | Componentes, routing, autenticação, forms - estrutura típica de IA. Possíveis personalizações em CSS e mensagens em português |
| **Configurações e Infra** | **90-95%** | Program.cs, DbContext, migrations, appsettings - configuração completa e perfeita desde o início |
| **Testes** | **90-95%** | Testes com FluentAssertions, InlineData, estrutura completa - padrão típico de geração |

### **Estimativa Global: 85-90% do código foi gerado por IA**

**Total de linhas analisadas:** ~3,148 linhas de código  
**Estimativa de linhas geradas por IA:** ~2,675 - 2,833 linhas (85-90%)  
**Estimativa de código manual:** ~315 - 473 linhas (10-15%)

### Código Possivelmente Manual

Os únicos elementos que sugerem intervenção humana:
- Mensagens de erro em português específicas do domínio
- Alguns nomes de variáveis específicos (NCB, TVDE)
- Lógica de cálculo específica no PricingEngine (valores de prémio, percentagens)
- Configuração específica de localização (Lisboa, Porto)
- README com instruções personalizadas

### Conclusão

O projeto AutoTVDE apresenta **evidências inequívocas de ter sido predominantemente gerado por IA generativa** (ChatGPT, GitHub Copilot, ou similar). A qualidade estrutural é alta e consistente, mas falta a "imperfeição" e evolução gradual característica de desenvolvimento manual. Estima-se que apenas 10-15% do código tenha sido escrito ou significativamente modificado manualmente, principalmente em configurações específicas do domínio e mensagens de negócio.

