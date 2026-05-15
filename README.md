
# Objetivos

O reconhecimento passivo é um método de coleta de informações em que as ferramentas não interagem diretamente com o dispositivo de destino ou a rede. Neste laboratório, você explorará as ferramentas comuns usadas para reunir informações sobre um destino por meio do DNS (Domain Name System).

- Use **nslookup** para obter informações de domínio e endereço IP.
- Use o **comando whois** para encontrar informações de registro adicionais.
- Compare a saída das ferramentas nslookup e dig.
- Execute Consultas DNS reversas.

# Plano de fundo / Cenário

Antes de começar qualquer teste de penetração ou outro compromisso de hacking ético, você precisa obter discretamente o máximo de informações sobre a organização-alvo quanto possível. Há uma riqueza de informações que pode ser obtida a partir de dados de registro de domínio disponíveis publicamente. Neste laboratório, você investigará a saída dos comandos **nslookup**, **whois**, e **dig**.

# Recursos Obrigatórios

- VM Kali personalizada para o curso de Ethical Hacker
- com acesso à Internet

# Instruções

## Parte 1: Use nslookup para obter informações de domínio e endereço IP.

### Etapa 1: Iniciar sessão em Kali Linux e acessar o ambiente do terminal.

1. Iniciar sessão no sistema Kali com o nome de usuário **kali** e a senha **kali**_._ Você verá a área de trabalho do Kali Linux.
2. Abra uma janela do terminal clicando no ícone do **Terminal** localizado perto da parte superior da tela.

### Etapa 2: Investigar as capacidades do nslookup

Nslookup que é uma ferramenta de linha de comando disponível no Linux e no Windows. Seu uso básico é converter um nome de domínio em um endereço IP. O Nslookup possui outras funcionalidades que podem fornecer informações adicionais.

1. Acesse as páginas manuais para **nslookup** usando o comando **man**:

![[{61378D3A-5A4D-4123-9EA7-65CAF332B4AD}.png]]


Que **palavra-chave você usaria** para consultar o registro mx do servidor de email em um domínio?

R:   Definir querytype=mx ou definir tipo=mx


### Etapa 3: Usando o comando nslookup


1. Use o **comando nslookup** sem opções para inserir o modo interativo. Para sair do modo interativo a qualquer momento, digite **sair** para retornar ao prompt do CLI.


2. O prompt do CLI é alterado para > indicar que agora você está no modo interativo e pode inserir os diversos comandos nslookup. Digite o nome do domínio **cisco.com** para resolver o nome do domínio para um endereço IP. Por padrão, o comando nslookup consulta os registros A e AAAA para o destino.


![[{F478500A-0D9C-4A8F-8149-4B3C01BA5DE7}.png]]



3. Para localizar os servidores de nome do domínio configurados para cisco.com, use o comando **set type** para alterar o tipo de consulta para "ns" para retornar as informações do servidor de nome.


![[{218FFCC6-2D9C-4B3B-B0E3-B6CADF074519}.png]]


![[{5A9CF074-F5EC-4B8E-A2AD-7718791D4DD1}.png]]




Quais são os endereços IPv4 e IPv6 do servidor DNS primário (ns1)?

**Endereço IPv4:** `72.163.5.201`
 **Endereço IPv6:** `2001:420:1101:6::a`


### Etapa 4: Alterar o servidor usado para realizar buscas.

Ocasionalmente, é desejável usar outro servidor DNS para fazer buscas. Isso pode ser necessário se o servidor DNS local não puder resolver um endereço ou resolver o nome do host para um endereço privado interno e você precisar obter o endereço acessível à Internet do host.

1. Nesta consulta, use a sintaxe de comando de uma linha nslookup **para alterar o servidor para pesquisar netacad.com.**

No modo interativo, você altera o servidor usando a palavra-chave **server**.


2. O **_tipo de consulta any_** pode recuperar muitas ou todas as informações contidas no registro DNS para um nome de host. Geralmente, registros DNS contêm **_registros de texto_** que podem fornecer mais detalhes sobre o domínio. Usando o servidor 8.8.8.8 Google DNS, obter os registros DNS de netacad.com.

![[{A4FFEE7A-0969-435F-8D5F-C36D75BFEC11}.png]]


![[{C1969B2A-164D-4A3B-A6D6-68A68D426733}.png]]


Quê tipos de registro são exibidos na saída do comando nslookup com o tipo definido como "any"?

Todos os tipos de registros permitidos, incluindo A, AAAA, NS, MX e texto.


### Parte 2: Use a função Whois para obter informações de domínio

A ferramenta whois consulta informações de registro de domínio, em vez dos registros do servidor DNS. É outra forma de reconhecimento passivo que pode identificar onde o domínio está registrado, informações de contato técnico e administrativo e localizações físicas. Saiba que as informações contidas nos registros de domínio podem ser definidas como privadas e, muitas vezes, as informações de contato são as do serviço de hospedagem, e não da própria organização.

### Etapa 1: Comparar a saída de whois para várias organizações.

1. A ferramenta whois está disponível no prompt CLI de Kali Linux. Use o **comando whois** para obter informações sobre o cisco.com.

Que conclusão você pode chegar sobre os dois domínios (cisco.com e netacad.com) com base na saída dos **comandos whois** ?

Ambos domínios são propriedade da Cisco e hospedados na nuvem.


### Etapa 2: Use whois para determinar as informações de registro do endereço IP.

A ferramenta whois também pode ser usada para reunir informações sobre intervalos de endereços IP que são atribuídos a uma organização. Na parte anterior deste laboratório, descobrimos os endereços IP atribuídos a vários nomes de host do servidor DNS de domínio. Agora você pode usar essas informações de endereço para obter mais detalhes sobre os intervalos de endereços IP externos que são atribuídos a essas organizações.

1. Revise a saída que você obteve usando **nslookup** para obter os endereços IP dos servidores DNS para cisco.com. Grave os endereços IP dos servidores Cisco DNS.

2. Use a ferramenta Whois para encontrar quais intervalos de endereço IP são atribuídos à Cisco e são usados nas redes que hospedam seus servidores DNS. 


![[{434F086B-ECD0-4B8D-A1BB-924B9DC3DDD8}.png]]



Qual é o intervalo de endereços IP para os endereços IPv4 alocados para a Cisco? O servidor ns1.cisco.com é endereçado nesse bloco.

72.163.0.0/16

3. Como as organizações podem usar as mesmas redes IP para outros servidores voltados para externos, saber os intervalos de endereços é valioso para determinar quais redes atingir durante um teste de penetração. Use a ferramenta whois para obter as alocações de endereço IP para as redes IP onde estão localizados outros servidores Cisco DNS.


![[{7675F7B9-3025-4C2D-865B-8F020B011EA5}.png]]

![[{30CD9CFD-86A4-46CF-BF54-892619CD1BBA}.png]]


O Intervalo e 64.100.0.0/14, 64.104.0.0/16

## Parte 3: Compare a saída do Nslookup e Dig

**Etapa 1: Use o Dig do Linux para consultar os servidores DNS.**

1. Dig é uma função do Linux que executa consultas DNS. O formato de uma consulta Dig é semelhante ao de Nslookup. Para resolver o nome do host cisco.com para um endereço IP. 

![[{41385A30-A806-436E-B16C-0CDC2E7E59DC}.png]]


Qual é a diferença entre os tipos de registros padrão consultados pelo Dig e os consultados por Nslookup?

O dig  por padrão consulta somente registros tipo A e o nslookup  consulta tanto os registros A quantos os AAAA.



2. Para obter o endereço IPv6 de cisco.com é necessário adicionar um tipo à estrutura de comandos. A sintaxe para instruir o Dig a consultar um tipo de registro específico é dig **_[_nome do host_]_ _[_tipo de registro_]_**.


![[{3F2A15CD-5D8D-49F3-B7CF-2ABCB3A30CA6}.png]]



#  **Etapa 2: Usar Dig para obter informações adicionais.**

1. Na parte anterior deste laboratório, nslookup foi usado para obter os servidores DNS para cisco.com. Use o servidor 8.8.8.8 Google DNS para consultar os registros do servidor DNS. A sintaxe para usar um comando dig para realizar uma consulta usando um servidor DNS diferente é **dig** **[_hostname_] @[_DNS server IP_] [_tipo_]**. No prompt, insira **cisco.com 8.8.8.8 ns**.


![[{138E2919-83F9-4E6C-AFE8-4F786C47825B}.png]]


2. Anteriormente, nslookup foi usado com a opção **_set type=any_** para encontrar informações adicionais sobre o nome do host netacad.com. O tipo de registro **_any_** também pode ser consultado usando Dig.


 Como a consulta inicial ao gateway local resultou em falha devido a restrições do roteador com o parâmetro `ANY`, utilizei o comando `dig @8.8.8.8 netacad.com any`. O uso do caractere **`@`** permitiu **ignorar completamente as configurações de rede local e o roteador, enviando os pacotes diretamente para o Google Public DNS (`8.8.8.8`)**. Por ser um resolvedor recursivo global e robusto, o servidor do Google processou a requisição sem os bloqueios de segurança ou limitações de cache comuns em infraestruturas domésticas, retornando com sucesso todos os registros DNS do alvo.
 
![[{B48EED90-417B-4EA8-A2A2-3D28E56CD9BA}.png]]


![[{29D28BD7-56D9-4E68-BA41-D28FD7380BFB}.png]]


Compare a saída da função Dig com a saída de Nslookup para **_ANY_** tipo de registro. Qual saída é mais fácil de ler para obter os valores contidos nos diversos tipos de registros?

Cada tipo de registro fica em uma linha própria que fica melhor de visualizar.


# ### Parte 4: Executar buscas DNS reversas

**Etapa 1: Use o Dig para executar buscas rDNS**

Agora que você pode executar buscas DNS e usar Whois para determinar os intervalos de endereço IP, use Dig para encontrar nomes de host adicionais. As buscas de Reverse DNS (rDNS) usam o endereço IP para consultar os nomes de host dos serviços que resolvem para esse endereço.

1. Digite o **comando dig** usando a opção -x para recuperar o nome do host e tipo de registro do servidor ns1.cisco.com DNS (**72.163.5.201)**.

![[{E0361E32-ED46-446E-8380-2983E3CF32F2}.png]]


Que tipo de registro é retornado com o nome do host?

PTR que e responsável por fazer o dns reverso.


![[{E5211843-DD42-4981-B6C5-B6CC836311BE}.png]]


Examine a saída retornada do comando dig. Que tipo de dispositivo você acha que está atribuído o endereço 72.163.1.1?

o registro **PTR** revelou o seguinte nome de domínio (FQDN) na seção de resposta
hsrp-72-163-1-1.cisco.com Como o host se chama hsrp-72-163-1-1, provavelmente é o endereço padrão do gateway atribuído a uma configuração do roteador HSRP.


**Etapa 2: Usar o Utilitário Host para realizar buscas rDNS**

O Utilitário Host é uma função no Linux que realiza buscas para converter endereços IP em nomes de host. 

![[{729F5391-B942-4BD9-A7E5-E06D4BE9455D}.png]]



Host também pode ser usado para realizar uma pesquisa rápida de endereço IP para um nome de host conhecido.


Como a saída do comando host difere de dig ou nslookup ao consultar um endereço IP atribuído a um host conhecido ?

A saída do host contém apenas o endereço IP, não o servidor DNS ou outras informações.

### Etapa 3: Use nslookup para Executar buscas RDNs

Nslookup é usado principalmente para realizar buscas de endereço IP para nomes de host conhecidos. Ele também pode ser usado para realizar buscas rDNS para retornar um nome de host atribuído a um endereço IP conhecido.

Use Nslookup para encontrar nomes de host associados a um endereço IP.


![[{9BB300F8-21DD-4D16-9BC8-F7C34FCEECEC}.png]]

![[{F59DA4F4-9499-4CBB-9B46-DAEB92F5B447}.png]]




O reconhecimento passivo via DNS provou ser uma etapa fundamental na fase de _Footprinting_. Através de ferramentas simples e nativas do Linux, foi possível mapear servidores de e-mail, blocos inteiros de IP pertencentes à organização e até inferir topologias de rede interna e protocolos ativos (como o HSRP) a partir de convenções de nomenclatura em registros PTR, tudo isso sem gerar alertas nos sistemas de detecção de intrusão (IDS) do alvo.


