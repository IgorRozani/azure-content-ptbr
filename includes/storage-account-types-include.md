Existem dois tipos de contas de armazenamento:

### Contas de armazenamento de uso geral

Uma conta do armazenamento de uso geral fornece a você acesso aos recursos de Armazenamento do Azure como Tabelas, Filas, Arquivos, Blobs e discos de máquina virtual do Azure em uma única conta. Esse tipo de conta de armazenamento tem dois níveis de desempenho:

- Um nível de desempenho padrão que permite o armazenamento de Tabelas, Filas, Arquivos, discos de máquina virtual do Azure e Blobs.
- Um nível de desempenho premium que atualmente suporta apenas discos de máquina virtual do Azure. Confira [Armazenamento Premium: Armazenamento de Alto Desempenho para as Cargas de Trabalho da Máquina Virtual do Azure](../articles/storage/storage-premium-storage.md) para ter uma visão geral detalhada do Armazenamento Premium.

### Contas de armazenamento de Blobs

Uma conta de Armazenamento de Blobs é uma conta de armazenamento especializada para armazenar dados não estruturados como blobs (objetos) no Armazenamento do Azure. As contas de armazenamento de Blobs são semelhantes a contas de armazenamento de finalidade geral existentes e compartilham todos os excelentes recursos de durabilidade, disponibilidade, escalabilidade e desempenho que você usa atualmente, incluindo 100% de consistência de API para blobs de bloco e blobs de acréscimo. Contas de Armazenamento de Blobs expõem o atributo **Camada de Acesso** que pode ser especificado durante a criação da conta e modificado posteriormente, conforme o necessário. Há dois tipos de camadas de acesso que podem ser especificados com base em seu padrão de acesso a dados:

- Uma camada de acesso **Dinâmica**, que indica que os objetos na conta de armazenamento serão acessados com mais frequência. Isso permite que você armazene dados por um custo menor de acesso.
- Uma camada de acesso **Estática**, que indica que os objetos na conta de armazenamento serão acessados com menos frequência. Isso permite que você armazene dados por um custo menor de armazenamento de dados.

Para aplicativos que exigem apenas o armazenamento de blobs em bloco ou acréscimo, recomendamos o uso de contas de Armazenamento de Blobs. Confira [Armazenamento de Blobs do Azure: camadas Estática e Dinâmica](../articles/storage/storage-blob-storage-tiers.md) para saber mais.

Antes de criar uma conta de armazenamento, você deve ter uma assinatura do Azure, que é um plano que dá acesso a uma variedade de serviços do Azure. Você pode começar com o Azure com uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/). Quando decidir adquirir um plano de assinatura, você poderá escolher entre uma variedade de [opções de compra](https://azure.microsoft.com/pricing/purchase-options/). Se for um [assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), você obterá créditos mensais gratuitos que podem se usados com os serviços do Azure, incluindo o Armazenamento do Azure. Consulte [Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/) para obter informações sobre preço por volume.

Para saber como criar uma conta de armazenamento padrão, consulte [Criar uma conta de armazenamento](../articles/storage/storage-create-storage-account.md#create-a-storage-account) para obter mais detalhes. Você pode criar até 100 contas de armazenamento nomeadas exclusivamente com uma única assinatura. Consulte [Escalabilidade e Metas de Desempenho do Armazenamento do Azure](../articles/storage/storage-scalability-targets.md) para obter detalhes sobre os limites da conta de armazenamento.

<!---HONumber=AcomDC_0427_2016-->