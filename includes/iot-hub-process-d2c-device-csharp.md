## Enviar mensagens interativas por meio do dispositivo simulado

Nessa seção, você modificará o aplicativo do dispositivo simulado que você criou no tutorial [Introdução ao Hub IoT] para enviar mensagens interativas do dispositivo para a nuvem ao Hub IoT.

1. No Visual Studio, no projeto **SimulatedDevice**, adicione o método a seguir à classe **Program**.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Thread.Sleep(10000);
      }
    }
    ```

    Esse método é muito semelhante ao método **SendDeviceToCloudMessagesAsync** no projeto **SimulatedDevice**. A única diferença é que agora você define a propriedade do sistema **MessageId** e uma propriedade de usuário chamada **messageType**. O código atribui um guid (identificador global exclusivo) para a propriedade **MessageId**, que o Barramento de Serviço pode usar para eliminar duplicações das mensagens recebidas. O exemplo usa a propriedade **messageType** para distinguir as mensagens interativas das de ponto de dados. Essas informações são transmitidas nas propriedades da mensagem pelo aplicativo, em vez de no corpo da mensagem, para que o processador de evento não precise desserializar a mensagem apenas para executar o roteamento.

    > [AZURE.NOTE] É importante criar o **MessageId**, usado para eliminar a duplicação das mensagens interativas, no código do dispositivo, já que a comunicação de rede intermitente (ou outras falhas) poderia resultar em várias retransmissões da mesma mensagem do dispositivo. Você também pode usar uma ID de mensagem semântica, como um hash dos campos de dados da mensagem relevante, no lugar de um guid.

2. Adicione o seguinte método no método **Main**, logo antes da linha `Console.ReadLine()`:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Para simplificar, esse tutorial não implementa nenhuma política de repetição. No código de produção, você deve implementar políticas de repetição, como uma retirada exponencial, como sugerido no artigo [Tratamento de falhas transitórias] do MSDN.

<!-- Links -->
[Tratamento de falhas transitórias]: https://msdn.microsoft.com/library/hh675232.aspx
[Introdução ao Hub IoT]: iot-hub-csharp-csharp-getstarted.md

<!---HONumber=AcomDC_0309_2016-->