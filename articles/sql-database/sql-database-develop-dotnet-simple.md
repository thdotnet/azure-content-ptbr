<properties
	pageTitle="Conectar-se ao Banco de Dados SQL usando .NET (C#)"
	description="Use o código de exemplo neste início rápido para criar um aplicativo moderno com C# e sustentado por um poderoso banco de dados relacional na nuvem com o Banco de Dados SQL do Azure."
	services="sql-database"
	documentationCenter=""
	authors="tobbox"
	manager="jeffreyg"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.workload="sql-database"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="03/16/2016"
	ms.author="tobiast"/>


# Usando o Banco de Dados SQL do .NET (C#)


[AZURE.INCLUDE [SQL-Database-develop-Includes-Selector-Language-Platform-Depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


## Etapa 1: configurar o Ambiente de Desenvolvimento

O .NET Framework é pré-instalado no Windows. Para Linux e Mac OS X, você pode baixar o .NET Framework a partir de [Projeto Mono](http://www.mono-project.com/).

## Etapa 2: Criar um banco de dados SQL

Consulte a [página de introdução](sql-database-get-started.md) para aprender a criar um banco de dados de exemplo. É importante que você siga o guia para criar um **modelo de banco de dados AdventureWorks**. Os exemplos mostrados abaixo funcionam apenas com o **esquema AdventureWorks**.

## Etapa 3: Obter a cadeia de conexão

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## Etapa 4: conectar

A [classe System.Data.SqlClient.SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) é usada para se conectar ao Banco de Dados SQL.


```
using System.Data.SqlClient;

class Sample
{
  static void Main()
  {
	  using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
	  {
		  conn.Open();
	  }
  }
}
```

## Etapa 5: executar uma consulta

É possível usar as classes [System.Data.SqlClient.SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx) e [SqlDataReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqldatareader.aspx) para recuperar um conjunto de resultados de uma consulta no Banco de Dados SQL. Observe que System.Data.SqlClient também oferece suporte à recuperação de dados em um [System.Data.DataSet](https://msdn.microsoft.com/library/system.data.dataset.aspx) offline.

```
using System;
using System.Data.SqlClient;

class Sample
{
	static void Main()
	{
	  using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
		{
			var cmd = conn.CreateCommand();
			cmd.CommandText = @"
					SELECT
						c.CustomerID
						,c.CompanyName
						,COUNT(soh.SalesOrderID) AS OrderCount
					FROM SalesLT.Customer AS c
					LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID
					GROUP BY c.CustomerID, c.CompanyName
					ORDER BY OrderCount DESC;";

			conn.Open();

			using(var reader = cmd.ExecuteReader())
			{
				while(reader.Read())
				{
					Console.WriteLine("ID: {0} Name: {1} Order Count: {2}", reader.GetInt32(0), reader.GetString(1), reader.GetInt32(2));
				}
			}					
		}
	}
}

```  

## Etapa 6: inserir uma linha

Nesse exemplo, você verá como executar uma instrução [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) com segurança, passar parâmetros que protegem seu aplicativo contra vulnerabilidade [a injeção de SQL](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) e recuperar o valor da [Chave Primária](https://msdn.microsoft.com/library/ms179610.aspx) gerado automaticamente.

```
using System;
using System.Data.SqlClient;

class Sample
{
    static void Main()
    {
		using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
        {
            var cmd = conn.CreateCommand();
            cmd.CommandText = @"
                INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)
                OUTPUT INSERTED.ProductID
                VALUES (@Name, @Number, @Cost, @Price, CURRENT_TIMESTAMP)";

            cmd.Parameters.AddWithValue("@Name", "SQL Server Express");
            cmd.Parameters.AddWithValue("@Number", "SQLEXPRESS1");
            cmd.Parameters.AddWithValue("@Cost", 0);
            cmd.Parameters.AddWithValue("@Price", 0);

            conn.Open();

            int insertedProductId = (int)cmd.ExecuteScalar();

            Console.WriteLine("Product ID {0} inserted.", insertedProductId);
        }
    }
}
```


## Próximas etapas

Saiba como usar a lógica de repetição manipulando códigos de erro transitórios para tornar seu código mais resiliente aqui: [Exemplo de código: lógica de repetição em C# para se conectar ao Banco de Dados SQL](sql-database-develop-csharp-retry-windows.md)

Saiba mais sobre possíveis códigos de erro aqui: [Códigos de erro de SQL para aplicativos cliente do Banco de Dados SQL: erro de conexão de banco de dados e outros problemas](sql-database-develop-error-messages.md)

<!-----------HONumber=AcomDC_0330_2016-->