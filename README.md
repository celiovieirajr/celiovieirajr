# 🧑‍💻 Célio Vieira Jr.

## 🚀 Sobre Mim

Profissional com experiência em qualidade de software e suporte técnico, buscando oportunidades como **Desenvolvedor Júnior** ou **Analista de Testes Automatizados Júnior**.

- **Foco**: API REST, geoprocessamento, automação de testes e documentação
- **Metodologias**: Scrum · Kanban
- **Perfil**: Proativo, com foco em aprendizado contínuo e aprimoramento de habilidades em desenvolvimento e automação de testes

---

## ⚙️ Tech Stack

### Backend & Frameworks
![Java](https://img.shields.io/badge/Java-17%20%7C%2023-blue?logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x%20%7C%204.x-brightgreen?logo=spring)
![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-Hibernate-6DB33F?logo=spring)
![Spring WebFlux](https://img.shields.io/badge/Spring_WebFlux-WebClient-6DB33F?logo=spring)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-3.x-005F0F?logo=thymeleaf)

### Database & Migrations
![H2](https://img.shields.io/badge/H2-Database-blue)
![MySQL](https://img.shields.io/badge/MySQL-8.0-orange?logo=mysql)
![Liquibase](https://img.shields.io/badge/Liquibase-YAML_Changelogs-2962FF?logo=liquibase)

### Geospatial & Data Processing
![JTS](https://img.shields.io/badge/JTS-1.19.0-teal)
![OpenCSV](https://img.shields.io/badge/OpenCSV-5.12-yellow)
![Apache POI](https://img.shields.io/badge/Apache_POI-Excel-green?logo=apache)
![Jsoup](https://img.shields.io/badge/Jsoup-HTML_Scraping-orange)

### Documentation & API
![Swagger](https://img.shields.io/badge/OpenAPI%20%2F%20Swagger-3.0-85EA2D?logo=swagger)
![Postman](https://img.shields.io/badge/Postman-API_testing-orange?logo=postman)

### DevOps & Quality
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-purple?logo=githubactions)
![JaCoCo](https://img.shields.io/badge/JaCoCo-≥70%25-red)
![JUnit 5](https://img.shields.io/badge/JUnit_5-Tests-25A162?logo=junit5)
![Mockito](https://img.shields.io/badge/Mockito-5.x-blue)

### Integrações Extras
![Twilio](https://img.shields.io/badge/Twilio-SMS_SDK-red?logo=twilio)
![Gmail SMTP](https://img.shields.io/badge/Gmail_SMTP-Notificações-EA4335?logo=gmail)

---

## 🌟 Projetos de Destaque

| Projeto | Descrição | Stack | Build | Cobertura |
|---|---|---|---|---|
| **lab-cana-fire** | Monitor de focos de incêndio em canaviais com geoprocessamento, ingestão reativa de CSVs do INPE, alertas por e-mail, exportação Excel e UI Thymeleaf | Java 17 · Spring Boot 3.x · JTS · OpenCSV · Thymeleaf · Jsoup · Apache POI · JDA (Discord) · Twilio | ![Pass](https://img.shields.io/badge/build-passing-brightgreen) | ![≥70%](https://img.shields.io/badge/coverage-≥70%25-success) |
| **E-Commerce Core API** | API REST de plataforma de vendas online com domínios modulares, migrações Liquibase, integração ViaCEP, documentação OpenAPI e perfis homolog/prod | Java 23 · Spring Boot 4.x · Liquibase · H2/MySQL · OpenAPI/Swagger · Jakarta Validation | ![Pass](https://img.shields.io/badge/build-passing-brightgreen) | - |

<details>
<summary>🔍 Insights Detalhados</summary>

### 🔥 lab-cana-fire

Sistema **end-to-end** de monitoramento de focos de calor que automatiza todo o fluxo:

- **🛰️ Ingestão**: Scheduler a cada 10 min faz scraping no diretório do INPE com **Jsoup**, baixa o CSV mais recente via **WebClient** reativo e parseia com **OpenCSV**
- **🗺️ Geoprocessamento**: Converte coordenadas para pontos **JTS** e faz *spatial join* contra múltiplos polígonos GeoJSON (`aracatuba.geojson`, `saoPaulo.geojson`, `bh.geojson`, `goias.geojson`, `matoGrosso.geojson`, `brasil.geojson`)
- **📊 Persistência**: Alertas salvos em **H2** com UUID auto-gerado
- **📧 Notificações**: E-mail rico via **Gmail SMTP** com `AlertMessageBuilder` (emojis, coordenadas, link Google Maps)
- **🌐 Interfaces**:
  - REST API completa (`/alerts` CRUD + `/alerts/latest`)
  - UI **Thymeleaf** com tabela de alertas e links para mapa
  - Exportação **Excel** via Apache POI (`/alerts/exports`)
  - **📱 Canal extra** (dependência declarada): **SMS** (Twilio SDK)
- **🏗️ Arquitetura**: Packages separados (`ingestion`, `service`, `repository`, `web`, `mapper`, `dto`, `utils`, `exception`) com `AlertService` (interface + impl), `EmailService`, `HotspotService`, `GlobalExceptionHandler`
- **🧪 Qualidade**: JaCoCo ≥ 70%, CI via GitHub Actions com upload de relatórios
- **📁 Homolog**: Submódulo `lab-cana-fire-hom` com configuração independente e escopo reduzido (apenas SP)

### 🛒 E-Commerce Core API

API REST modular seguindo boas práticas de arquitetura corporativa:

- **📦 Domínios**: `Customer`, `Product`, `Sale`, `ItemSale`, `Viacep` (Embeddable) — cada um com seu controller, service (interface + impl), repository e mapper
- **🗄️ Migrações**: **Liquibase** com changelogs YAML versionados (3 changesets: criação de tabelas, seed data, reset de identidades)
- **🌍 Perfis**:
  - `homolog` → H2 in-memory (porta 3000), console habilitado, debug SQL/Liquibase
  - `prod` → MySQL (porta 8000), credenciais via variáveis de ambiente
- **🔗 Integração ViaCEP**: Chamada reativa via **WebClient** com tratamento de erros e DTO específico
- **📐 Validações**: Bean Validation (`@NotBlank`, `@CPF`, `@Positive`) + validação customizada de CPF com algoritmo de dígitos verificadores
- **📖 Documentação**: **OpenAPI/Swagger** com `@Operation`, `@ApiResponse` e `@Tag` em todos os endpoints
- **🚨 Exception Handling**: `@RestControllerAdvice` global com `ApiException`, `ViacepException`, `FieldErrorDto` e respostas padronizadas com timestamp
- **🧪 Testes**: JUnit 5 + Mockito (unitários e integração)
- **⚙️ CI**: GitHub Actions com JDK 23, Maven cache e `mvn -B package`
- **🔮 Roadmap**: Dockerização, deploy automatizado (blue/green), métricas Prometheus, JaCoCo + Checkstyle

</details>

---

## 🎯 Competências Técnicas

| Categoria | Tecnologias |
|---|---|
| **Linguagens** | Java 17/23, SQL |
| **Frameworks** | Spring Boot 3.x/4.x, Spring Data JPA, Spring WebFlux, Thymeleaf |
| **ORM / Migrações** | JPA / Hibernate, Liquibase (YAML) |
| **Bancos** | H2 (dev/test), MySQL (prod) |
| **Geoprocessamento** | JTS Topology Suite, GeoJSON, PreparedGeometry |
| **Processamento** | OpenCSV, Apache POI, Jsoup |
| **API & Docs** | REST, OpenAPI/Swagger, Postman |
| **Testes** | JUnit 5, Mockito, JaCoCo (≥ 70%) |
| **CI/CD** | GitHub Actions, Maven |
| **DevOps** | Docker (planejado), perfis de ambiente (homolog/prod) |
| **Notificações** | Gmail SMTP, Twilio SMS |
| **Metodologias** | Scrum, Kanban |

---

## 📫 Contato

[![LinkedIn](https://img.shields.io/badge/LinkedIn-celiovieirajr-blue?logo=linkedin)](https://www.linkedin.com/in/celiovieirajr)
✉️ celiojuniorata@outlook.com
