// MODELOS (ENTIDADES)

    public class Livro
    {
        public string Titulo { get; set; }
        public string Autor { get; set; }
        public string ISBN { get; set; }
        public bool Disponivel { get; set; } = true;
    }

    public class Usuario
    {
        public string Nome { get; set; }
        public int ID { get; set; }
    }

    public class Emprestimo
    {
        public Livro Livro { get; set; }
        public Usuario Usuario { get; set; }
        public DateTime DataEmprestimo { get; set; }
        public DateTime DataDevolucaoPrevista { get; set; }
        public DateTime? DataDevolucaoEfetiva { get; set; }
    }

    
    // INTERFACES PARA NOTIFICAÇÕES (ISP, DIP)

    public interface IEmailService
    {
        void EnviarEmail(string destinatario, string assunto, string mensagem);
    }

    public interface ISmsService
    {
        void EnviarSms(string destinatario, string mensagem);
    }

    
    // IMPLEMENTAÇÕES DE SERVIÇOS DE NOTIFICAÇÃO

    public class EmailService : IEmailService
    {
        public void EnviarEmail(string destinatario, string assunto, string mensagem)
        {
            Console.WriteLine($"[EMAIL] Para: {destinatario} | Assunto: {assunto} | Mensagem: {mensagem}");
        }
    }

    public class SmsService : ISmsService
    {
        public void EnviarSms(string destinatario, string mensagem)
        {
            Console.WriteLine($"[SMS] Para: {destinatario} | Mensagem: {mensagem}");
        }
    }

  
    // SERVIÇO DE LIVRO (SRP)

    public class LivroService
    {
        private readonly List<Livro> livros = new();

        public void AdicionarLivro(string titulo, string autor, string isbn)
        {
            livros.Add(new Livro { Titulo = titulo, Autor = autor, ISBN = isbn });
            Console.WriteLine($"Livro cadastrado: {titulo}");
        }

        public Livro BuscarPorISBN(string isbn) => livros.FirstOrDefault(l => l.ISBN == isbn);
        public List<Livro> ListarTodos() => livros;
    }

 
    // SERVIÇO DE USUÁRIO (SRP + DIP)

    public class UsuarioService
    {
        private readonly List<Usuario> usuarios = new();
        private readonly IEmailService emailService;

        public UsuarioService(IEmailService emailService)
        {
            this.emailService = emailService;
        }

        public void AdicionarUsuario(string nome, int id)
        {
            var usuario = new Usuario { Nome = nome, ID = id };
            usuarios.Add(usuario);
            emailService.EnviarEmail(nome, "Bem-vindo", "Você foi cadastrado na biblioteca.");
            Console.WriteLine($"Usuário cadastrado: {nome}");
        }

        public Usuario BuscarPorId(int id) => usuarios.FirstOrDefault(u => u.ID == id);
        public List<Usuario> ListarTodos() => usuarios;
    }

    
    // SERVIÇO DE EMPRÉSTIMO (SRP + OCP + DIP)

    public class EmprestimoService
    {
        private readonly List<Emprestimo> emprestimos = new();
        private readonly IEmailService emailService;
        private readonly ISmsService smsService;

        public EmprestimoService(IEmailService emailService, ISmsService smsService)
        {
            this.emailService = emailService;
            this.smsService = smsService;
        }

        public bool RealizarEmprestimo(Usuario usuario, Livro livro, int diasEmprestimo)
        {
            if (!livro.Disponivel) return false;

            livro.Disponivel = false;

            var emprestimo = new Emprestimo
            {
                Usuario = usuario,
                Livro = livro,
                DataEmprestimo = DateTime.Now,
                DataDevolucaoPrevista = DateTime.Now.AddDays(diasEmprestimo)
            };

            emprestimos.Add(emprestimo);

            // Notificações
            emailService.EnviarEmail(usuario.Nome, "Empréstimo realizado", $"Você pegou o livro: {livro.Titulo}");
            smsService.EnviarSms(usuario.Nome, $"Livro emprestado: {livro.Titulo}");

            return true;
        }

        public double RealizarDevolucao(string isbn, int usuarioId)
        {
            var emprestimo = emprestimos.FirstOrDefault(e =>
                e.Livro.ISBN == isbn &&
                e.Usuario.ID == usuarioId &&
                e.DataDevolucaoEfetiva == null);

            if (emprestimo == null)
                return -1; // Código de erro

            emprestimo.DataDevolucaoEfetiva = DateTime.Now;
            emprestimo.Livro.Disponivel = true;

            double multa = 0;

            if (DateTime.Now > emprestimo.DataDevolucaoPrevista)
            {
                multa = (DateTime.Now - emprestimo.DataDevolucaoPrevista).Days * 1.0;
                emailService.EnviarEmail(emprestimo.Usuario.Nome, "Multa por atraso", $"Você deve R$ {multa:0.00}");
            }

            return multa;
        }

        public List<Emprestimo> ListarTodos() => emprestimos;
    }

   
    // CLASSE PRINCIPAL (UI / TESTE)

    class Program
    {
        static void Main(string[] args)
        {
            // Inversão de dependência via interfaces
            IEmailService emailService = new EmailService();
            ISmsService smsService = new SmsService();

            var livroService = new LivroService();
            var usuarioService = new UsuarioService(emailService);
            var emprestimoService = new EmprestimoService(emailService, smsService);

            // Cadastro de livros
            livroService.AdicionarLivro("Clean Code", "Robert C. Martin", "978-0132350884");
            livroService.AdicionarLivro("Design Patterns", "Erich Gamma", "978-0201633610");

            // Cadastro de usuários
            usuarioService.AdicionarUsuario("João Silva", 1);
            usuarioService.AdicionarUsuario("Maria Oliveira", 2);

            // Realizar empréstimo
            var livro = livroService.BuscarPorISBN("978-0132350884");
            var usuario = usuarioService.BuscarPorId(1);

            if (livro != null && usuario != null)
            {
                emprestimoService.RealizarEmprestimo(usuario, livro, 7);
            }

            // Simular devolução com atraso
            System.Threading.Thread.Sleep(1000); // Simulação de tempo
            double multa = emprestimoService.RealizarDevolucao("978-0132350884", 1);
            Console.WriteLine($"Multa gerada: R$ {multa:0.00}");

            Console.ReadLine();
        }
    }
}

