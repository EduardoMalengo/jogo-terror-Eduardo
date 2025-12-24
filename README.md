# jogo-terror-Eduardo

using System; //<--

enum Nivel { Facil, Medio, Dificil }

class Program
{
    static void Main(string[] args)
    {
        Random rng = new Random();
        Nivel nivelAtual = Nivel.Facil;

        // Escolha do mapa
        Console.WriteLine("Escolha o mapa:");
        Console.WriteLine("1 - Caverna");
        Console.WriteLine("2 - Hotel");
        Console.WriteLine("3 - Mansão");

        string escolhaMapa = Console.ReadLine();
        string mapa;

        if (escolhaMapa == "1") mapa = "Caverna";
        else if (escolhaMapa == "2") mapa = "Hotel";
        else if (escolhaMapa == "3") mapa = "Mansão";
        else mapa = "Caverna"; // padrão

        Console.WriteLine($"\nVocê escolheu: {mapa}");
        Console.WriteLine("=== JOGO COMEÇOU ===");

        // Loop dos níveis
        while (true)
        {
            // Mostrar nível e cor
            string cor = "Verde";
            if (nivelAtual == Nivel.Medio) cor = "Laranja";
            else if (nivelAtual == Nivel.Dificil) cor = "Vermelho";

            Console.WriteLine($"\nNível: {nivelAtual} ({cor})");

            // Iluminação
            Console.WriteLine("Iluminação normal...");
            Console.WriteLine("Iluminação fraca...");

            // Monstro aparece?
            int chance = (nivelAtual == Nivel.Facil) ? 30 :
                         (nivelAtual == Nivel.Medio) ? 60 : 90;

            bool monstroApareceu = rng.Next(0, 100) < chance;

            if (monstroApareceu)
            {
                Console.WriteLine("👹 O monstro apareceu!");
                Console.WriteLine($"Escolha sua ação:");
                Console.WriteLine($"1 - Fugir para algum lugar da {mapa}");
                Console.WriteLine($"2 - Se esconder na {mapa}");

                string acao = Console.ReadLine();

                // Dificuldade de escapar
                int dificuldade = (nivelAtual == Nivel.Facil) ? 30 :
                                  (nivelAtual == Nivel.Medio) ? 50 : 70;

                bool sucesso = rng.Next(0, 100) > dificuldade;

                if (acao == "1")
                {
                    Console.WriteLine(sucesso
                        ? $"Você fugiu pela {mapa} e escapou!"
                        : $"Você tentou fugir pela {mapa}, mas o monstro te pegou!");
                    if (!sucesso) break;
                }
                else if (acao == "2")
                {
                    Console.WriteLine(sucesso
                        ? $"Você se escondeu bem na {mapa} e o monstro não te achou!"
                        : $"Você tentou se esconder na {mapa}, mas o monstro te encontrou!");
                    if (!sucesso) break;
                }
                else
                {
                    Console.WriteLine("Você hesitou... o monstro te pegou!");
                    break;
                }
            }
            else
            {
                Console.WriteLine("Nenhum monstro apareceu dessa vez...");
            }

            // Avançar nível
            if (nivelAtual == Nivel.Facil)
            {
                nivelAtual = Nivel.Medio;
                Console.WriteLine("Você avançou para o nível Médio!");
            }
            else if (nivelAtual == Nivel.Medio)
            {
                nivelAtual = Nivel.Dificil;
                Console.WriteLine("Você avançou para o nível Difícil!");
            }
            else
            {
                Console.WriteLine("\nParabéns! Você venceu todos os níveis!");
                break;
            }
        }

        Console.WriteLine("\n=== Fim do jogo ===");
    }
}

