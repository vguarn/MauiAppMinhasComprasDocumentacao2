// Classe que representa um Produto, com propriedades que definem as características do item na lista de compras.
public class Produto
{
    // Identificador único para cada produto (geralmente usado no banco de dados)
    public int Id { get; set; }

    // Nome do produto, como "Arroz", "Leite", etc.
    public string Nome { get; set; }

    // Quantidade do produto que o usuário deseja comprar.
    public int Quantidade { get; set; }

    // Preço do produto. Usado para calcular o valor total das compras.
    public decimal Preco { get; set; }
}


// Classe responsável pela interação com o banco de dados SQLite. Ela fornece métodos para salvar e recuperar produtos.
public class SQLiteDatabaseHelper
{
    private readonly SQLiteConnection _database;

    // Construtor que recebe o caminho para o banco de dados e cria a tabela de produtos, se necessário.
    public SQLiteDatabaseHelper(string dbPath)
    {
        _database = new SQLiteConnection(dbPath); // Cria a conexão com o banco de dados SQLite.
        _database.CreateTable<Produto>(); // Cria a tabela Produto caso não exista.
    }

    // Método para salvar um produto no banco de dados. Retorna o ID do produto inserido.
    public int SaveProduto(Produto produto)
    {
        return _database.Insert(produto); // Insere o produto na tabela.
    }

    // Método para recuperar todos os produtos armazenados no banco de dados.
    public List<Produto> GetAllProdutos()
    {
        return _database.Table<Produto>().ToList(); // Retorna todos os produtos da tabela.
    }
}


<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MauiAppMinhasCompras.App">
    <!-- Definição da página principal do aplicativo -->
    <Application.MainPage>
        <MainPage /> <!-- Página principal será a MainPage.xaml -->
    </Application.MainPage>
</Application>


<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MauiAppMinhasCompras.MainPage">

    <!-- Lista de produtos que será exibida ao usuário -->
    <ListView x:Name="ProdutoListView">
        <ListView.ItemTemplate>
            <DataTemplate>
                <!-- Exibição do nome do produto e sua quantidade -->
                <TextCell Text="{Binding Nome}" Detail="{Binding Quantidade}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    
    <!-- Botão para adicionar um novo produto à lista -->
    <Button Text="Adicionar Produto" Clicked="OnAdicionarProdutoClicked" />
</ContentPage>


public partial class MainPage : ContentPage
{
    // Instância do helper para interagir com o banco de dados SQLite.
    private readonly SQLiteDatabaseHelper _dbHelper;

    // Construtor da página. Inicializa os componentes e carrega os produtos.
    public MainPage()
    {
        InitializeComponent(); // Inicializa os componentes da página.
        
        // Caminho para o banco de dados (armazenado localmente no dispositivo).
        _dbHelper = new SQLiteDatabaseHelper(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "produtos.db3"));
        
        LoadProdutos(); // Carrega os produtos do banco de dados e exibe na lista.
    }

    // Método para carregar os produtos armazenados no banco de dados e exibí-los na ListView.
    private void LoadProdutos()
    {
        ProdutoListView.ItemsSource = _dbHelper.GetAllProdutos(); // Define a fonte de dados da ListView.
    }

    // Método chamado quando o botão "Adicionar Produto" é clicado.
    private void OnAdicionarProdutoClicked(object sender, EventArgs e)
    {
        // Cria um novo produto com dados fictícios (para fins de exemplo).
        var produto = new Produto
        {
            Nome = "Produto Exemplo",
            Quantidade = 1,
            Preco = 10.99M
        };
        
        // Salva o produto no banco de dados.
        _dbHelper.SaveProduto(produto);
        
        // Recarrega a lista de produtos para refletir a adição do novo item.
        LoadProdutos();
    }
}
# AppMinhasCompras

## Descrição

O **AppMinhasCompras** é um aplicativo simples de gerenciamento de lista de compras, desenvolvido com .NET MAUI e SQLite. Ele permite que o usuário adicione produtos à sua lista de compras, veja os itens armazenados e os armazene de forma persistente no banco de dados local.

## Funcionalidades

- Exibir uma lista de produtos adicionados.
- Adicionar novos produtos à lista.
- Armazenamento local de dados com SQLite.

## Como Executar

1. Clone o repositório para o seu computador.
2. Abra o projeto no Visual Studio.
3. Certifique-se de que o **.NET 6 SDK** e o **Visual Studio 2022** com suporte ao **.NET MAUI** estão instalados.
4. Execute o aplicativo em um emulador ou dispositivo real.

## Tecnologias

- **.NET MAUI**: Framework multiplataforma para criar aplicativos móveis e desktop.
- **SQLite**: Banco de dados local para armazenamento de dados.

## Licença

Este projeto está licenciado sob a **MIT License**. Consulte o arquivo LICENSE para mais detalhes.

