# Proposta Técnica: Desenvolvimento de Website Institucional e CMS

Este documento estabelece as diretrizes técnicas, arquitetura de software e o cronograma de execução para o desenvolvimento do website institucional e plataforma de blog.

---

## 1. Arquitetura e Stack Tecnológica

A solução será construída com tecnologias de alta performance, priorizando segurança, escalabilidade e otimização para motores de busca (SEO).

* **Frontend:** SvelteKit (Framework com renderização no lado do servidor - SSR).
* **Estilização:** Tailwind CSS (Arquitetura utilitária para design responsivo).
* **Backend:** Go / Golang (Linguagem de alta performance para API REST).
* **Banco de Dados:** PostgreSQL (Sistema de gerenciamento de dados relacional).
* **Infraestrutura:** Integração contínua (CI/CD) via GitHub com deploy em Vercel e Railway.

---

## 2. Cronograma de Desenvolvimento

O projeto será executado em duas fases distintas, garantindo entregas incrementais e validação contínua.

### Fase 1: Interface e Validação Estrutural (1 a 2 semanas)
*Foco na entrega da presença digital e fidelidade visual.*

* **Desenvolvimento de Páginas Institucionais:**
    * Home (Narrativa principal e conversão).
    * Nossa História (Identidade corporativa).
    * Atuação (Áreas de especialidade).
    * Equipe (Perfis profissionais).
    * Blog (Interface de leitura e listagem).
* **Otimização Técnica (SEO):**
    * Configuração de metadados dinâmicos e títulos semânticos.
    * Otimização de performance (Core Web Vitals).
    * Implementação de design responsivo para dispositivos móveis e desktop.

### Fase 2: Inteligência e Gestão de Conteúdo (2 a 3 semanas)
*Foco na autonomia administrativa e dinamismo de dados.*

* **Desenvolvimento do Backend em Go:**
    * Criação de API para persistência de artigos e categorias de blog.
    * Sistema de autenticação segura para acesso administrativo.
* **Painel Administrativo (CMS):**
    * Interface de gerenciamento de conteúdo (CRUD de artigos).
    * Integração com editor de texto rico (Rich Text) via TipTap.
    * **Módulo de Importação:** Funcionalidade para conversão automática de arquivos Microsoft Word (.docx) em HTML semântico, preservando a estrutura de SEO.
* **Integração e Deploy:**
    * Migração de textos para o banco de dados.
    * Configuração de ambiente de produção e certificados de segurança (SSL).

---

## 3. Critérios de Desempenho e Entrega

Para a aceitação do projeto, os seguintes parâmetros técnicos devem ser atendidos:

1.  **Performance:** Nota mínima de 90/100 no Google PageSpeed Insights.
2.  **SEO:** URLs amigáveis, sitemap.xml automatizado e robots.txt configurado.
3.  **Segurança:** Proteção nativa contra vulnerabilidades XSS, CSRF e SQL Injection.
4.  **Autonomia:** Capacidade de edição total do conteúdo do blog sem necessidade de intervenção em código.

---


### Estimativa de Investimento

| Item       | Descrição                                          | Investimento     |
| :--------- | :------------------------------------------------- | :--------------- |
| **Fase 1** | Design, Desenvolvimento Front-end e Otimização SEO | R$ [Inserir]     |
| **Fase 2** | Backend em Go, Painel Admin e Importação de Word   | R$ [Inserir]     |
| **Total**  | **Desenvolvimento Completo**                       | **R$ [Inserir]** |

**Observações:**
- Os custos de domínio e serviços de terceiros (hospedagem/e-mail) são de responsabilidade do cliente.
- Pagamento parcelado conforme as entregas de cada fase.