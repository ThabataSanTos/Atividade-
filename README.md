# Atividade-

Parte 1: Identificação de Problemas

1. Violação do SRP (Single Responsibility Principle)

Classe: GerenciadorBiblioteca
Métodos: AdicionarUsuario, RealizarEmprestimo, RealizarDevolucao, EnviarEmail, EnviarSMS

Explicação:
A classe GerenciadorBiblioteca concentra múltiplas responsabilidades: gerencia livros, usuários, empréstimos, envia notificações e calcula multas. Essa centralização dificulta a manutenção e a realização de testes.

Má prática:
Cada classe deve ter apenas um motivo para mudar. Qualquer alteração nas regras de envio de notificação, por exemplo, exigiria modificar a mesma classe que controla os empréstimos.


---

2. Violação do OCP (Open/Closed Principle)

Classe: GerenciadorBiblioteca
Métodos: RealizarEmprestimo, RealizarDevolucao

Explicação:
A lógica de notificação está acoplada diretamente nos métodos. Se for necessário adicionar uma nova forma de notificação (ex: push notification), será preciso modificar esses métodos diretamente.

Má prática:
O código deve ser aberto para extensão e fechado para modificação.


---

3. Violação do DIP (Dependency Inversion Principle)

Classe: GerenciadorBiblioteca
Métodos: EnviarEmail, EnviarSMS

Explicação:
A classe depende diretamente de implementações concretas para envio de e-mails e SMS. O ideal seria depender de abstrações (interfaces), permitindo flexibilidade.

Má prática:
Isso dificulta a substituição por outros meios de notificação (ex: via aplicativo).


---

4. Violações de Clean Code – Nomes de Classe e Métodos Confusos

Classe: GerenciadorBiblioteca
Método: Todos

Explicação:
Nomes genéricos e pouco claros dificultam o entendimento do código. Por exemplo, AdicionarLivro poderia ser parte de um LivroService, com responsabilidades mais bem definidas.

Má prática:
Nomes devem expressar claramente a intenção e escopo da funcionalidade.


---

5. Violações de SRP + Clean Code – Métodos Muito Longos

Métodos: RealizarEmprestimo, RealizarDevolucao

Explicação:
Estes métodos realizam diversas tarefas: busca, verificação de regras, alterações de estado e envio de notificações. Essa sobrecarga compromete a clareza e a testabilidade.

Má prática:
Métodos devem ser pequenos, coesos e fáceis de testar.
