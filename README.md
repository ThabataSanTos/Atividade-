# Atividade-

Parte 1: Identificação de Problemas.


---

1. Violação do SRP (Single Responsibility Principle)

Classe: GerenciadorBiblioteca
Métodos: AdicionarUsuario, RealizarEmprestimo, RealizarDevolucao, EnviarEmail, EnviarSMS
Explicação:
A classe GerenciadorBiblioteca tem múltiplas responsabilidades: gerencia livros, usuários, empréstimos, envia notificações e calcula multas. Isso dificulta manutenção e testes.
Má prática: Cada classe deve ter apenas um motivo para mudar. Aqui, qualquer alteração nas regras de envio de notificação, por exemplo, exigiria alteração na mesma classe que controla empréstimos.


---

2. Violação do OCP (Open/Closed Principle)

Classe: GerenciadorBiblioteca
Métodos: RealizarEmprestimo, RealizarDevolucao
Explicação:
A lógica de notificação está embutida no código. Se quisermos alterar a forma de notificação (ex: adicionar push notifications), será necessário modificar esses métodos.
Má prática: O código deve ser aberto para extensão e fechado para modificação.


---

3. Violação do DIP (Dependency Inversion Principle)

Classe: GerenciadorBiblioteca
Métodos: EnviarEmail, EnviarSMS
Explicação:
A classe depende de implementações concretas de envio de e-mail e SMS. Deveria depender de abstrações (interfaces).
Má prática: Isso torna difícil a reutilização e a substituição por outras formas de notificação (ex: via app).


---

4. Violações de Clean Code - Nome de Classe e Métodos Confusos

Classe: GerenciadorBiblioteca
Método: Todos
Explicação:
O nome da classe e de seus métodos são genéricos e não seguem uma estrutura clara. Ex: AdicionarLivro poderia ser responsabilidade de um LivroService.
Má prática: Nomes devem expressar claramente a intenção e escopo.


---

5. Violações de SRP + Clean Code - Métodos muito longos

Método: RealizarEmprestimo, RealizarDevolucao
Explicação:
Esses métodos executam muitas ações (busca, verificação, alteração de estado, notificações).
Má prática: Métodos devem ser pequenos, coesos e fáceis de testar.
