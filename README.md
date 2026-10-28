<div align="center">

# FisioLife

**Sistema web de gestão para clínicas de fisioterapia**

Agenda online, prontuário eletrônico, financeiro e relatórios em um só lugar, com um portal para cada perfil da clínica.

![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MariaDB](https://img.shields.io/badge/MySQL%20%2F%20MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

![Testes](https://img.shields.io/badge/testes%20de%20integra%C3%A7%C3%A3o-52%20aprovados-2ea44f?style=flat-square)
![Perfis](https://img.shields.io/badge/perfis-4-0a66c2?style=flat-square)
![Tabelas](https://img.shields.io/badge/banco-22%20tabelas-0a66c2?style=flat-square)
![Licença](https://img.shields.io/badge/licen%C3%A7a-todos%20os%20direitos%20reservados-critical?style=flat-square)

<br>

<img src="screenshots/01-inicio.png" alt="Página inicial do FisioLife" width="900">

</div>

---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Telas do sistema](#telas-do-sistema)
- [Funcionalidades por perfil](#funcionalidades-por-perfil)
- [Como o sistema funciona](#como-o-sistema-funciona)
- [Segurança e proteção de dados](#segurança-e-proteção-de-dados)
- [Qualidade e testes](#qualidade-e-testes)
- [Tecnologias](#tecnologias)
- [Código-fonte e licença](#código-fonte-e-licença)
- [Autor](#autor)

---

## Sobre o projeto

Em muitas clínicas pequenas, os horários, os dados dos pacientes e os pagamentos ficam espalhados entre agendas de papel, planilhas e mensagens. Isso causa **horários marcados duas vezes**, **evoluções clínicas perdidas** e **dificuldade para saber quanto a clínica recebeu**.

O **FisioLife** reúne essa rotina em um único sistema web:

- 📅 **O paciente** vê os horários livres e marca a própria consulta, sem precisar ligar para a clínica.
- 🩺 **O fisioterapeuta** acompanha a agenda do dia e registra a evolução de cada atendimento no prontuário.
- 🗂️ **A recepção** organiza a agenda de todos os profissionais, os pacientes, os pagamentos e o caixa.
- 📊 **O administrador** gerencia contas e permissões, consulta relatórios financeiros e de produtividade e faz o backup do banco.

---

## Telas do sistema

> As telas abaixo mostram dados **fictícios** de demonstração.

### Área pública

| Serviços | Login |
| :---: | :---: |
| <img src="screenshots/02-servicos.png" alt="Página de serviços"> | <img src="screenshots/03-login.png" alt="Tela de login"> |

### Portais

| Paciente: agenda da semana e próximas consultas | Fisioterapeuta: turno e atendimentos do dia |
| :---: | :---: |
| <img src="screenshots/04-paciente.png" alt="Portal do paciente"> | <img src="screenshots/05-fisioterapeuta.png" alt="Portal do fisioterapeuta"> |

| Recepção: agenda de todos os profissionais | Prontuário e histórico do paciente |
| :---: | :---: |
| <img src="screenshots/06-recepcao.png" alt="Portal da recepção"> | <img src="screenshots/09-prontuario.png" alt="Prontuário do paciente"> |

| Administrador: pagamentos e planos mensais | Administrador: relatório financeiro |
| :---: | :---: |
| <img src="screenshots/07-financeiro.png" alt="Financeiro"> | <img src="screenshots/08-relatorio.png" alt="Relatório financeiro"> |

### Tema escuro e celular

| Tema escuro | Celular |
| :---: | :---: |
| <img src="screenshots/10-tema-escuro.png" alt="Tema escuro" width="620"> | <img src="screenshots/11-celular.png" alt="Versão para celular" width="230"> |

---

## Funcionalidades por perfil

<table>
<tr>
<td width="50%" valign="top">

### 🧑 Paciente
- Agenda por dia ou por semana, com os horários livres de cada fisioterapeuta
- Reserva com até 3 meses de antecedência
- Confirmação de presença e pedido de cancelamento
- Lista de espera quando não há vagas
- Histórico das consultas e do prontuário
- Acompanhamento do plano mensal (sessões usadas no mês)

</td>
<td width="50%" valign="top">

### 🩺 Fisioterapeuta
- Agenda do turno, com o status de cada atendimento
- Início e conclusão do atendimento
- Registro de evolução e de alta no prontuário
- Histórico clínico completo do paciente
- Registros permanentes: nada é sobrescrito

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🗂️ Recepção
- Agenda de todos os profissionais, com filtros
- Cadastro de pacientes e contas de acesso
- Horários extras e bloqueio de horários
- Pagamentos com itens da tabela de preços
- Caixa diário: abertura, suprimento, sangria e fechamento
- Comprovantes para imprimir ou salvar em PDF
- Pedidos de cancelamento e mural de avisos

</td>
<td width="50%" valign="top">

### 🛡️ Administrador
- Gestão de usuários, cargos e permissões
- Relatórios de faturamento e produtividade
- Exportação em CSV e versão para impressão
- Planos mensais e tabela de serviços
- Auditoria de acessos e alertas de segurança
- Backup completo do banco de dados

</td>
</tr>
</table>

---

## Como o sistema funciona

### Arquitetura

```mermaid
flowchart LR
    U["👤 Navegador<br/>HTML · CSS · JavaScript"] -- "Fetch API (JSON)" --> A["⚙️ Servidor PHP 8.2<br/>Páginas e APIs"]
    A -- "PDO · consultas parametrizadas" --> D[("🗄️ MySQL / MariaDB<br/>22 tabelas")]
    A -- "Sessão e cookie seguro" --> U
```

As páginas são geradas pelo servidor em PHP. As ações do dia a dia (marcar consulta, registrar evolução, lançar pagamento) passam por APIs em JSON, então a tela atualiza sem recarregar a página. Cada requisição confere quem é o usuário e o que o perfil dele pode fazer.

### Ciclo de uma consulta

```mermaid
stateDiagram-v2
    direction LR
    [*] --> Agendado: paciente reserva
    Agendado --> Confirmado: presença confirmada
    Confirmado --> EmAtendimento: fisioterapeuta inicia
    EmAtendimento --> Realizado: evolução registrada
    Agendado --> Cancelado: cancelamento
    Confirmado --> Cancelado: cancelamento
    Confirmado --> Faltou: paciente não compareceu
    Realizado --> [*]
```

Concluir um atendimento grava a evolução no prontuário **na mesma transação** que muda o status. Assim, nunca existe consulta "realizada" sem o registro clínico correspondente.

### Banco de dados

O modelo relacional tem **22 tabelas**: pacientes, profissionais, horários, consultas, prontuários, pagamentos, caixa, planos, auditoria e outras.

<details>
<summary><b>Ver o diagrama entidade-relacionamento (DER)</b></summary>
<br>
<img src="screenshots/12-diagrama-der.png" alt="Diagrama entidade-relacionamento do FisioLife">
</details>

---

## Segurança e proteção de dados

O sistema guarda dados de saúde, que a LGPD classifica como **dados sensíveis**. Por isso a segurança faz parte do projeto desde o início:

| Mecanismo | Como funciona |
| --- | --- |
| 🔐 **Senhas criptografadas** | Senhas guardadas com hash bcrypt. A senha temporária precisa ser trocada no primeiro acesso. |
| 🚫 **Limite de tentativas** | O login é bloqueado temporariamente depois de várias tentativas erradas. |
| ⏱️ **Sessão com prazo** | A sessão encerra após 30 minutos sem uso. Trocar a senha ou desativar a conta derruba as sessões abertas. |
| 🛡️ **Proteção CSRF** | Toda ação que altera dados exige um token válido da sessão. |
| 💉 **Proteção contra SQL Injection** | Todas as consultas usam PDO com parâmetros separados do SQL. |
| 👥 **Acesso por perfil** | O fisioterapeuta vê apenas os próprios atendimentos, o paciente vê só os próprios dados e a recepção não acessa o prontuário. |
| 📝 **Auditoria** | Acessos e ações importantes ficam registrados para o administrador. |
| 🔑 **Recuperação de senha** | Código enviado por e-mail, válido por 15 minutos, com limite de tentativas. |

---

## Qualidade e testes

| Tipo | O que verifica |
| --- | --- |
| ✅ **52 testes de integração** | Login, permissões, CSRF, reservas, conflito de horários, prontuário, alta, caixa, pagamentos, recuperação de senha, sessões e backup |
| ✅ **Testes de validação** | Datas, horários e CPF |
| ✅ **Testes de navegador** (Playwright) | Calendários do paciente e do fisioterapeuta, uso pelo celular, teclado e tema escuro |

Os testes rodam em um banco separado e descartável. Os dados reais da clínica nunca são usados.

---

## Tecnologias

| Camada | Tecnologias |
| --- | --- |
| **Interface** | HTML5, CSS3 (Grid e Flexbox), JavaScript, Bootstrap 5.3, Bootstrap Icons |
| **Servidor** | PHP 8.2, sessões PHP, Fetch API e JSON |
| **Banco de dados** | MySQL / MariaDB 10.4 (InnoDB, chaves estrangeiras e transações) |
| **Ambiente** | XAMPP (Apache, MariaDB, phpMyAdmin) e Git |
| **Testes** | PHP CLI, Python e Playwright |

---

## Código-fonte e licença

Este repositório é uma **vitrine do projeto**: apresenta o sistema, as telas e o funcionamento. **O código-fonte é privado.**

© 2026 Thiago Soethe Paschoal. **Todos os direitos reservados.** É proibido copiar, modificar, distribuir ou usar comercialmente o sistema, as telas ou a documentação sem autorização por escrito do autor. Veja o arquivo [LICENSE](LICENSE).

Para avaliação acadêmica, demonstração ou interesse comercial, entre em contato pelo GitHub: [github.com/thiagosoethe](https://github.com/thiagosoethe).

---

## Autor

<table>
<tr>
<td>

**Thiago Soethe Paschoal**<br>
Análise e Desenvolvimento de Sistemas, UniSENAI Joinville/SC<br>
Projeto Individual, 2026<br>
[github.com/thiagosoethe](https://github.com/thiagosoethe)

</td>
</tr>
</table>
