# Programa-o-Orientada-a-Objeto
Realização de uma aplicação em C# com intuito de encaminhar  mensagens para diversos canais de comunicação.

using System;

public class Program
{
    public static void Main()
    {
        Console.WriteLine("Hello World");

        ICanal whatsAppProfessor = new WhatsApp();
        ICanal instaAluno = new Instagram();

        MensagemBasica mensagemTexto = new MensagemTexto("Olá");
        MensagemVideo mensagemVideo = new MensagemVideo("Olá - Mensagem de vídeo", "fim de semana.mp4", TiposDeArquivo.MP4, 60);

        // Enviando mensagem de texto
        whatsAppProfessor.EnviarMensagem("+5511964687373", mensagemTexto);

        // Enviando mensagem de vídeo
        instaAluno.EnviarMensagem("+5511964687373", mensagemVideo);
    }
}

public enum TiposDeArquivo
{
    MP3,
    MP4,
    JPG,
    PDF
}

public interface ICanal
{
    void EnviarMensagem(string destinatario, Mensagem mensagem);
}

public abstract class CanaisMensagem : ICanal
{
    protected abstract string Canal { get; }

    public void EnviarMensagem(string destinatario, Mensagem mensagem)
    {
        Console.WriteLine($"Mensagem enviada pelo canal: {Canal}");
        Console.WriteLine($"Destinatário: {destinatario}");
        Console.WriteLine($"Mensagem: {mensagem.Conteudo}");
        Console.WriteLine($"Data Envio: {mensagem.DataEnvio}");

        if (mensagem is MensagemMultimidia multimidia)
        {
            Console.WriteLine($"Arquivo: {multimidia.Arquivo}");
            Console.WriteLine($"Tipo Arquivo: {multimidia.Formato}");
        }

        if (mensagem is MensagemVideo video)
        {
            Console.WriteLine($"Duração: {video.Duracao}");
        }
    }
}

public class WhatsApp : CanaisMensagem
{
    protected override string Canal { get { return "WhatsApp"; } }
}

public class Telegram : CanaisMensagem
{
    protected override string Canal { get { return "Telegram"; } }
}

public class Instagram : CanaisMensagem
{
    protected override string Canal { get { return "Instagram"; } }
}

public class Facebook : CanaisMensagem
{
    protected override string Canal { get { return "Facebook"; } }
}

public class Mensagem
{
    public string Conteudo { get; set; }
    public DateTime DataEnvio { get; set; }

    public Mensagem(string conteudo)
    {
        Conteudo = conteudo;
        DataEnvio = DateTime.Now;
    }
}

public class MensagemTexto : Mensagem
{
    public MensagemTexto(string conteudo) : base(conteudo)
    {
    }
}

public class MensagemMultimidia : Mensagem
{
    public string Arquivo { get; set; }
    public TiposDeArquivo Formato { get; set; }

    public MensagemMultimidia(string conteudo, string arquivo, TiposDeArquivo formato) : base(conteudo)
    {
        Arquivo = arquivo;
        Formato = formato;
    }
}

public class MensagemVideo : MensagemMultimidia
{
    public int Duracao { get; set; }

    public MensagemVideo(string conteudo, string arquivo, TiposDeArquivo formato, int duracao) : base(conteudo, arquivo, formato)
    {
        Duracao = duracao;
    }
}
