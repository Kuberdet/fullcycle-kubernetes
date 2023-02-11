# K8S

## Iniciando com k8s

### Introudução ao k8s

K8s é um gerenciados e um orquestrador de containers.

Podmos pedir para o k8s orquestrar os nossos containeres docker -> peace of mind

K8s é um produto opensource utilizado para automat a implant, o dimensionamento e o gerenciamento de applicativos em container.

Conceitos extremamente importantes.

K8s veio do Google, do projeto interno chamado de Bord. A ideia principal eh rodar processos e jobs em uma quantidade gigantesca de clustes e maquinas. A melhoria fou um outro gerenciadr chamado de omega e evoului para o k8s. Por isso o GCP da google é muito estavel na orqustração de containeres.

Pontos importantes k8s. É disponiblizado atraves de um conjunto de APIs. Tudo o que fazemos no k8s é nan verdade uma chamada API através de endpoints que o server disponibliza pra a gente.

Na maioria das vezes, usamos o kubectl par comandar o k8s.

Quase tuudo no k8s é baseado em estados e baseado no estado desse objeto o k8s toma determinadas ações. Entao, vamos imaginar que tem,os um deployment. Entenda que o deployment é um objeto que o k8s consegue configurar diversos containeres para nós.

Entao esse deployment é um objeto que configuramos e entao mandamos esse objeto para o k8s, o k8s recebe esse objeto, le todo esse objeto, e oprovisiona para nós os containers que precisamos. Entao trabalhar com k8s na verdade é trabalhar com sete diversos tipos de objetos que o k8s trabalha.

O mais interessante é que com o k8s conseguimos trabalhar com projetos declarativos mais ou menos como trabalhamos com o compose do docker. Conseguimos criar um arquivo yaml, passar todas as especificações e rodar uma vez. Ou seja, o objeto vai ser criado. Caso queremos alterar, modificamos o arquivo, aplicamos o aruqivo e as alterações vao ser reaLIZADAS.

O k8s trabalha em clusters. O cluster é um conjunto de máquinas que juntas tem um grande poder computacional para realizar diversos tipos de tarefas.

No caso do k8s, vamos ter um nó que chamamos de master. E esse master é quem vai controilar todo o processo que os outros nós vao fazer. 

Entao, pensando nesses nós, o master vai ter alguns serviços que o k8s deixa ali disponibilizado.

Eles sao api server, o controlle manager, e o scheduller.

Tudo isso é em relação ao master.



Já o node que não é master possuem o kubelet e o kubeproxy. Entao eles fazem a comunicação com o master para receber os comandos e como eles vao conseguir fazer os acessos entre um no e outro atraves dos proxys.

Cada máquina possui uma quantidade de vCPU e memoria. Cada máquina é conhecida como um Node. 

No fim, o k8s soma todos esses recuros e ele sabe o quanto ele tem de espaço para provisionar os recusros para os serviços que ele vai rodar para a gente.

Os pods sao unidades que contem os containers provisionados. O Pod representa os processos rodando no cluster.

Toda vez que vemos um pod, tem um container. Geralmente um pod para cada container que está rodando a nossa aplicação.

Deplyment é um outro tipo de objeto qyuetem obj de provisonar os pods, mas para prov , precisa saber quantas replicas de cada pod ele vai provisionar. Istop é dado através de um ReplicaSet, que é onde falamos quantas replicas vamos querer de cada set de pods.



Se um pod de uma replicaset cair, o k8s vai reiniciar sozinho.

Os pods vao ocupar uma máquina de acordo com os recusros e os replicasets disponiveis. Caso nao exista recusro disponivel para os RS, ele vai ficar pendente até que esse recuso possa ser novamente disponibilizado.



### Instalando o kind

Vamos fazer o k8s localmente primeiro. A ferramenta que vamos utilizar é o kind.

Ela instala o k8s rodando container docker. Cada container será como uma máquina.

Há diversas formas de instalar, na verdade é um executável que colocamos no path, seja com golang, baixar um binário ou instalar from source.

O kind trabalha com o kubectl. O kubectl pode ser baixado através do apt get ou através de um binario tb,.

https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-source



O kind cria o cluster e o kubectl gerencia o cluster.



### Criando primeiro cluster com kind

```bash
❯ kind
kind creates and manages local Kubernetes clusters using Docker container 'nodes'

Usage:
  kind [command]

Available Commands:
  build       Build one of [node-image]
  completion  Output shell completion code for the specified shell (bash, zsh or fish)
  create      Creates one of [cluster]
  delete      Deletes one of [cluster]
  export      Exports one of [kubeconfig, logs]
  get         Gets one of [clusters, nodes, kubeconfig]
  help        Help about any command
  load        Loads images into nodes
  version     Prints the kind CLI version

Flags:
  -h, --help              help for kind
      --loglevel string   DEPRECATED: see -v instead
  -q, --quiet             silence all stderr output
  -v, --verbosity int32   info log verbosity, higher value produces more output
      --version           version for kind

Use "kind [command] --help" for more information about a command.

```

Os princiopais comandos sao o create e o delete e o get.

Algumas variações de quando formos instalar o kind. Por padrao, o kind vai instalar um node. Mas podemos querer um cluster com mais opções.

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Thanks for using kind! 😊

```



Dessa maneira, teremos apenas uma máquina. No nosso kind, agora se quisermos nos conectarmos com esse cluster criado, teremos que executar o comando do kubectl mostrado. Quando trabalhamos com k8s, o kubectl é um client para nos comunicarmos com o cluster.

Porém para ele se comunicar com o cluster existe um arquivo. uma pasta chamada .kube e dentro do arquivo config vao estar todas as credencias para podemros acessar um cluster. O mais interessante de tudo é que podemos ter esse config com diversos cluster.

Entao,podemos ter um cluster em cada nuve, digital, ocean, aws, azure. Para isso, precisamos mudar aoenas o contexto. Isto é, em qual cluster eu quero mexer agora. 

O kubectl faz exatamente isso. Se quisermos mexer agora com esse contezxto do kind, vemos conforme está escrito no comando:

```nashj
kubectl cluster-info --context kind-kind
```

Kind-kind vai ser o nome do nosso cluster aqui. Entao preciamos rodar o comando acima para fazer o kubectl obeter o contexto do cluster criado pelo kind kind.

```bash
❯ kubectl cluster-info --context kind-kind

Kubernetes control plane is running at https://127.0.0.1:35821
CoreDNS is running at https://127.0.0.1:35821/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```



Entao, agora temos o k8s control-plane rodando no endereço https://127.0.0.1:35821 e o kubedns aqui https://127.0.0.1:35821/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy.

Entao, agora temos um cluster k8s rodando com apenas um nó e temos o kubectl apontando para esse cara.



Como sabemos se está tudo ok?

A primeira coisa, é verificar se o docker está rodando um container kind

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED       STATUS       PORTS                       NAMES
2aa7cd315721   kindest/node:v1.24.0   "/usr/local/bin/entr…"   2 hours ago   Up 2 hours   127.0.0.1:35821->6443/tcp   kind-control-plane
```



Que legal! Temos o kind funcionando no doker!

Mas a melhor forma de vermos nesse momento é com o kubectl

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get nodes
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   93m   v1.24.0

```

OLha só que interessante. Esse comando vai nos trazer todos os nodes do nosso cluester k8s. E está Ready e pronto para ser utilizado. Isso é um ponto extremamente importante!



Na proxima aula vamos apagar e criar um utro camarada para criar 



### Criando cluster multi node

Agora, vamos fazer o mesmo processo que fizemos até o momento mas adicionar um novo node.

Precisamos deletar o que acabamos de fazer. COm o comando abaixo, temos tdos os clusters criados ao utilizarmos o kind:

```bash
 kind get clusters
kind
```



Aqui temos apenas um cluster chamado kind pois quando criamos um cluster com o kind create cluster o nome padrão é kind.

entao vamos deletá-lo:

```bash
❯ kind delete clusters kind
Deleted clusters: ["kind"]

```

Entao agora, vamos ver se temos algum cluster e algum container rodando. 

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kind get clusters
No kind clusters found.
rogerio in FullCycle/3.0/4.K8s 
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
rogerio in FullCycle/3.0/4.K8s 

```



Tudo zerado. Tudo ok!

Agora vamos adicionar mais nós no nosso cluster definindo um arquivo de como ele será criado.

https://kind.sigs.k8s.io/docs/user/configuration/

kind.yaml

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker

```

Entao vamos no terminal, no lugar da pasta e criar o cluster com o arquivo de configuração. Agora aparecem varias caixinhas de preparação dos nos. 4 ao total! Quando formos trabalhar com o k8s em produção, vamos ter que ter no minimo 3 nodes e no minimo muilti masters.

````bash
rogerio in FullCycle/3.0/4.K8s 
❯ kind create cluster --config=k8s/kind.yaml --name=fullc
ycle
Creating cluster "fullcycle" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-fullcycle"
You can now use your cluster with:

kubectl cluster-info --context kind-fullcycle

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
````



Vamos selecionar e colocar para rodar de acordo com a linha de comando indicada:

```bash
rogerio in FullCycle/3.0/4.K8s took 52s 
❯ kubectl cluster-info --context kind-fullcycle
Kubernetes control plane is running at https://127.0.0.1:34703
CoreDNS is running at https://127.0.0.1:34703/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'
```

Vamos dar o docker ps e verificamos entao que temos quato containers rodando!

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                       NAMES
be2eb25baa9d   kindest/node:v1.24.0   "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                               fullcycle-worker3
47f9fdb5c95f   kindest/node:v1.24.0   "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                               fullcycle-worker
971f5218b827   kindest/node:v1.24.0   "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes   127.0.0.1:34703->6443/tcp   fullcycle-control-plane
c887902999f8   kindest/node:v1.24.0   "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                               fullcycle-worker2
```



e entao vamos pegar o kubectl get nodes!

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get nodes
NAME                      STATUS   ROLES           AGE     VERSION
fullcycle-control-plane   Ready    control-plane   3m59s   v1.24.0
fullcycle-worker          Ready    <none>          3m37s   v1.24.0
fullcycle-worker2         Ready    <none>          3m37s   v1.24.0
fullcycle-worker3         Ready    <none>          3m37s   v1.24.0
```

Vemos aqui que tb temos 4 nodes rodando no k8s, todos ready, mas um deles é o master.



### Mudança de contexto e a extensão do VSCode



Eventualmente podemos ter diversos clusteres em nossa máquina é pode ser um pouco chato sabermos quais temos disponiveis ou qual o contexto vamos utilizar. Mesmo que essas configurações estao no kubeconfig. Existem algumas ferramentas que podem nos ajudar.

Com o kubectl, temos uma opção chamada de config.

```rogerio in FullCycle/3.0/4.K8s 
❯ kubectl config
Modify kubeconfig files using subcommands like "kubectl config set current-context my-context"

 The loading order follows these rules:

  1.  If the --kubeconfig flag is set, then only that file is loaded. The flag may only be set once and no merging takes place.
  2.  If $KUBECONFIG environment variable is set, then it is used as a list of paths (normal path delimiting rules for your system). These paths are merged. When a value is modified, it is modified in the file that defines the stanza. When a value is created, it is created in the first file that exists. If no files in the chain exist, then it creates the last file in the list.
  3.  Otherwise, ${HOME}/.kube/config is used and no merging takes place.

Available Commands:
  current-context   Display the current-context
  delete-cluster    Delete the specified cluster from the kubeconfig
  delete-context    Delete the specified context from the kubeconfig
  delete-user       Delete the specified user from the kubeconfig
  get-clusters      Display clusters defined in the kubeconfig
  get-contexts      Describe one or many contexts
  get-users         Display users defined in the kubeconfig
  rename-context    Rename a context from the kubeconfig file
  set               Set an individual value in a kubeconfig file
  set-cluster       Set a cluster entry in kubeconfig
  set-context       Set a context entry in kubeconfig
  set-credentials   Set a user entry in kubeconfig
  unset             Unset an individual value in a kubeconfig file
  use-context       Set the current-context in a kubeconfig file
  view              Display merged kubeconfig settings or a specified kubeconfig file

Usage:
  kubectl config SUBCOMMAND [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```



A partir daí, podemos listar os nossos arquivos de configuração de contexxto dos clusteres:

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl config get-clusters
NAME
kind-fullcycle
```

Entao, vemos que temos 3 clusters configurados no nosso kubeconfig.

Nesse caso, temos apenas um cluster. Mas poderiamos mudar o context do kubectl para qualquer um dos clusters que aparecerem na lista!

```
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl config get-clusters
NAME
kind-fullcycle
```

E entao com o comando de get nodes saberiamos como está os nosso cluster.

Com esses comandos, conseguimos listar os clusters e de qual cluster vamos utilizar o contexto ao que o kubectl vaai trabalhar.

POde nao ser tao prático fazer isso todas as vezes. Mas podemos utilizar uma extensao Kubernetes do VSCode para nos ajudar.

ssa exetnsao vai nos ajudar mostrando quais sao os clusters que temos nesse momento e quais temos ativo ao contexto do kubectl.

Se quisermos navegar opara um contexto, basta apenas clicar com o botao direito e escolher Set to current cluster. iSSO É REALMENTE BEM PRÁTICO!



e ENTAO AS CONFIGURAÇÕES DOS CLUSTERS VAO ESTAR DISPONIVEIS PARA ADMINISTRARMOS 



## Primeiros passos na pratica

### Criando aplicação exemplo e imagem

Vamos criar uma aplicação muito basica em golang para termos um servidor golang e o k8s subir.

Agora vamos criar um Dockerfile.

Vamos buildar a imagem com o Dockerfile

```bash
docker build -t rogeriocassares/hello-go .
```

E vamos rodar a imagem

```bash
docker run --rm -p 80:8080 rogeriocassares/hello-go
```



http://localhost:80

Funcionando!1

Vamos fazer um push para o registry do DOcker HUb!

```bash
docker push rogeriocassares/hello-go
```



Pronto! Agora temos uma imagem Docer pronta e vamos querer colocá-la para funcionar no k8s!



### Trabalhando com Pods!

O POd é o menor objeto do k8s. e dentro do pod vamos ter um container rodando. O pod é como uma máquina que tem um IP, etc. O pod é como se fosse uma máquina e o container esta rodado dentro dessa máquina.

Vamos criar o nosso primeiro Pod usando o k8s

O mais comum e recomendável é usarmos aqruvisoa .yaml

vamos criar o pod.yaml

O k8s tem uma api e toda vez que vamos fazer qualquer coisa no k8s é feito através da api e ele gerencia os schedulers e etc.

Eventualmemte, cada tipo de aruivo pode ter uma versao diferente dessa api

A label é uma etiqueta que nos ajudam a fazer filtros, buscas, criar regras, por exemplo.

Se quisermos filtrar pela label "app go server" podemos fazer isso!

```ỳaml
apiVersion: v1
kind: Pod
metadata:
  name: "goserver"
  labels:
    app: "goserver"
spec:
  containers:
    - name: "goserver"
      image: "rogeriocassares/hello-go:latest"
      
```



Agoira vamos dar o comando que vamos mais utilizar no k8s

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl apply -f k8s/pod.yaml 
pod/goserver created
```

Nesse momento, criamos o nosso primeiro pod aplicando o nosso arquivo de configuração.

Entao, podemos agora ver o comando kubectl get pod ou kubectl get po

```bash
❯ kubectl get po
NAME       READY   STATUS    RESTARTS   AGE
goserver   1/1     Running   0          17h
```



Apesar de o Pod estar rodando no k8s, ainda nao criamos nenhum mecanismo de rede para acessar esse container dentro do pod.

Entao:

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl port-forward pod/goserver 8000:8080
```



Isso é uma maneira apenas para vermos que o pod está funcionando. Criamos um redirecionamento de porta para que mesmo que mesmo que nao tivessemos criado nenhum a regra nem configuração, vimos que é possivel fazer essa configuração.



Provavelemnte aa porta 80 é necessário de uma permissap sudo.



Vemos que o pod está online com o comando 

```bash
❯ kubectl get po
NAME       READY   STATUS    RESTARTS   AGE
goserver   1/1     Running   0          18h
```



E podemos deletar o Pod com o comando 

```
❯ kubectl delete pod goserver
pod "goserver" deleted
```

E vemos que nao temos mais nenhum pod:

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get po             
No resources found in default namespace.
```

Entao apagamos o pod, e ele nao está sendo criado novamente. É muito arriscado criarmos apenas um Pod e deixarmos rodando pois ele nao vai reiniciar automaticamente.



### Criando o primeiro ReplicaSet

A grabde vantagem do k8s é gerenciar e garantir a disponibilidade e criar varios pods para escalarmos a aplicação. QUantyo mais Pods, mais dividio o trafego e maior robustez.

Para recriar Pods automaticamente, existe o RS. O RS é um objeto que gerencia os Pods com os seguintes containers. Podemos faklar quantas regras de um determinado Pod quisermos. Se em algum momento pod for removido ou cair, ele recira e mantem sempre o numero do RS configurado:

replicaset.yaml

Quando quisermos colocar uma label, podemos selecionar somente as labels que tem certa spec. Entao, a propriedade selector server para o filtro das labels para selecionar a aplicação e encaminahr trafego para eles através de services.

Após a proioruedade replicas, vamos colocar como um template para o Pod que queremos criar!



AGora vamos aplicar esse aqruivo:

```bash
❯ kubectl apply -f k8s/replicaset.yaml 
replicaset.apps/goserver created
```



E ele criou o nosso replicaset

```bash
❯ kubectl get po
NAME             READY   STATUS              RESTARTS   AGE
goserver-fbgq7   0/1     ContainerCreating   0          36s
goserver-h7bpl   0/1     ContainerCreating   0          36s
```



E se repararmos no nome:

```bash
❯ kubectl get replicaset
NAME       DESIRED   CURRENT   READY   AGE
goserver   3         2         0       21h
```



Vemos que o replicaset tem o nome de goserver e o pod tem o nome goserver-NOME_ALEATORIO_DO_POD!



A replicaset nao apenas cria, mas gerencia o pod! Vamos deletar um pod:

```bash
❯ kubectl get replicaset           
NAME       DESIRED   CURRENT   READY   AGE
goserver   3         2         0       21h
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get po                   
NAME             READY   STATUS    RESTARTS      AGE
goserver-h7bpl   1/1     Running   1 (45m ago)   21h
```

Ao aumentarmos ou dimunirmos o numero de replicaset e aplicarmos novamente o replicaset, as coisas sao atualizadas automaticamente!



### O problema do ReplicaSet

Se formos em nosso arquivo server.go e modificarmos o código fonte, dermos um docker built para a imagem mudando a tag para :v2, por exemplo, geramos uma nova imagem da nossa aplicação.

E entao damos um push dessa nova versao para o docker hub.

Agora, o que deveriamos fazer, seria mudar  o arquivo do replicaset para a imagem v2

Aplicamos o arquivo do replicaset e configuramos o RS com a versao nova da nossa aplicação.

Se formos verificar os Pods, vemos que os Pods com a versao 1 ainda estao rodando! Mesmo que o RS esteja já com a versao mais rtecente

Podemos observar isso verificando a descrição do pod:

```bash
kubectl descripe pod
```

Vimnos que a imagem ainda esta antiga!

PS: NESSES PONTOS PARECE QUE O KIND NÃO ATUALIZA ATÉ QUE O SISTEMA SEJA REINIADO. CONFORME ORIENTADO, VAMOS INSTALAR O K3D, Criar o cluster e mudar o contexto

```BASH
❯ wget -q -O - https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash 

❯ k3d cluster create fullcycle                   
INFO[0000] Prep: Network                                
INFO[0000] Created network 'k3d-fullcycle'              
INFO[0000] Created image volume k3d-fullcycle-images    
INFO[0000] Starting new tools node...                   
INFO[0000] Starting Node 'k3d-fullcycle-tools'          
INFO[0001] Creating node 'k3d-fullcycle-server-0'       
INFO[0001] Creating LoadBalancer 'k3d-fullcycle-serverlb' 
INFO[0001] Using the k3d-tools node to gather environment information 
INFO[0001] HostIP: using network gateway 192.168.80.1 address 
INFO[0001] Starting cluster 'fullcycle'                 
INFO[0001] Starting servers...                          
INFO[0001] Starting Node 'k3d-fullcycle-server-0'       
INFO[0007] All agents already running.                  
INFO[0007] Starting helpers...                          
INFO[0007] Starting Node 'k3d-fullcycle-serverlb'       
INFO[0014] Injecting records for hostAliases (incl. host.k3d.internal) and for 2 network members into CoreDNS configmap... 
INFO[0017] Cluster 'fullcycle' created successfully!    
INFO[0017] You can now use it like this:                
kubectl cluster-info


❯ kubectl cluster-info --context k3d-fullcycle

Kubernetes control plane is running at https://0.0.0.0:38891
CoreDNS is running at https://0.0.0.0:38891/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:38891/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```



Vamos verificar o contexto e na extennsao dok8s para o vscode vemos que ele está como default.



Vamos aplicar o arquivo replicaset com 10 replicas

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get po                       
No resources found in default namespace.
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl apply -f k8s/replicaset.yaml 
replicaset.apps/goserver created
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get po                      
NAME             READY   STATUS    RESTARTS   AGE
goserver-5n5fb   0/1     Pending   0          2s
goserver-dj8gr   0/1     Pending   0          2s
goserver-bv28s   0/1     Pending   0          2s
goserver-jgjh7   0/1     Pending   0          2s
goserver-p8w7w   0/1     Pending   0          2s
goserver-wlg7l   0/1     Pending   0          2s
goserver-hs88f   0/1     Pending   0          2s
goserver-bqzvw   0/1     Pending   0          2s
goserver-bdhdb   0/1     Pending   0          2s
goserver-nz6tp   0/1     Pending   0          2s
```



Funcionou!

Vamos alterar o código fonte, subir a imagem e alterar o replicaset.yaml e descrever o Pod:

server.go

```go
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8080", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("<h1>Hello Fullcycle!!!</h1>"))
}

```

buildar e push na imagem:

````bash
❯ docker build -t rogeriocassares/hello-go:v2 .

❯ docker push rogeriocassares/hello-go:v2 
````



Foi! Algora no arquivo k8s/replicaset.yaml vamos mudar de latest para v2

```ỳaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 10
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v2"

```



Podemos verificar os nossos pods:

```nash
❯ kubectl get po
NAME             READY   STATUS    RESTARTS   AGE
goserver-5n5fb   0/1     Pending   0          7m
goserver-dj8gr   0/1     Pending   0          7m
goserver-bv28s   0/1     Pending   0          7m
goserver-jgjh7   0/1     Pending   0          7m
goserver-p8w7w   0/1     Pending   0          7m
goserver-wlg7l   0/1     Pending   0          7m
goserver-hs88f   0/1     Pending   0          7m
goserver-bqzvw   0/1     Pending   0          7m
goserver-bdhdb   0/1     Pending   0          7m
goserver-nz6tp   0/1     Pending   0          7m
```





Vamos aplciar as alterações com a versao nova da imagem!

```bash
❯ kubectl apply -f k8s/replicaset.yaml 
```



Podemos verificar os nossos pods:

```nash
❯ kubectl get po
NAME             READY   STATUS    RESTARTS   AGE
goserver-5n5fb   0/1     Pending   0          7m
goserver-dj8gr   0/1     Pending   0          7m
goserver-bv28s   0/1     Pending   0          7m
goserver-jgjh7   0/1     Pending   0          7m
goserver-p8w7w   0/1     Pending   0          7m
goserver-wlg7l   0/1     Pending   0          7m
goserver-hs88f   0/1     Pending   0          7m
goserver-bqzvw   0/1     Pending   0          7m
goserver-bdhdb   0/1     Pending   0          7m
goserver-nz6tp   0/1     Pending   0          7m
```

Vimos que não houve nenhuma mudança nos nomses dos Pods! Eles nao foram trocados mesmo que o RS esteja com uma versao mais recente:



Vamos descrever um dos pods!

```bash
❯ kubectl describe po goserver-5n5fb

Containers:
  goserver:
    Image:        rogeriocassares/hello-go:latest
```

Constatamos que a imagem é a latest ainda e nao é a v2!

Vamos deletar esse pod 

```bash
❯ kubectl delete po goserver-5n5fb  
pod "goserver-5n5fb" deleted
```



e entao vamos dar um get pods!

```bash
❯ kubectl get po
NAME             READY   STATUS    RESTARTS   AGE
goserver-dj8gr   0/1     Pending   0          12m
goserver-bv28s   0/1     Pending   0          12m
goserver-jgjh7   0/1     Pending   0          12m
goserver-p8w7w   0/1     Pending   0          12m
goserver-wlg7l   0/1     Pending   0          12m
goserver-hs88f   0/1     Pending   0          12m
goserver-bqzvw   0/1     Pending   0          12m
goserver-bdhdb   0/1     Pending   0          12m
goserver-nz6tp   0/1     Pending   0          12m
goserver-rcg4z   0/1     Pending   0          28s
```

E vemos que um deles acabou de ser criado!

Vamos descreve-lo



```bash
❯ kubectl describe po goserver-rcg4z 

Containers:
  goserver:
    Image:        rogeriocassares/hello-go:v2
```

E pudemos entao observar que o último Pod foi criado com a nova imagem v2!

Assim, concluimos que um RS com muitos pods, ao trocar a imagem no replicaset.yaml, a unica forma é matando cada um dos pods para que o RS gera cada um dos Pods com as novas versoes!

Isto é, o RS não altera a versao dos Pods automaticamente ao menos que os Pods sejam deletados. Para isso, usaremos o deployment!

PS: CASO OS PODS NAO SAIAM DO STATUS PENDING, PODE ESTAR TENDO ALGUM TIPO DE CONFLITO.

VAMOS REMOVER TODAS AS IMAGENS E CONTAINERS, REMOVER O CLUSTER KIND E DEIXAR TUDO ZERADO.

```bash
❯ kubectl config delete-cluster kind-fullcycle
deleted cluster kind-fullcycle from /home/rogerio/.kube/config
rogerio in FullCycle/3.0/4.K8s 

❯ kubectl config get-clusters                 
NAME
k3d-fullcycle

❯ docker rm $(docker ps -a -q) -f

❯ docker rmi $(docker images -q) -f

❯ sudo docker system prune -af

❯ sudo docker volume prune -f
```



E criamos novamente o cluster com o k3d:

```bash
rogerio in FullCycle/3.0/4.K8s 
❯ docker image ls -a
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
rogerio in FullCycle/3.0/4.K8s 
❯ k3d cluster create fullcycle
INFO[0000] Prep: Network                                
INFO[0000] Created network 'k3d-fullcycle'              
INFO[0000] Created image volume k3d-fullcycle-images    
INFO[0000] Starting new tools node...                   
INFO[0001] Pulling image 'ghcr.io/k3d-io/k3d-tools:5.4.4' 
INFO[0001] Creating node 'k3d-fullcycle-server-0'       
INFO[0006] Pulling image 'docker.io/rancher/k3s:v1.23.8-k3s1' 
INFO[0012] Starting Node 'k3d-fullcycle-tools'          
INFO[0090] Creating LoadBalancer 'k3d-fullcycle-serverlb' 
INFO[0091] Pulling image 'ghcr.io/k3d-io/k3d-proxy:5.4.4' 
INFO[0107] Using the k3d-tools node to gather environment information 
INFO[0107] HostIP: using network gateway 192.168.96.1 address 
INFO[0107] Starting cluster 'fullcycle'                 
INFO[0107] Starting servers...                          
INFO[0107] Starting Node 'k3d-fullcycle-server-0'       
INFO[0112] All agents already running.                  
INFO[0112] Starting helpers...                          
INFO[0112] Starting Node 'k3d-fullcycle-serverlb'       
INFO[0119] Injecting records for hostAliases (incl. host.k3d.internal) and for 2 network members into CoreDNS configmap... 
INFO[0121] Cluster 'fullcycle' created successfully!    
INFO[0121] You can now use it like this:                
rogerio in FullCycle/3.0/4.K8s took 2m 2s 
❯ kubectl apply -f k8s/replicaset.yaml 
replicaset.apps/goserver created

assares/hello-go:v2"
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl describe po
Name:         goserver-bwns6
Namespace:    default
Priority:     0
Node:         k3d-fullcycle-server-0/192.168.96.2
Start Time:   Thu, 21 Jul 2022 11:03:14 -0300
Labels:       app=goserver
Annotations:  <none>
Status:       Running
IP:           10.42.0.7
IPs:
  IP:           10.42.0.7
Controlled By:  ReplicaSet/goserver
Containers:
  goserver:
    Container ID:   containerd://760e818607ce4029ef7f5a772b014bed80cd29eb9d1898d833a2f67e03ba0f27
    Image:          rogeriocassares/hello-go:v2
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:2fdeab95d81a92603b44a7ee8e3236495db06b07606a4cd7abeddb8ad4030fb6
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 21 Jul 2022 11:11:25 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-b868w (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-b868w:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  10m   default-scheduler  Successfully assigned default/goserver-bwns6 to k3d-fullcycle-server-0
  Normal  Pulling    10m   kubelet            Pulling image "rogeriocassares/hello-go:v2"
  Normal  Pulled     116s  kubelet            Successfully pulled image "rogeriocassares/hello-go:v2" in 8m9.45015331s
  Normal  Created    116s  kubelet            Created container goserver
  Normal  Started    116s  kubelet            Started container goserver
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get po     
NAME             READY   STATUS    RESTARTS   AGE
goserver-bwns6   1/1     Running   0  

```



### Implementando Deployment

Vimos que o RS tem alguns limites de cria;'ao de novos Pods

O k8s tem um objeto que resolve tudo isso e chamamos de deployment

Deployment -. ReplicaSet -> Pod

O deployment cria o RS que gerencia os Pods.

Portanto, ao ser aplicada uma nova configuração no deployment, o deployment vai criar um outro ReplicaSet, vai matar todas as réplicas anteriores, e vai subr as replicas novas automaticamente.

Vamos criar o aqruivo de deployment duplicando o replicaset e mudando apenas o tipo (kind) para Deployment ao invés de ReplicaSet

deployment.yaml

``````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v2"

``````

Agora vamos ver os Replicasets que estao sendo utilizados e deleta-los:

````bash
❯ kubectl get replicaset
NAME       DESIRED   CURRENT   READY   AGE
goserver   1         1         1       40m
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl delete replicaset goserver
replicaset.apps "goserver" deleted
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get replicaset            
No resources found in default namespace.
❯ kubectl get po        
No resources found in default namespace.
````



Pronto! Nao temos mais nenhum pod rodando.

Vamos alterar de v2 para latest em deployment.yaml

E agora vamos aplicar o aquivo de deployment!

``````bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver created
``````



Vamos ver!

`````bàsh
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-6675499669-jln2q   1/1     Running   0          52s
goserver-6675499669-swxbq   1/1     Running   0          8s
goserver-6675499669-r6hhb   1/1     Running   0          8s
goserver-6675499669-mvfg7   1/1     Running   0          8s
goserver-6675499669-z2dl2   1/1     Running   0          8s
goserver-6675499669-46ld6   1/1     Running   0          8s
goserver-6675499669-lpkl9   1/1     Running   0          8s
goserver-6675499669-h6j5r   1/1     Running   0          8s
goserver-6675499669-t67j5   1/1     Running   0          8s
goserver-6675499669-jkp74   1/1     Running   0          8s
`````

Pronto!



ASgora podemos perceber que o nome de nossos pods estão ainda um pouco diferente:

goserver-6675499669-jln2q

O que podemos considerar como 

NOME_DO_DEPLOYMENT-NOME_DA_RS-NOME_DO_POD



Podemos verificar o nome da RS em:

``````bash
❯ kubectl get replicaset
NAME                  DESIRED   CURRENT   READY   AGE
goserver-6675499669   10        10        10      2m52s
``````



fICOU CLARO ENTAO QUE dEPLOYMENT -> rs -> pODS



Vamos agora, com o Deployment e alterar de latest para v2 e reaplicar o aquivo de configuração e verificar como estao os pods!

````bash
❯ kubectl get po                      
NAME                        READY   STATUS              RESTARTS   AGE
goserver-6675499669-jln2q   1/1     Running             0          5m59s
goserver-6675499669-swxbq   1/1     Running             0          5m15s
goserver-6675499669-r6hhb   1/1     Running             0          5m15s
goserver-6675499669-z2dl2   1/1     Running             0          5m15s
goserver-6675499669-46ld6   1/1     Running             0          5m15s
goserver-6675499669-lpkl9   1/1     Running             0          5m15s
goserver-6675499669-h6j5r   1/1     Running             0          5m15s
goserver-6675499669-t67j5   1/1     Running             0          5m15s
goserver-6675499669-mvfg7   1/1     Terminating         0          5m15s
goserver-54dd6d8758-hskhw   0/1     ContainerCreating   0          2s
goserver-54dd6d8758-6x7lz   0/1     ContainerCreating   0          2s
goserver-54dd6d8758-5nw46   0/1     ContainerCreating   0          2s
goserver-54dd6d8758-4cv8p   0/1     ContainerCreating   0          2s
goserver-54dd6d8758-9whzk   0/1     ContainerCreating   0          2s
````



Olha que legal! Podemos ver que foi gerado novos Pods com um Novo replicaSet e os antigos pods e replica set estão sendo removidos!

Dessa forma, por padrão, acabamos tendo zero downtime porque por padarao ele vai criando os novos e terminando os antigos conforme eles vao subindo! Ao subirem os novos Pods, ele vai fazendo uma troca progressiva para nao ficar nenhum tempo fora do ar!



Vamos verificar!

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-54dd6d8758-5nw46   1/1     Running   0          3m
goserver-54dd6d8758-hskhw   1/1     Running   0          3m
goserver-54dd6d8758-6x7lz   1/1     Running   0          3m
goserver-54dd6d8758-9whzk   1/1     Running   0          3m
goserver-54dd6d8758-4cv8p   1/1     Running   0          3m
goserver-54dd6d8758-vls8d   1/1     Running   0          2m57s
goserver-54dd6d8758-9x6jh   1/1     Running   0          2m56s
goserver-54dd6d8758-t99v6   1/1     Running   0          2m55s
goserver-54dd6d8758-zmpr2   1/1     Running   0          2m53s
goserver-54dd6d8758-p2699   1/1     Running   0          2m53s
````



E vamos descrever para ver se realmente a nova imagem foi aplicada!

````bash
❯ kubectl describe pod goserver-54dd6d8758-5nw46
Name:         goserver-54dd6d8758-5nw46
Namespace:    default
Priority:     0
Node:         k3d-fullcycle-server-0/192.168.96.2
Start Time:   Thu, 21 Jul 2022 11:51:26 -0300
Labels:       app=goserver
              pod-template-hash=54dd6d8758
Annotations:  <none>
Status:       Running
IP:           10.42.0.22
IPs:
  IP:           10.42.0.22
Controlled By:  ReplicaSet/goserver-54dd6d8758
Containers:
  goserver:
    Container ID:   containerd://e58e8ea926d5f080578c3f1a2374963f47605f707c70f7572bb33ea3a166cbf5
    Image:          rogeriocassares/hello-go:v2
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:2fdeab95d81a92603b44a7ee8e3236495db06b07606a4cd7abeddb8ad4030fb6
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 21 Jul 2022 11:51:29 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9lh6g (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-9lh6g:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  3m55s  default-scheduler  Successfully assigned default/goserver-54dd6d8758-5nw46 to k3d-fullcycle-server-0
  Normal  Pulling    3m54s  kubelet            Pulling image "rogeriocassares/hello-go:v2"
  Normal  Pulled     3m53s  kubelet            Successfully pulled image "rogeriocassares/hello-go:v2" in 1.366639434s
  Normal  Created    3m53s  kubelet            Created container goserver
  Normal  Started    3m53s  kubelet            Started container goserver
````



Agora sim pdemos ver que o ReplicaSet que subiu ao aplicarmos o novo deployment.yaml está funcionando com a versao v2 de nossa imagem!

Entaop no nosso dia a dia nao vamos subir RS, mas deployments!

Olha só´agora que interessante!

````bash
❯ kubectl get replicasets
NAME                  DESIRED   CURRENT   READY   AGE
goserver-6675499669   0         0         0       12m
goserver-54dd6d8758   10        10        10      6m25s
````

Vemos que temos dois ResplicaSets!



Um antigo (latest) e o outro mais atual (v2)! 



Entao aqui vamos perceber que o k8s nao mata o RS, mas mantem! 

A unica coisa que vai acontecer é que ele nao vai utilizar o RS. Isso é extremamente importante!



Portanto, quando mudamos a versao da imagem, por exemplo, vai ser criado um novo RS e o antigo apenas nao vai ser utilizado!



### Rollout e Revisão

Caso subirmos uma aplicação e ela estiver com algum tipo de bug e precisarmos imediatamente voltarmos para a versao anterior, não seria viável irmos para o arquivo de deployment.yaml e modificarmos o nome da nossa imagem para aplicar novamente a imagem antiga.

 Na realidade, o k8s tem uma solução para isso:

Como conseguimos pegar a lista do histórico de tudo o que está acontecendo de nossas versoes para conseguirmos trabalhar:

````bash
❯ kubectl rollout history deployment goserver
deployment.apps/goserver 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
````



Vemos aqui que ele trouxe para nós que temos duas revisoes! Uma é a latest e a outra é a v2.

Agora como sonseguimos voltar pra a versao anterior?

Vamos verificar os nossos Pods atuais:

`````bàsh
❯ kubectl get po 
NAME                        READY   STATUS    RESTARTS   AGE
goserver-54dd6d8758-5nw46   1/1     Running   0          32m
goserver-54dd6d8758-hskhw   1/1     Running   0          32m
goserver-54dd6d8758-6x7lz   1/1     Running   0          32m
goserver-54dd6d8758-9whzk   1/1     Running   0          32m
goserver-54dd6d8758-4cv8p   1/1     Running   0          32m
goserver-54dd6d8758-vls8d   1/1     Running   0          32m
goserver-54dd6d8758-9x6jh   1/1     Running   0          32m
goserver-54dd6d8758-t99v6   1/1     Running   0          32m
goserver-54dd6d8758-zmpr2   1/1     Running   0          32m
goserver-54dd6d8758-p2699   1/1     Running   0          32m
`````



Vamos fazer um rollout para que possamos fazer o undo do nosso deployment. Desta forma a seguir, ele vai voltar para a ultima versao que ele está rodando

````bash
❯ kubectl rollout undo deployment goserver
````



Mas também podemos voltar para uma versão específica da revisao que queremos voltar:

````bash
❯ kubectl rollout undo deployment goserver --to-revision=
````

Agora, vamos apenas fazer um undo:

````bash
❯ kubectl rollout undo deployment goserver
deployment.apps/goserver rolled back


❯ kubectl get po
NAME                        READY   STATUS              RESTARTS   AGE
goserver-54dd6d8758-5nw46   1/1     Running             0          36m
goserver-54dd6d8758-hskhw   1/1     Running             0          36m
goserver-54dd6d8758-9whzk   1/1     Running             0          36m
goserver-6675499669-79zpk   1/1     Running             0          8s
goserver-6675499669-b2j5x   1/1     Running             0          8s
goserver-6675499669-tnb98   1/1     Running             0          8s
goserver-54dd6d8758-vls8d   1/1     Terminating         0          36m
goserver-6675499669-gznx7   0/1     Pending             0          4s
goserver-6675499669-x4lb8   0/1     ContainerCreating   0          5s
goserver-6675499669-hnhhm   1/1     Running             0          8s
goserver-54dd6d8758-zmpr2   1/1     Terminating         0          36m
goserver-6675499669-hg6vz   0/1     Pending             0          3s
goserver-6675499669-h29lt   1/1     Running             0          8s
goserver-54dd6d8758-4cv8p   1/1     Terminating         0          36m
goserver-6675499669-5vhw9   0/1     Pending             0          2s
goserver-6675499669-87g4g   0/1     ContainerCreating   0          4s
goserver-54dd6d8758-9x6jh   0/1     Terminating         0          36m
````



Entao ele está terminando alguns pods e iniciando outros!

````BASH
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-6675499669-79zpk   1/1     Running   0          74s
goserver-6675499669-b2j5x   1/1     Running   0          74s
goserver-6675499669-tnb98   1/1     Running   0          74s
goserver-6675499669-hnhhm   1/1     Running   0          74s
goserver-6675499669-h29lt   1/1     Running   0          74s
goserver-6675499669-x4lb8   1/1     Running   0          71s
goserver-6675499669-87g4g   1/1     Running   0          70s
goserver-6675499669-gznx7   1/1     Running   0          70s
goserver-6675499669-5vhw9   1/1     Running   0          68s
goserver-6675499669-hg6vz   1/1     Running   0          69s
````

Agora todos estão já iniciados e rodando!

Vamos descrever ao menos um dos pods e verificar qual a versão que estamos utilizando!

`````bàsh
❯ kubectl describe po goserver-6675499669-79zpk
Name:         goserver-6675499669-79zpk
Namespace:    default
Priority:     0
Node:         k3d-fullcycle-server-0/192.168.96.2
Start Time:   Thu, 21 Jul 2022 12:27:27 -0300
Labels:       app=goserver
              pod-template-hash=6675499669
Annotations:  <none>
Status:       Running
IP:           10.42.0.32
IPs:
  IP:           10.42.0.32
Controlled By:  ReplicaSet/goserver-6675499669
Containers:
  goserver:
    Container ID:   containerd://b289fa78c3fd8bfae213cfa0f1b4efd1f51ee37306128aab52e47c12ec3d9642
    Image:          rogeriocassares/hello-go:latest
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:e3d0adf90eea6b94606c26c0c890f3c207b274ce70cc8eeb3fc66fc41247abb0
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 21 Jul 2022 12:27:29 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9g8bh (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-9g8bh:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m14s  default-scheduler  Successfully assigned default/goserver-6675499669-79zpk to k3d-fullcycle-server-0
  Normal  Pulling    2m14s  kubelet            Pulling image "rogeriocassares/hello-go:latest"
  Normal  Pulled     2m13s  kubelet            Successfully pulled image "rogeriocassares/hello-go:latest" in 1.399799047s
  Normal  Created    2m13s  kubelet            Created container goserver
  Normal  Started    2m13s  kubelet            Started container goserver
`````



Olha que incrível! Agora podemos ver que estamos utilizando a vesao latest e nao mais a v2. Ele realmente fez o rollout para o deploy anterior do goserver deployment!



Já que voltamos, podemos verificar os nossos ResplicaSets:

````bash
❯ kubectl get replicasets
NAME                  DESIRED   CURRENT   READY   AGE
goserver-54dd6d8758   0         0         0       40m
goserver-6675499669   10        10        10      46m
````

Enato vemos que a ResplicaSet anterior foi zerada e passamos a utilizar o ReplicaSet antigo mas nenhuma delas foi apagada!



Sobre o histórico, agora podemos contemplar a revisao 2 e 3:

``````bash
❯ kubectl rollout history deployment goserver 
deployment.apps/goserver 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
``````

Sendo que a revisao é a versao rodando agora!



Agora vamos mudar explicitamente para a revisao 2!

````bash
❯ kubectl rollout undo deployment goserver --to-revision=2
deployment.apps/goserver rolled back
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl get replicasets                                 
NAME                  DESIRED   CURRENT   READY   AGE
goserver-6675499669   3         3         3       50m
goserver-54dd6d8758   10        10        5       44m
````



E agora passamos a ver as alterações dos Pods ao visualizarmos as nossas ReplicaSets!

Vamos verificar os nossos Pods!

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-54dd6d8758-kh86r   1/1     Running   0          68s
goserver-54dd6d8758-szhzj   1/1     Running   0          68s
goserver-54dd6d8758-fpbfx   1/1     Running   0          68s
goserver-54dd6d8758-89tdg   1/1     Running   0          68s
goserver-54dd6d8758-kfqvq   1/1     Running   0          68s
goserver-54dd6d8758-lb8gz   1/1     Running   0          65s
goserver-54dd6d8758-cwsqw   1/1     Running   0          64s
goserver-54dd6d8758-zslhs   1/1     Running   0          63s
goserver-54dd6d8758-67zb2   1/1     Running   0          62s
goserver-54dd6d8758-8hs2r   1/1     Running   0          62s
````



Pronto! Os pOds das Replicas Sets antigas foram terminados e subimos novamente a imagem v2

````bash
❯ kubectl describe po goserver-54dd6d8758-kh86r
Name:         goserver-54dd6d8758-kh86r
Namespace:    default
Priority:     0
Node:         k3d-fullcycle-server-0/192.168.96.2
Start Time:   Thu, 21 Jul 2022 12:35:36 -0300
Labels:       app=goserver
              pod-template-hash=54dd6d8758
Annotations:  <none>
Status:       Running
IP:           10.42.0.40
IPs:
  IP:           10.42.0.40
Controlled By:  ReplicaSet/goserver-54dd6d8758
Containers:
  goserver:
    Container ID:   containerd://9ed0cde08cc14d6134ee73d7195ad3298751c67b4a035552bfbbf597a5a912c0
    Image:          rogeriocassares/hello-go:v2
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:2fdeab95d81a92603b44a7ee8e3236495db06b07606a4cd7abeddb8ad4030fb6
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Thu, 21 Jul 2022 12:35:38 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dgqq6 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-dgqq6:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  2m7s  default-scheduler  Successfully assigned default/goserver-54dd6d8758-kh86r to k3d-fullcycle-server-0
  Normal  Pulling    2m6s  kubelet            Pulling image "rogeriocassares/hello-go:v2"
  Normal  Pulled     2m5s  kubelet            Successfully pulled image "rogeriocassares/hello-go:v2" in 1.540216096s
  Normal  Created    2m5s  kubelet            Created container goserver
  Normal  Started    2m5s  kubelet            Started container goserver
````



Agora ficou claro como conseguimos trabalhar e controlar as versoes do nosso RS. É interessante ver que para cada eklementeo que estamos trabalhando, conseguiomos ver os eventos que estao acontecendo!

Podemos descrever o deployment do goserver, por exemplo:

````bash
❯ kubectl describe deployment goserver
Name:                   goserver
Namespace:              default
CreationTimestamp:      Thu, 21 Jul 2022 11:45:29 -0300
Labels:                 app=goserver
Annotations:            deployment.kubernetes.io/revision: 4
Selector:               app=goserver
Replicas:               10 desired | 10 updated | 10 total | 10 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=goserver
  Containers:
   goserver:
    Image:        rogeriocassares/hello-go:v2
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   goserver-54dd6d8758 (10/10 replicas created)
Events:
  Type    Reason             Age                   From                   Message
  ----    ------             ----                  ----                   -------
  Normal  ScalingReplicaSet  56m                   deployment-controller  Scaled up replica set goserver-6675499669 to 1
  Normal  ScalingReplicaSet  56m                   deployment-controller  Scaled up replica set goserver-6675499669 to 10
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled up replica set goserver-54dd6d8758 to 3
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled up replica set goserver-54dd6d8758 to 5
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled down replica set goserver-6675499669 to 8
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled up replica set goserver-54dd6d8758 to 6
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled down replica set goserver-6675499669 to 7
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled down replica set goserver-6675499669 to 6
  Normal  ScalingReplicaSet  50m                   deployment-controller  Scaled up replica set goserver-54dd6d8758 to 7
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled up replica set goserver-6675499669 to 5
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled down replica set goserver-54dd6d8758 to 8
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled up replica set goserver-6675499669 to 3
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled down replica set goserver-54dd6d8758 to 7
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled up replica set goserver-6675499669 to 6
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled down replica set goserver-54dd6d8758 to 6
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled up replica set goserver-6675499669 to 7
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled down replica set goserver-54dd6d8758 to 5
  Normal  ScalingReplicaSet  14m                   deployment-controller  Scaled up replica set goserver-6675499669 to 8
  Normal  ScalingReplicaSet  6m42s (x18 over 50m)  deployment-controller  (combined from similar events): Scaled down replica set goserver-6675499669 to 8
````



Podemos verificar a estrátégia de RollingUpdateStrategy, de no máximo 5% indiponível enquanto outros 25% vao surgindo, por exemplo. Mas o mais importyante de tudo é o Réplicas, que mostra quantas estão disponíveis, e os eventos que podemos ter!



Ele mostra tudo o que vai acontecendo com o Deployment, quando scale up/dowmn, cria um novo e etc.

E inclusive conseguimos ver qual RS ele está trabalhando! iNCLUSIVE MOSTRANBDO O NUMERO DA rEVISAO EM QUE ESTAMOS! nO CASDO, agora estamos na revisao 4!

`````bàsh
❯ kubectl rollout history deployment goserver             
deployment.apps/goserver 
REVISION  CHANGE-CAUSE
3         <none>
4         <none>
`````

 

## Services

### Entendendo o conceito de Services

Agora que temos a aplicaçãõ funcionando e dez pods rodando igualizinho.

Agora, como fazemos para alguem acessar essa app e quais deses pods a pessoa vai acessar. Para qual pods devemos ser encaminhados?

O primeiro ponto, a nossa aplicação rodando nao significa que ela pode ser axcessada. Entao criamos o services, que é a porta de entrada para a nossa aplicação.

Imaginemos que criamos um service x e toda vez que alguem acessar o service x ele vai redirecionar para uma aplicação como um load balance!

Nós nao precisamos nos preocupar com esse tipo de load balance inicialmemte! Basta atribuitmos o nosso service ao deployment, inicialmente (vamos entender depois como fazemos isso para entender o filtro e selector) mas a grande sacada é que o k8s gerencia tudo para nós!

Entao vamos criar um service, o k8s vai fazer o serviço de service discovery, ele vai selecionar qual o melhor pod para aser acessado naquele momento edar o acesso para nós!

O port forwar é u recusso que usamos para acessar um pod ou um service quando nao temos um IP direto para acessar essa máquina.

Mas obviamente, quando a aplicação estiver em produção, ninguém vai conseguir dar um port-forward. Vamos ter que passar pelo menos um dominio ou um Ip e esse ip sempre vai ser apontado para um determinado service.

No fim das coisas, tudo nao passa de associação!



### Utilizando um ClusterIP

Vamos criar um arquivo chamado service.yaml

O selector é´o responsavel por filtrar todos os pods que vao estar incorporados neste serviço.

Imaginamos que temos 500 pods em uma aplicação. Quais deles vao ser utilizados para esse service?

O selector serve para filtrar e saber quais pods vao ser usado snessa aplicação.

Quando estamos no arquivo de deployment e vemos o selector de lá. ele é o reponsável por indicar qual cara wque vamos fazer essa seleção! No caso do deployment.yaml a selecção é´o app: goserver. 

Logo, vamos descrever ao service para que ele pegue todos os pods cuja a parte de matchlabel dentro do seu selector onde app é goserver.

Entao ele vai verificar se cada deployent tem como label no selector -> matchlabel app igual a goserver. Entao todos os pods desse deployment farao parte daquele service!

Essse é o filtro. É dessa forma que consegumos diferenciar um serviço do outro.

Se tivermos um serviço de nginx, por exemplo, ele vai filtrar apenas os pods que tenham um matchlabel com a regra que temos colocado tipo app: goserver ou server: nginx.



No nosso caso aqui, selector é o app: goserver. ntao nesse momento ele vai opegar todos os pods que o matchlabel for app: goserver.



Outa coisa importantissisma!Qual o type do nosso service?

Os tipos são:

- ClusterIP, 
- node port, 
- load balancer e 
- headless service.



Agora vamos explorar o ClusterIP.

Agora, vamos colocar o ports. Quis portas vamos acessar em relação aos nossos pods. Entualmente é interessante colocar um nome como uma regra para varias portas uttlizando os - para definir os nomes.



No nosso serviço go, estamos disponiblizando a porta 8080 e o protocolo TCP



service.yaml

``````yaml
apiVersion: v1
kind: Service
metadata:
  name: goserver-service
spec:
  selector:
    app: goserver
  type: ClusterIP
  ports:
  - name: goserver-service
    port: 8080
    protocol: TCP

``````



Vamos executar!

````bash
❯ kubectl apply -f k8s/service.yaml
service/goserver-service created
````



Vamos ver se realemnte criou o serviço com kubectl get services ou  kubectl get svc

``````bash
❯ kubectl get svc     
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes         ClusterIP   10.43.0.1       <none>        443/TCP    124m
goserver-service   ClusterIP   10.43.174.111   <none>        8080/TCP   47s
``````



Agora, temos aqui dois services.

O service kubernetes é o service padrão para receber requisição api, e etc. 

Já o service goserver-service é  oservice que nós criamos. Se olharmos, tem o type E cLUSTERip E UM IP!

Esse clusterIP é um IP interno do servidor. Entao todos que estiverem dentro do cluster k8s vao conseguir acessar esse IP e entao esse service nesse IP vai nos falar qual dos pods disponíveis algue, vai conseguir acessar.

Ou chamamdo ele pelo IP ou podemos chamar o app pelo nome do service, visto eu o k8s trabalha com resolução de nome via DNS. 

Vamos imaginar agora que ele é´um db, entao colcoamos root:root@goserver-service. Entao esse host se torna visivel dentro do k8s!

Entao na maioria das vezes, o que vai acontecer é chamarmos pelo nome ao oives do IP!



Como acessamos esse pod estando nós fora do Cluster?

Entao, vamos fazer um redirecionamento de portas para que quando acessar uma porta de nossa maquina conseguiomos acessar uma porta de nossa maquina:

````bash
❯ kubectl port-forward svc/goserver-service 8000:8080
Forwarding from 127.0.0.1:8000 -> 8080
Forwarding from [::1]:8000 -> 8080
````



Pronto! 

Vamos agora tentar acessar o endereço!

`````bàsh
❯ curl http://localhost:8000
<h1>Hello Fullcycle!!!</h1>%  
`````

E funcionou!

Agora, a diferença é que antes tinhamos direto de um Pod e agora estamos fazendo para um service e este service vai chgmar automaticamente um dos 10 pods criados para fazwr esse tipo de balanceamento!



### Diferenças entre Port e targetPort

Um detalhe muito importante. Até agora fomos no serviços e falamos para ele ir na porta 8080.

E quando tivermos uma aplicaçãõ que rode na porta 8000 mas que quando alguem acesse o service ela acesse pela porta 80?

Entao existem duas coisas aqui. Uma coisa é o service e outra coisa é o container.

Nós podemos acessar o nosso service pela porta 80 e o service vai nos redirecionar para a porta 8000 do nosso Pod/Container.

Vamos gerar uma versao 3 do server.go para a porta 8000, e modificar o deployment para isso.

server.go

````go
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("<h1>Hello Fullcycle!!!</h1>"))
}

````

Build and push

````bash
❯ docker build -t rogeriocassares/hello-go:v3 .
❯ docker push rogeriocassares/hello-go:v3
````

deployment.yaml

````bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 10
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v3"

````

Aplicar o novo arquivo deployment.yaml

````bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured
````



Entao se deixarmos a porta errada do container no acesso do service, nao vai funcionar! Pq iremos acessar o serviço na porta 80 e ele vai redirecionar o serviço para  a porta 8080 da nossa apliccação do container. que nao tem nada funcionando. Vamos tentar fazer isso para ver o erro.

A intenção é que quando acessarmos a porta 9000 de nossa máquina, ele acessa o serviço na 8080

`````bàsh
❯ kubectl port-forward svc/goserver-service 9000:8080
`````

Entao vimos que isso nao funcionou. ao acessar o http://localhost:9000 pois fomos redirecionados para a porta 8080, sendo que o container está rodando na porta 8000.

Como resolvemos isso: Poder acessar um container na porta 8080 e fazer com que o serviço redirecione para a porta que quisermos dentro do nosso container?



Resolvemos isso com o targetPort, que é a porta do nosso conatiner!

Entao todas as vezes em que acessarmos o service na porta 8080, ele vai redirecionar para a porta 8000 do nosso container.

QUANDO NAO COLOCAMOS NADA, ELE FALA QUE O TARGET PORT É IGUAL À NOSSA PORT

service.yaml

````yaml
apiVersion: v1
kind: Service
metadata:
  name: goserver-service
spec:
  selector:
    app: goserver
  type: ClusterIP
  ports:
  - name: goserver-service
    port: 8080
    targetPort: 8000
    protocol: TCP

````

E agora aplicamos o novo serviço no k8s.

````bash
❯ kubectl apply -f k8s/service.yaml 
service/goserver-service configured
````



E aplicamos um port-forward na porta 9000

````bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````



Entao agora estamos assim:

Client/localhost-pc (:9000) -> goserver-service (:8080) -> Pod/Container (:8000)

E entao, se formos acessar o http://localhost:9000

``````bash
❯ curl http://localhost:9000

<h1>Hello Fullcycle!!!</h1>
``````



Funcionou!!!

Port e Target port:

PORT É A PORTA DO SERVICE E NAO A PORTA DO CONTAINER!



Entao podemos criar um service na porta 443, e quando alguém acessa, podemos escolher qual é a target porta que aquele service vai acessar qualquer um dos 10 containers e essa ultima porta é a chamada target port!



### Utilizando proxy para acessar API do Kubernetes



Como o k8s funciona em relação a api E COMO ELE FUNCIONA QUANDO UTILIZAMOS O KUBECTL

O kubectl nada mais é que um biario executável que é um client que se comunica com a API do k8s através de certificados autenticados. Essa API do k8s pode ser acessada diretamente e é isso que vamos ver agora.

Primeiramente, nesse nosso caso, ele está em uma rede fechada. cOMO ENTAO PDODEMOS ACESSAR ESSA REDE FECHADA?

O comando kubectl proxy gera um proxy da nossa maquina com o cluster k8s para fazermos esse acesso. Por exemplo

````bash
❯ kubectl proxy --port=8080 
Starting to serve on 127.0.0.1:8080
````



Nesse ponto, toda vez que acessarmos a porta 8080 vamos bater na API do k8s.

Entao quando acessamos http://localhost:8080, na web, acessamos a listagem de todos os objetos da API do k8s.

````json
// 20220721184325
// http://localhost:8080/

{
  "paths": [
    "/.well-known/openid-configuration",
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/admissionregistration.k8s.io",
    "/apis/admissionregistration.k8s.io/v1",
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1",
    "/apis/apps",
    "/apis/apps/v1",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/autoscaling/v2",
    "/apis/autoscaling/v2beta1",
    "/apis/autoscaling/v2beta2",
    "/apis/batch",
    "/apis/batch/v1",
    "/apis/batch/v1beta1",
    "/apis/certificates.k8s.io",
    "/apis/certificates.k8s.io/v1",
    "/apis/coordination.k8s.io",
    "/apis/coordination.k8s.io/v1",
    "/apis/discovery.k8s.io",
    "/apis/discovery.k8s.io/v1",
    "/apis/discovery.k8s.io/v1beta1",
    "/apis/events.k8s.io",
    "/apis/events.k8s.io/v1",
    "/apis/events.k8s.io/v1beta1",
    "/apis/flowcontrol.apiserver.k8s.io",
    "/apis/flowcontrol.apiserver.k8s.io/v1beta1",
    "/apis/flowcontrol.apiserver.k8s.io/v1beta2",
    "/apis/helm.cattle.io",
    "/apis/helm.cattle.io/v1",
    "/apis/k3s.cattle.io",
    "/apis/k3s.cattle.io/v1",
    "/apis/metrics.k8s.io",
    "/apis/metrics.k8s.io/v1beta1",
    "/apis/networking.k8s.io",
    "/apis/networking.k8s.io/v1",
    "/apis/node.k8s.io",
    "/apis/node.k8s.io/v1",
    "/apis/node.k8s.io/v1beta1",
    "/apis/policy",
    "/apis/policy/v1",
    "/apis/policy/v1beta1",
    "/apis/rbac.authorization.k8s.io",
    "/apis/rbac.authorization.k8s.io/v1",
    "/apis/scheduling.k8s.io",
    "/apis/scheduling.k8s.io/v1",
    "/apis/storage.k8s.io",
    "/apis/storage.k8s.io/v1",
    "/apis/storage.k8s.io/v1beta1",
    "/apis/traefik.containo.us",
    "/apis/traefik.containo.us/v1alpha1",
    "/healthz",
    "/healthz/autoregister-completion",
    "/healthz/etcd",
    "/healthz/log",
    "/healthz/ping",
    "/healthz/poststarthook/aggregator-reload-proxy-client-cert",
    "/healthz/poststarthook/apiservice-openapi-controller",
    "/healthz/poststarthook/apiservice-registration-controller",
    "/healthz/poststarthook/apiservice-status-available-controller",
    "/healthz/poststarthook/bootstrap-controller",
    "/healthz/poststarthook/crd-informer-synced",
    "/healthz/poststarthook/generic-apiserver-start-informers",
    "/healthz/poststarthook/kube-apiserver-autoregistration",
    "/healthz/poststarthook/priority-and-fairness-config-consumer",
    "/healthz/poststarthook/priority-and-fairness-config-producer",
    "/healthz/poststarthook/priority-and-fairness-filter",
    "/healthz/poststarthook/rbac/bootstrap-roles",
    "/healthz/poststarthook/scheduling/bootstrap-system-priority-classes",
    "/healthz/poststarthook/start-apiextensions-controllers",
    "/healthz/poststarthook/start-apiextensions-informers",
    "/healthz/poststarthook/start-cluster-authentication-info-controller",
    "/healthz/poststarthook/start-kube-aggregator-informers",
    "/healthz/poststarthook/start-kube-apiserver-admission-initializer",
    "/livez",
    "/livez/autoregister-completion",
    "/livez/etcd",
    "/livez/log",
    "/livez/ping",
    "/livez/poststarthook/aggregator-reload-proxy-client-cert",
    "/livez/poststarthook/apiservice-openapi-controller",
    "/livez/poststarthook/apiservice-registration-controller",
    "/livez/poststarthook/apiservice-status-available-controller",
    "/livez/poststarthook/bootstrap-controller",
    "/livez/poststarthook/crd-informer-synced",
    "/livez/poststarthook/generic-apiserver-start-informers",
    "/livez/poststarthook/kube-apiserver-autoregistration",
    "/livez/poststarthook/priority-and-fairness-config-consumer",
    "/livez/poststarthook/priority-and-fairness-config-producer",
    "/livez/poststarthook/priority-and-fairness-filter",
    "/livez/poststarthook/rbac/bootstrap-roles",
    "/livez/poststarthook/scheduling/bootstrap-system-priority-classes",
    "/livez/poststarthook/start-apiextensions-controllers",
    "/livez/poststarthook/start-apiextensions-informers",
    "/livez/poststarthook/start-cluster-authentication-info-controller",
    "/livez/poststarthook/start-kube-aggregator-informers",
    "/livez/poststarthook/start-kube-apiserver-admission-initializer",
    "/logs",
    "/metrics",
    "/openapi/v2",
    "/openid/v1/jwks",
    "/readyz",
    "/readyz/autoregister-completion",
    "/readyz/etcd",
    "/readyz/informer-sync",
    "/readyz/log",
    "/readyz/ping",
    "/readyz/poststarthook/aggregator-reload-proxy-client-cert",
    "/readyz/poststarthook/apiservice-openapi-controller",
    "/readyz/poststarthook/apiservice-registration-controller",
    "/readyz/poststarthook/apiservice-status-available-controller",
    "/readyz/poststarthook/bootstrap-controller",
    "/readyz/poststarthook/crd-informer-synced",
    "/readyz/poststarthook/generic-apiserver-start-informers",
    "/readyz/poststarthook/kube-apiserver-autoregistration",
    "/readyz/poststarthook/priority-and-fairness-config-consumer",
    "/readyz/poststarthook/priority-and-fairness-config-producer",
    "/readyz/poststarthook/priority-and-fairness-filter",
    "/readyz/poststarthook/rbac/bootstrap-roles",
    "/readyz/poststarthook/scheduling/bootstrap-system-priority-classes",
    "/readyz/poststarthook/start-apiextensions-controllers",
    "/readyz/poststarthook/start-apiextensions-informers",
    "/readyz/poststarthook/start-cluster-authentication-info-controller",
    "/readyz/poststarthook/start-kube-aggregator-informers",
    "/readyz/poststarthook/start-kube-apiserver-admission-initializer",
    "/readyz/shutdown",
    "/version"
  ]
}
````



Isso tudo é o k8s que está expondo para nós!

Uma das APIs que utilizamos é a API v1, que é a api que utilizamos para entrar nos nossos services. Para saber mais, basta clicar nela.

Se colocarmos http://localhost:8080/api/v1/namespaces/default/services/goserver-service, vamos ver que estamos acessando  endpoint do service que criamos que traá as principais alterações que fizemos no service.yaml! E entao temos mais informações em relação aquilo que estamos fazendo.

`````json
// 20220721184803
// http://localhost:8080/api/v1/namespaces/default/services/goserver-service

{
  "kind": "Service",
  "apiVersion": "v1",
  "metadata": {
    "name": "goserver-service",
    "namespace": "default",
    "uid": "bcb3cb12-6692-4036-bfb9-7252b7a25473",
    "resourceVersion": "5903",
    "creationTimestamp": "2022-07-21T16:06:45Z",
    "annotations": {
      "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Service\",\"metadata\":{\"annotations\":{},\"name\":\"goserver-service\",\"namespace\":\"default\"},\"spec\":{\"ports\":[{\"name\":\"goserver-service\",\"port\":8080,\"protocol\":\"TCP\",\"targetPort\":8000}],\"selector\":{\"app\":\"goserver\"},\"type\":\"ClusterIP\"}}\n"
    },
    "managedFields": [
      {
        "manager": "kubectl-client-side-apply",
        "operation": "Update",
        "apiVersion": "v1",
        "time": "2022-07-21T16:06:45Z",
        "fieldsType": "FieldsV1",
        "fieldsV1": {
          "f:metadata": {
            "f:annotations": {
              ".": {
                
              },
              "f:kubectl.kubernetes.io/last-applied-configuration": {
                
              }
            }
          },
          "f:spec": {
            "f:internalTrafficPolicy": {
              
            },
            "f:ports": {
              ".": {
                
              },
              "k:{\"port\":8080,\"protocol\":\"TCP\"}": {
                ".": {
                  
                },
                "f:name": {
                  
                },
                "f:port": {
                  
                },
                "f:protocol": {
                  
                },
                "f:targetPort": {
                  
                }
              }
            },
            "f:selector": {
              
            },
            "f:sessionAffinity": {
              
            },
            "f:type": {
              
            }
          }
        }
      }
    ]
  },
  "spec": {
    "ports": [
      {
        "name": "goserver-service",
        "protocol": "TCP",
        "port": 8080,
        "targetPort": 8000
      }
    ],
    "selector": {
      "app": "goserver"
    },
    "clusterIP": "10.43.174.111",
    "clusterIPs": [
      "10.43.174.111"
    ],
    "type": "ClusterIP",
    "sessionAffinity": "None",
    "ipFamilies": [
      "IPv4"
    ],
    "ipFamilyPolicy": "SingleStack",
    "internalTrafficPolicy": "Cluster"
  },
  "status": {
    "loadBalancer": {
      
    }
  }
}
`````





Qualquer coisa que fazemos no k8s acontece via api e aqui está a api para podemos consutá-las. Uma vez que entendemos como ela funciona, as coisas vao clareando porque conseguimos entender o que está por trás!

basicamente o kubectl apply faz uma requisição em um desses endpoints para criar qualquer coisa que precisamos.



### Utilizando NodePort

O Node Port é um tipo diferente de serviço do CluserIP. Esse serviço é o mais arcaico que podemos ter quando quereremos acessar o cluster kubernets de fora dele.

O clusterIP gera um IP interno no cluster que conseguimos acessar esses serviços internamente.

Mas quando queremos acessar de fora o cluster, precisamos ter outros tipos de serviços e um dele é Node Port

.

O Node-Port funciona assim:

Imaginemos que temos o Node 1, o Node 2, o Node 3 e o Node 4 com diversos Pos cada máquina. Entao de repente queremos acessar um serviço utilizando o Node Port.

Entao, quando falar que vamos usar o Node Port, vamos usar uma porta Alta no Node 1, por exemplo. Porta alta é com o numero maior que 30000 e menor que 32767. Vamos pegar a porta 30001. Quando falamamos isso e criamos o Node Port, essa porta 3001 vai ser aberta em todos os nossos nodes!

E o que vai acontecer é que qualquer pessoa que souber o IP ou endereço de qualquer um dos nodes do nosso cluster e acessar essa porta vai cair em nossos serviçõs.

Portanto, o Node Port geram uma porta e libera a porta em todos os Nodes do cluster, independente do Node que iremos acessar e entao consegumos entrar nesses serviço.



Geralmente, esse tipo é quando se quer fazer uma demontraçãoo ou uma frorma temporaria de subir um serviço e vai sar do ar, ou ttablhar com um proprio load balance etc. Na prática, com aplicações em produção, vai ser muito dificil colocar esse tipo de serviço no ar pq existem serviços mais adequados em relação a isso.



Se nao passarmos nada no arquivo de configuração, ele vai gerar uma porta automaticamente 

service.yaml

````yaml
apiVersion: v1
kind: Service
metadata:
  name: goserver-service
spec:
  selector:
    app: goserver
  type: NodePort
  ports:
  - name: goserver-service
    port: 8080
    targetPort: 8000
    protocol: TCP
    nodePort: 30001

````

Vamos apicar esse serviço

````bash
❯ kubectl apply -f k8s/service.yaml
service/goserver-service configured
````



Vamos ver o nosso NodePort

````bash
❯ kubectl get svc
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP   10.43.0.1       <none>        443/TCP          8h
goserver-service   NodePort    10.43.174.111   <none>        8080:30001/TCP   5h59m
````



Entao olha o que está acontecendo. Todo mundo que acessar qualquer um dos nodes do cluster pela porta 30001 vai cair na porta 8080 do nosso serviço! E todo mundo que cair na porta 8080 do nosso serviço vai ser redirecionado para a porta 8000 do container que está rodando este serviço.

Supondo que esse cluset tenha 1 master e 4 workres. Se entrarmos em qualquer um deles na porta 30001, vamos cair nesse service goserver-service.



Entao, a ideia do NodePort é conseguirmos expor uma porta para fora do nosso cluster.







### Trabalhando com LoadBalancer

Primeiramente, vamos deletar o serviço NodePort

````bash
❯ kubectl get svc                                       
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP   10.43.0.1       <none>        443/TCP          23h
goserver-service   NodePort    10.43.174.111   <none>        8080:30001/TCP   21h
rogerio in FullCycle/3.0/4.K8s 
❯ 
rogerio in FullCycle/3.0/4.K8s 
❯ kubectl delete svc goserver-service
service "goserver-service" deleted
````



Vamos falar qu eo tipo de serviço é LoadBalancer em service.yaml:

````yaml
apiVersion: v1
kind: Service
metadata:
  name: goserver-service
spec:
  selector:
    app: goserver
  type: LoadBalancer
  ports:
  - name: goserver-service
    port: 8080
    targetPort: 8000
    protocol: TCP

````



O ClusterIP é o IP interno do servidor, o nodeport conseguomos expor atraves de uma porta atraves de qualquer um dos noses. E o loadBalancer gera um IP para acessarmos a nossa aplicação de fora.

Esse tipo de serviço é ,uito utilizado quando utilizamos um cluster gerenciado, um cluster k8s que está conectado a um provedor  de nuvem 

Imaginamos que estamos na AWS, qaundo criamos um server do tipo LoadBalancer, ele vai gerar automaticamente um IP externo e todos que acessarem esse IP externo vai conseguir acessar determinado servidor.

vAMOS APLICAR ESSE ARQUIVO:

````bash
❯ kubectl apply -f k8s/service.yaml
service/goserver-service created
````

Pronto! Vamos verificar:

`````bash
❯ kubectl get svc                    
NAME               TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
kubernetes         ClusterIP      10.43.0.1       <none>         443/TCP          24h
goserver-service   LoadBalancer   10.43.203.126   192.168.96.2   8080:30210/TCP   6s
`````



Da mesma forma que o Cluster IP, tem um IP interno, o LB tb gera!

Mas o LoadBalancer gera um IP externo tb. Um IP que podemos passar para qualquer outro sistema para colocar.

Se estivessemos numa AWS por exemplo e rodarmos esse serviço, vai aparecer um novo IP para nós e qualquer pessoa vai poder acessar esse IP.

Uma coisa interessante de quando utilizamos o LB tb ele vai gerar um NodePort (:30210). 

Entao, aqui vimos que o LB é o suprasumo. Ele contem o ClusterIP, o nODEpORT E AO MESMO TEMPO ELE TEM UM ip EXCLUSIVO!

NOTA: Normalmente, quanddo quisermos expor qualquer aplicação para a Internet de maneira geral, vamos usar 



## Objetos de Configuração

### Entendendo objetos de configuração

Não nessáriamente objetos k8s, mas também variaveis de ambiente das aplicações. como confuramos no k8s para a nossa aplicação ou eventualmente injetar um arquivo de configuração que criamos para as informações dinamicas que precisamos. Como guardar passwds e dados sensiveis no k8s para que sejam usados como variaveis de ambiente dentro da aplicação.

Por conta disso, temos alguns objetos para nos ajudar a criar esses arquivos e dados sensives de uma forma muito tranquila.

O que  muda de um abiende dev para prod, geralmente sao esses dados sensiveis que o k8s estabelece esses recursos!



### Usando variaveis de ambiente

Como configurar variaveis de ambiente no k8s para que a nossa aplicaçao consiga acessar.

A primeira coisa é adicionar variaveis de ambiente na aplicação server.go para que ela as acesse.

````go
package main

import (
	"fmt"
	"net/http"
	"os"
)

func main() {
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

````

Agora vamos gerar o build e dar o push no docker registry!

````bash
❯ docker build -t rogeriocassares/hello-go:v4 .
❯ docker push rogeriocassares/hello-go:v4 
````

Vamos configurar o deployment com uma replica, v4, e colocar uma label com env!

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v4"
          env:
            - name: NAME
              value: "Rogerio"
            - name: AGE
              value: "29"

````



Entao vamos aplicar esse arquivo no k8s

````bash
❯ kubectl apply -f  k8s/deployment.yaml           

deployment.apps/goserver configured
````

Agora vamos fazer a configuração de porta para scessar as aplicaçãõ e ver se esta tudo funcionando!

````bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````

Acessar http://localhost:9000

E funcionou!

### Variaveis de ambiente com Config Map

nÃO É MUITO LEGAL COLCOAR VARIAVEIS DE AMBIENTES DENTRO DO DEPLOYMENT, MAS SIM EM UMA ARQUIVO SEPARADO E IMPORTA-LO NO DEPLOYMENT.

Para isso, o k8s possui o ConfigMap.

O ConfigMap basicamente é uma mapa de configuração que conseguimos colocar e utilizá-lo de diversas formas na aplicação

Priemiro exemplo

Camos criar um arquivo config map

configmap-env.yaml

````yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: goserver-env
data:
  NAME: "Rogerio"
  AGE: "29"
````



Após criarmos o mapa de configuração, vamos fazer uma pequena mudança no deployment.yaml

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v4"
          env:
            - name: NAME
              valueFrom:
                configMapKeyRef:
                  name: goserver-env
                  key: NAME

            - name: AGE
              valueFrom:
                configMapKeyRef:
                  name: goserver-env
                  key: AGE

````



Entao, agora que alteramos o deployment, o configmap é uma referencia para o arquivo deployment.

NOTA: SE MUDARMOS O CONIMAP, O DEPLOYMENT NAO VAI MUDAR AUTOMATICAMENTE!

pRECISAMOS RECIRAR E ATUALIZAR O DEPLOYMENT PARA ELE LER O CONFIGMAP NOVO!

Vamos aplicar o config map

````bash
❯ kubectl apply -f k8s/configmap-env.yaml
configmap/goserver-env created
````



E entao vamos aplicar o deployment

````
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured
````



Agora o nosso deployent esta usando o config map e vamos ver se funcionou usando o port-forward

````bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````

E acessando o http://localhost:9000

E funcionou!





Porém existe uma forma mais fácil, porem nem sempre aplicavel ao nosso caso.

Vamos imaginar que gostariamos de pegar todas as variaveis que estao no configmap e transformá-las em variaveis de ambiente.

Imaginemos que temos 100 parametros que queremos passas como variaveis de ambiente. O arquivo de deployment vai ficar enorme!



ao inves de colocar env, vamos colocar envFrom e vai puxarr todas as keys do configMap no formato de variavel de ambiente! E o deployment fica como a seguir:

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v4"
          envFrom:
            - configMapRef:
                name: goserver-env
          
          # env:
          #   - name: NAME
          #     valueFrom:
          #       configMapKeyRef:
          #         name: goserver-env
          #         key: NAME

          #   - name: AGE
          #     valueFrom:
          #       configMapKeyRef:
          #         name: goserver-env
          #         key: AGE

````



Vamos aplicar, fazer o port forward e acessar o http://localhost:9000

````bash
❯ kubectl apply -f k8s/deployment.yaml               
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````

E funcionou!



Entao temos aqui tres formas claras de trabalharmos com k8s usando variaveis de ambientes:

1. colocando env e passando a key e o value
2. colcoando o configmap e passando a key e o value passando o config map
3. pegar todos as keys do configmao e colocando para variavel de ambiente no dployment.



### Injetando ConfigMap na aplicação

Muitas vezes temos arquivos que sao dinamicos. Vamos imaginar que temos um token txt e esse token (arquivo) tem que estar na raiz do projeto.

E esse arquivo muda de proj pra proj,

Ou muitas vezes queremos colocar um arquivo .env na raiz do nosso projeto tb!

Ou queremos fazer a substituiçãõ de um arquivo que já existe no projeto por um oitro arquivo que queremos criar com uma conf especifica. Exemplo. Antes de subir o nginx temos um arquivo .conf e queremos colocar esse arquivo .conf no lugar do .conf original.

Pq atualmente, temos que fazer a alteraçãõ lá´no nginx, gerar uma imagem e subir.

Mas se tivermos que mudar uma porta ou dominio, temos que sair criando imagens.

Como fazemos isso de uma forma mais flexivel?

Entao podemos tb usar o configmap para ser um arquivo fisico que vai ser injetado lá no container para podermos utilizar!

Isso realmente é fantatisco e muito utilizado no k8s.

Vamos imageinas que queremos criar membros da familia.

Entao, no arquivo server.go vamos criar uma funçãõ ConfigMap e ler um arquivo que vai estar dentro do nosso projeto. Vamos ler esse arquivo e imprimir o valor desse arquivo.

server.go

````´´´ǵo
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
)

func main() {
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}
````

Vamos buildar e dar um push

````bash
❯ docker build -t rogeriocassares/hello-go
❯ docker push rogeriocassares/hello-go:v5
````

Vamos criar um arquivo configmap-family.yaml

````yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap-family
data:
  members: "pai, mae, irmao 1, irmao2, irmao3"
  
````

E vamos aplicar esse arquivo ao k8s:

``````bash
❯ kubectl apply -f k8s/configmap-family.yaml 
configmap/configmap-family created

``````

Agora que temos o configmap criado e a imagem criada, falta fazermos o ajuste e no deployment!

Quando estamos falando em pegar um arquivo de um configmap e injetar na nossa aplicação, estamos falando de um volume que está sendo injetado na nossa aplicação.

Uma vez que temos esse volume, vamos montá-lo nna aplicação. Entao precisamos declarar esse volume e montar na aplicação.

No configmap vamos falar quais itens queremos montar para ele.



Agora que temos o volume declarad, temos que montar o volume dentro do deployment, dentro do container. com o volumeMounts

Entao estamos montando o volume config na pasta /go/myfamily dentro da nossa aplicação.

deployment.yaml

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5"
          envFrom:
            - configMapRef:
                name: goserver-env

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"
````

Vamos aplicar o deployment

````bash
❯ kubectl apply -f k8s/deployment.yaml
deployment.apps/goserver configured

````

E está rodando

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-6bc696c769-wl458   1/1     Running   0          25s
````

Vamos dar o port forward

````bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````

Vamos ver se está tudo certo em http://localhost:9000 e está ok.

Agora vamos em http://localhost:9000/configmap E FUNCIONOUUu!!



Como podemos ver se no caso deu erro?

Podemos ver se o pod deu restart

```bàsh
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-6bc696c769-wl458   1/1     Running   0          4m41s
```

Vamos copiar o nome do nosso Pod e dazer exatamente como fazemos no docker. Entao vamos fazer um exec nele! E acessar o arquivo do family.txt montado no nosso Pod!

````bash
❯ kubectl exec -it goserver-6bc696c769-wl458 bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@goserver-6bc696c769-wl458:/go# ls
bin  myfamily  server  server.go  src
root@goserver-6bc696c769-wl458:/go# cat myfamily/family.txt 
pai, mae, irmao 1, irmao2, irmao3root@goserver-6bc696c769-wl458:/go#
````



Vimos que o arquivo foi montado! Entao o erro nao deve estar no deplyment mas pode ser no programa em go.

Vamos agora pegar os logs!

````bash
❯ kubectl logs goserver-6bc696c769-wl458
````

Vamos alterar o caminho no server.go para a raiz.

````go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
)

func main() {
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("/go/myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}

````



Deployy e push`

````bash
❯ docker build -t rogeriocassares/hello-go:v5.1 .
❯ docker push rogeriocassares/hello-go:v5.1
````



Ajusta o deplumnet para a versao 5.1

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.1"
          envFrom:
            - configMapRef:
                name: goserver-env

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"
````

E aplciamos o deployment com o port forward

````bash
❯ kubectl apply -f k8s/deployment.yaml              
deployment.apps/goserver configured

❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````



Vamos em http://localhost:9000 e tb em http://localhost:9000/configmap e ambos funcionaram!!!

Para garantirmos que esses arquivos nunca serao modificados, podemos colocar uma configuração de readonly: true em volumeMonts:

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.1"
          envFrom:
            - configMapRef:
                name: goserver-env

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"
````



Assim, ninguem e nem o proprio sistema vai conseguir modificar!



#### Secrets e variaveias de ambiente

Secrets é´um objeto do k8s para guardar dados mais sensíveis. Os dados do secret fica um pouco mais ofuscado que no configmap mas não é a forma mais segura do mundo. Mas existem integraçoes do k8s com vault, kms para ninguem ter acesso a esses secrets. Existem alguns tipos de secrtes, mas na prática, o que usaremos é um secret do tipo opaco, que nao deixa esses sececrts ali visiveis e facilmente a vista, mas essa visibilidade pe uma pseudo visibilidade., 





Vamos imaginar que em nossa apicação go vamos criar um novo endpoint para secret.

````go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
)

func main() {
	http.HandleFunc("/secret", Secret)
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func Secret(w http.ResponseWriter, r *http.Request) {
	user := os.Getenv("USER")
	password := os.Getenv("PASSWORD")
	fmt.Fprintf(w, "User: %s. Paswword: %s.", user, password)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("/go/myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}

````

Pronto. Agora vamos gerar uma nova versão:

``````bash
❯ docker build -t rogeriocassares/hello-go:v5.2 .
❯ docker push rogeriocassares/hello-go:v5.2
``````



Agora vamos criar um arquivo k8s/secret.yaml E COLOCAR OS NOSSOS VALORES DE SECRET EM BASE64

````BASH
❯ echo "rogerio" | base64
cm9nZXJpbwo=

❯ echo "123456" | base64
MTIzNDU2Cg==
````

Pronto!

Fica assim:

````yaml
apiVersion: v1
kind: Secret
metadata:
  name: goserver-secret
type: Opaque
data:
  USER: "cm9nZXJpbwo="
  PASSWORD: "MTIzNDU2Cg=="
  
````



Ma atenção! O base 64 não é criptografia! O importnte, aqui é nao ter uma forma visual descaradamente. Esse é um padrão do k8s. Existem formas mais seguras de terceiros. 



Vamos aplicar esse secret

````bash
❯ kubectl apply -f k8s/secret.yaml 
secret/goserver-secret created
````

Agora como faremos para o go utilizar o nosso secret?

Entao, lá em deployment precisamos colocar secretRef ao inves de configmapRef.

deployment.yaml

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.2"
          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"
````

Vamos agora aplicar o deployment

````bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured
````

E dar o port forward:

````bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
````



Vamos acessar http://localhost:9000/secret!

E FUNCIONOU!

Nao podemos nos esquecer que agora isso virou variavel de ambiente! Vamos ver:

````bash
❯ kubectl get po
NAME                       READY   STATUS    RESTARTS   AGE
goserver-855f7c97b-kdlcw   1/1     Running   0          3m9s

❯ kubectl exec -it goserver-855f7c97b-kdlcw -- bash
root@goserver-855f7c97b-kdlcw:/go# echo $USER
rogerio
root@goserver-855f7c97b-kdlcw:/go# echo $PASSWORD
123456

````

vEMOS AQUI QUE ESTAMOS COM VARIAVEIS DE AMBIENTE. sECRETS SAO BASICAMENTE ISSO, A NAO SER QUE QUISERMOS INTEGRAR COM OUTROS SISTEMAS PARA TENTAR PROTEGER UM POUCO MAIS.



## Probes

### Entendendo o health check

É uma forma para verificarmos se a nossa aplicação está saudável e se nao estiver, emos que tomar uma ação. Uma aplicação e o ciclo de vida dela sao bem distintos. Por isso temos que entender que quando estamos trabalhando com o health check, precisamos entender o moento da nossa aplicação. Isso vai ajudar a garantir que ela fique sempre dispionível.



Se a naplicação está funcionando no ar e está tudo ok e funcionando, precisamos de um mecanismo que a verifique. Se nao estiver, o que podemos fazer? gERALMENTE NO K8S REINICIAMOS A APLICAÇÃÕ DO CONTAINER PARA QUE ELA VOLTE AO NORMAL.

Por outro lado, existe um outro fator. Quando estamos subundo a nsossa aplicação pela primeira vez, como sabemos que ela está´pronta para subir p´ára produção?

Por exemplo quando subimos uma banco de dados, o db demora as vezes uns 10 segundos para iniciar. Nesse momnento em que o banco de dados está iniciando, nao queremos jogar trafego para esse cara pq que cair nesse pod vai se dar conta de um erro. 

Entao temos que garantir que só vamos mandar o trafego quando a nossa aplicação estover pronta.

Entao, nesse momento tmemos dois pontos impiortantes: quando a aplicação está´no ar e quando ela ainda está sendo inicializada.



Agora um outro ponto:

Vamos imaginar que aplicação está no ar e esta tudo funcionando e essa aplicação ela de problema. Obviamente que podemos reiniciar o processo. Mas antes de reiniciar o processo, precisamos nao mais redirecionar para aquele determinado Pod pq senao vai contuar dando erro.

É a histroia de sabemos que está com problema e continuar enviando o trafego de problema.

Nesse caso, temos que parar de enviar o trafego e reiniciar o nosso container.

Entao vamos falar de liveness probe, readyness probe e startup probe. 

Esses assuntos sao extremamente importantes e tem que tomar cuidado para nao cairmos em cilada e nao cair em erros que aidna 



### Criando endpoint Healthz

Vamos alterar o server.go para gerar um problema. De tempos em tempos vamos verificar a saude da nosssa apklicação

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
	"time"
)

var startedAt = time.Now()

func main() {
	http.HandleFunc("/healthz", Healthz)
	http.HandleFunc("/secret", Secret)
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func Secret(w http.ResponseWriter, r *http.Request) {
	user := os.Getenv("USER")
	password := os.Getenv("PASSWORD")
	fmt.Fprintf(w, "User: %s. Paswword: %s.", user, password)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("/go/myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}

func Healthz(w http.ResponseWriter, r *http.Request) {

	duration := time.Since(startedAt)

	if duration.Seconds() > 25 {
		w.WriteHeader(500)
		w.Write([]byte(fmt.Sprintf("Duration: %v", duration.Seconds())))
	} else {
		w.WriteHeader(200)
		w.Write([]byte("ok"))
	}
}

```



A ideia do programa acima é funcionar normalmente, mas depois de 25  segundos ela vai começar a gerar um erro (500) para testarmos algumas coisas.

Vamos gerar um buld agora com a versao 5.3, dar um push no docker hub e dar um deployment com essa nova versao.

```bash
❯ docker build -t rogeriocassares/hello-go:v5.3 .
❯ docker push rogeriocassares/hello-go:v5.3
```



deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.3"
          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



Vamos aplicar o deployment

```bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured

❯ kubectl get po
NAME                       READY   STATUS    RESTARTS   AGE
goserver-df7547bb9-9w57h   1/1     Running   0          26s
```



Nesse caso, o pod continua rodando pois a aplicação do k8s não sabe que ela está com problema!

Vamos acessar a aplicação

```bash
❯ kubectl port-forward svc/goserver-service 9000:8080
Forwarding from 127.0.0.1:9000 -> 8000
Forwarding from [::1]:9000 -> 8000
```

http://localhost:9000/healthz



E percebemos que agora há um erro no console do navegador!

```bash
Failed to load resource: the server responded with a status of 500 (Internal Server Error)
```



Aqui entra o liveness probe Ele irá verificar de tempos em tempos para verificar se a aplicação está saudável.

Então temos um problema, um endpois para verificar eesse problema mas ainda assim o k8s nao sabe dessa existencia, pois para ele o container está riodando entao deixa funcionar!





### Liveness na prática

Vamos usar os proprios recusrsos do k8s com o livenessprobe

No deployment.yaml, na parte do container usando a porta do container e nao do service:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.3"
          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```

Temos 3 tipos pricipais. HTTP, COMMAND, TCP.

Geralmente o mais utilizado e a parte do http

Alguns testes podem ser feitos para tb testar coisas mais integradas, ao invés de ser nossa aplicação pode ser a integração do banco de dados. Nesse caso, um sistema integrado pode ser mais conservador e colocar um timeoutSconds maior. APenas a aplicação deve responder bem rápido.

SUCCESStHESHOLD. qUANTAS VEZES TEM QUE TESTAR PARA DAR CERTO.

E podemos verificar essa aplicação de maneira online com o comando watch

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po
deployment.apps/goserver configured

Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 11:16:45 2022

NAME                        READY   STATUS    RESTARTS   AGE
goserver-7869cb67f7-p2qx7   1/1     Running   0          33s
```



Perceba que após 50 segundos, o container já foi reiniciado 2 vezes por causa da configurações que havíamos feito no deployment.

```bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 11:17:24 2022

NAME                        READY   STATUS    RESTARTS      AGE
goserver-7869cb67f7-p2qx7   1/1     Running   2 (11s ago)   72
```

Agora vamos alterar o failure Thresold para 3. Isso significa que precisa testar 3 vezes pelo menos, ai reincia o container

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.3"
          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 3
            timeoutSeconds: 1
            successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



Vamos testar:

```bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 11:28:53 2022

NAME                        READY   STATUS    RESTARTS      AGE
goserver-6dc5fc9dc5-g7whd   1/1     Running   1 (17s ago)   58s
```



Funcionou! Entao ele tenta 3 vezes para reiniciar o container.



Para usar esse recurso, ao tem tando segredo, mas esse recurso em conjunto com outros pode tornar um pouco mais complexo.



Se quisermos verificar o histórico de restar do container, podemos pedir para descrever o pod:

```bash
❯ kubectl get po   
NAME                        READY   STATUS    RESTARTS       AGE
goserver-6dc5fc9dc5-g7whd   1/1     Running   6 (107s ago)   5m53s


rogerio in FullCycle/3.0/4.K8s 
❯ kubectl describe po goserver-6dc5fc9dc5-g7whd
Name:         goserver-6dc5fc9dc5-g7whd
Namespace:    default
Priority:     0
Node:         k3d-fullcycle-server-0/192.168.96.2
Start Time:   Thu, 28 Jul 2022 11:27:56 -0300
Labels:       app=goserver
              pod-template-hash=6dc5fc9dc5
Annotations:  <none>
Status:       Running
IP:           10.42.0.132
IPs:
  IP:           10.42.0.132
Controlled By:  ReplicaSet/goserver-6dc5fc9dc5
Containers:
  goserver:
    Container ID:   containerd://ad3ffe74e6e3dde37e21be2ac13a8e56b465191af447ca87dbc0cbc5a8e85d73
    Image:          rogeriocassares/hello-go:v5.3
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:5cf03363ba6784f729b800bb7e32a357c8acf9a4b56811424519152ee1ab1bad
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    2
      Started:      Thu, 28 Jul 2022 11:33:35 -0300
      Finished:     Thu, 28 Jul 2022 11:34:11 -0300
    Ready:          False
    Restart Count:  6
    Liveness:       http-get http://:8000/healthz delay=0s timeout=1s period=5s #success=1 #failure=3
    Environment Variables from:
      goserver-env     ConfigMap  Optional: false
      goserver-secret  Secret     Optional: false
    Environment:       <none>
    Mounts:
      /go/myfamily from config (ro)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-crxvr (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      configmap-family
    Optional:  false
  kube-api-access-crxvr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  7m29s                  default-scheduler  Successfully assigned default/goserver-6dc5fc9dc5-g7whd to k3d-fullcycle-server-0
  Normal   Pulled     7m26s                  kubelet            Successfully pulled image "rogeriocassares/hello-go:v5.3" in 3.437009418s
  Normal   Pulled     6m47s                  kubelet            Successfully pulled image "rogeriocassares/hello-go:v5.3" in 1.896422462s
  Normal   Pulled     6m7s                   kubelet            Successfully pulled image "rogeriocassares/hello-go:v5.3" in 2.055890405s
  Normal   Created    6m7s (x3 over 7m26s)   kubelet            Created container goserver
  Normal   Started    6m7s (x3 over 7m25s)   kubelet            Started container goserver
  Warning  Unhealthy  5m29s (x9 over 6m59s)  kubelet            Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    5m29s (x3 over 6m49s)  kubelet            Container goserver failed liveness probe, will be restarted
  Normal   Pulling    5m29s (x4 over 7m29s)  kubelet            Pulling image "rogeriocassares/hello-go:v5.3"
  Warning  BackOff    2m20s (x7 over 3m24s)  kubelet            Back-off restarting failed container
```

Percebemos que a livenessprobe falhou, deu um estado de unhealthy e subiu novamente.

### Entendendo Readiness

O Readiness verifica quando a aplicação está pronta para receber trafego e é um dos pontos principais quando estamos desenvolvendo uma aplicação. As vezes nossa aplicação, na hora que o container sobe, ela pode nao estar 100% pronta pois os processos ainda podem estar rodando, uma carga pode estar acontecendo, ele pode estar ainda subindo um monte de processos,  o banco de dados pode estar  se conectando ... Pode estar acontecendo um monte de coisas que demora, uma aplicação legada, enfim. Entao percebemos que nao podemos mandar trafego para uma aplicação que ainda nao está pronta. Por conta disso temos o readiness.

O readiness verifica se a aplicação está pronta. Como faemos isso nesse momento?

Vamos mudar no server.go para que se a duração da nossa aplicação for menor que 10, isto é, pelo menos 10 segundos para iniciar, vai dar problema falando que aplicação ainda nao esta pronta. Enquanto isso, nao queremos qu eo trafego ainda seja enviado para a aplicação.

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
	"time"
)

var startedAt = time.Now()

func main() {
	http.HandleFunc("/healthz", Healthz)
	http.HandleFunc("/secret", Secret)
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func Secret(w http.ResponseWriter, r *http.Request) {
	user := os.Getenv("USER")
	password := os.Getenv("PASSWORD")
	fmt.Fprintf(w, "User: %s. Paswword: %s.", user, password)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("/go/myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}

func Healthz(w http.ResponseWriter, r *http.Request) {

	duration := time.Since(startedAt)

	if duration.Seconds() < 10 {
		w.WriteHeader(500)
		w.Write([]byte(fmt.Sprintf("Duration: %v", duration.Seconds())))
	} else {
		w.WriteHeader(200)
		w.Write([]byte("ok"))
	}
}

```



Vamos gerar  o build, push e aplicar o deployment

```bash
❯ docker build -t rogeriocassares/hello-go:v5.4 .
❯ docker push rogeriocassares/hello-go:v5.4
```

Deployment. Vamos comentar o live e apenas verificar

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.4"
          # livenessProbe:
          #   httpGet:
          #     path: /healthz
          #     port: 8000
          #   periodSeconds: 5
          #   failureThreshold: 3
          #   timeoutSeconds: 1
          #   successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```

Apply e verificar

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po
```

Podemos ver que aqui, mesmo que a aplicação ainda nao estivesse pronta, o trafego já estava sendo enviado para a aplciaçção e isso nao pode acontecer.

Vamos       deletar o deployment e aplicar de novo com um port forward e verificar como a nossa aplicação ainda nao esta pronta.

```bash
❯ kubectl delete deploy goserver
deployment.apps "goserver" deleted

❯ kubectl apply -f k8s/deployment.yaml               

❯ kubectl port-forward svc/goserver-service 9000:8080
```

Acessar http://localhost:9000/healthz

Pronto. se vissemos na pagina, vemos que o ok aparece apenas após os 10 segundos, indicando que a aplicação está funcionando, Mas o nosso desejo é que as pessoas apenas acessem a nossa aplicação quando ela estiver pronta! Por conta iddso existe o readiness.

O readiness faz exatamente o que o liveness faz, mas par fazer a verificação que a aplicação está pronta. O readiness tb vai fazer a conulta na porta 8000 a cada tanto tempo, e podemos configurar para que se deu problema 1 vez, a nossa aplicação nao esta pronta. o success ailure já sabemos que é 1, e o timeout seconds, por default tb é 1.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.4"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 3

          # livenessProbe:
          #   httpGet:
          #     path: /healthz
          #     port: 8000
          #   periodSeconds: 5
          #   failureThreshold: 3
          #   timeoutSeconds: 1
          #   successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



​	e	 entao vamos aplicar e rodar a verificação!

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po

Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 12:49:53 2022

NAME                        READY   STATUS    RESTARTS   AGE
goserver-8f445fcd8-j949n    1/1     Running   0          6m33s
goserver-6b4f59b7d4-vjlm6   0/1     Running   0          5s

```

Aqui vimos que mesmo após criado, o Pod ainda não está Ready, mas após os 3 + 3 + 3 + 3 segundos ele passa a subi. Observe:

```bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 12:50:13 2022

NAME                        READY   STATUS    RESTARTS   AGE
goserver-6b4f59b7d4-vjlm6   1/1     Running   0          24s
```

O readiness espera até que o container esteja pronto para começar a mandar o tráfego! Enquanto nao mandar o tráfego significa que o Pod ainda não está pronto.

Uma oitra coisa muito importante  é perceber que o Pod anterior ainda estava disponível até que o novo estivesse pronto para que entao pudesse ser terminado para a aplicação nao ter downtime e essa forma é como o k8s trabalha.

Vejamos, nesse nosso caso, sabemos que a aplicação demora pelo menos 10 segundos para ficar pronta. Entao nao precisamos ficar pedindo, para que, nesses 10 segundos iniciais, o k8s fique enviando tráfego para a aplicação pq nao adianta.

Uma opção que tb funciona para o liveness é o intitialDelaySeconds, que é um offset de tempo para começar a verificar. Então o periodo que o k8s vai começar a fazer as requisiçoes vai ser apenas esse initialDelaySeconds.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.4"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          # livenessProbe:
          #   httpGet:
          #     path: /healthz
          #     port: 8000
          #   periodSeconds: 5
          #   failureThreshold: 3
          #   timeoutSeconds: 1
          #   successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



Vamos verificar:

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po

Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 13:04:30 2022

NAME                        READY   STATUS    RESTARTS   AGE
goserver-6b4f59b7d4-vjlm6   1/1     Running   0          14m
goserver-f44f9df49-qg6zt    0/1     Running   0          5s
```



E depois de uns 13 ou 14 segundos ele sobe normalmente.

````bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 13:04:58 2022

NAME                       READY   STATUS    RESTARTS   AGE
goserver-f44f9df49-qg6zt   1/1     Running   0          33s

````



Funcionou! Entao podemos esperar um pouquinho para começar a subir as nossas probes!



### Combinando Liveness e Readiness Probes

Vamos descomentar a parte de livenessProbe no yaml

Assim, vamos esperar 10 segundos para o readiness e o live vai verificar o processo a cada 5 segundos.

Mas vamos pensar o seguinte: Restartar o nosso processo toda a vez que der um erro: failureThreshold: 1

deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.4"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```

Vamos aplicar e observar:

```bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 14:33:03 2022

NAME                        READY   STATUS             RESTARTS     AGE
goserver-f44f9df49-qg6zt    1/1     Running            0            88m
goserver-78d5d7b499-lmrxj   0/1     CrashLoopBackOff   2 (9s ago)   24s
```

O que acontece aqui é o seguinte: O container fica reinciando sem parar e gera um monte de problemas.

epois de um tempo em running, ele aguarda ficar pronto mas reinicia de novo!

Isso acontece porque o readiness tem que aguardar 10 seundos para garantir que o container está pronto e entao o container só vai ficar pronto READY 1/1 quando receber essa resposta do readness, que funcionou.

No liveness, vai verificar a cada 5 segundos se o container está no ar. Se nao estiver, ele reinicia.

Nesse caso, não deu nem tempo do readiness realmente ficar pronto que ele já reiniciou o container através do liveness.

Entao toda a vez que esta para o readiness ficar pronto, o liveness vai lá e reinicia o container, matando o container e subindo de novo.

Por isso, precisamos sempre ter em mente que o readiness tem que funcionar em algum momento, pq se ele nao funcionar, o liveness vai reiniciar o container e o readinesse nunca vai ficar pronto. Esse é um ponto extremamanete importante para se porestar atenção!

A primeira coisa entao, seria adicionar um initialDelaySeconds tb no livenessProbe. Porque o liveness só iria começar a contar após os 10 segundos e provavelmente teriamos tempo para o readiness ficar pronto.

O rediness testa de 3 em 3 segundos. E o liveness testa a cada 5 segundos.. Mas quando ficar pronto, ambos vão testar ao mesmo tempo. Vamos ver se vai dar tempo de um readiness ficar pronto para o liveness poder testar.



Vamos ver:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.4"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            initialDelaySeconds: 10

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



E aplicamos o deployment:

```yaml
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 15:03:29 2022

NAME                       READY   STATUS    RESTARTS   AGE
goserver-f44f9df49-qg6zt   1/1     Running   0          119m
goserver-c7b4666f8-bfh4t   0/1     Running   0          7s
```

E funcionou!

```bash
Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 15:05:09 2022

NAME                       READY   STATUS    RESTARTS      AGE
goserver-c7b4666f8-bfh4t   1/1     Running   4 (66s ago)   107s
```





Outra coisa: o readiness nao verifica apenas na inicialização do container. Ele fica a cada 3 segundos vendo que o container está READY mesmo após ter sido iniciado. Ele nao quer ver se está live, mas READY. Se nao estiver Ready ele desvia o trafego enquanto oliveness tentra reinciar.

Entao, o readyness tira o trafego fora. O liveness recria o processo. 



Vamos ver o server.go. Estamos demorando 10 segundos para subir a aplicação. Mas vamos fazer algo para que quando a nossa aplicação ficar mais de 30 segundos no ar, ela tb vai ter um erro.

Entao: inicio (10segundos) -> READY (+20) -> Erro (aplicaão não funcionando corretamente)

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os"
	"time"
)

var startedAt = time.Now()

func main() {
	http.HandleFunc("/healthz", Healthz)
	http.HandleFunc("/secret", Secret)
	http.HandleFunc("/configmap", ConfigMap)
	http.HandleFunc("/", Hello)
	http.ListenAndServe(":8000", nil)
}

func Hello(w http.ResponseWriter, r *http.Request) {
	name := os.Getenv("NAME")
	age := os.Getenv("AGE")
	fmt.Fprintf(w, "Hello, I am %s. I am %s.", name, age)
}

func Secret(w http.ResponseWriter, r *http.Request) {
	user := os.Getenv("USER")
	password := os.Getenv("PASSWORD")
	fmt.Fprintf(w, "User: %s. Paswword: %s.", user, password)
}

func ConfigMap(w http.ResponseWriter, r *http.Request) {

	data, err := ioutil.ReadFile("/go/myfamily/family.txt")
	if err != nil {
		log.Fatalf("Error reading file: ", err)
	}
	fmt.Fprintf(w, "My family: %s.", string(data))
}

func Healthz(w http.ResponseWriter, r *http.Request) {

	duration := time.Since(startedAt)

	if duration.Seconds() < 10 || duration.Seconds() > 30 {
		w.WriteHeader(500)
		w.Write([]byte(fmt.Sprintf("Duration: %v", duration.Seconds())))
	} else {
		w.WriteHeader(200)
		w.Write([]byte("ok"))
	}
}

```



Vamos fazer o build, o push e o deploy do yaml

```bash
❯ docker build -t rogeriocassares/hello-go:v5.5 .
❯ docker push rogeriocassares/hello-go:v5.5      
```

Vamos subir o readiness sozinho:

deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.5"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          # livenessProbe:
          #   httpGet:
          #     path: /healthz
          #     port: 8000
          #   periodSeconds: 5
          #   failureThreshold: 1
          #   timeoutSeconds: 1
          #   successThreshold: 1
          #   initialDelaySeconds: 10

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```

Vamos aplicar o deployment e observar. O comportamento que esperamos é que demore cerca de 14 segundos para a aplicação ficar READY após ter sido verificada pelo readiness, depois de 30 segundos, ele vai ver que  a imagem está fora pois a aplicação deu erro eo Pod vai ficar com o status NAO READY!

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po

Every 1,0s: kubectl get po                        rogerio-pc: Thu Jul 28 17:46:06 2022

NAME                        READY   STATUS    RESTARTS   AGE
goserver-57755dc668-5wbl4   0/1     Running   0          2m30s
```



Ou seja, o Pod está rodando ma so tráfego não está sendo enviado para ele nesse momento. porque o readiness não deixando! 

Nesse caso, para voltar a funcionar, o liveness é quem reinicia o Container.

Entao, enquando o Readiness verifica o Container e desabilita ele, o liveness verifica que está fora e reinicia para habilitá-lo novamemte.

Enão, nesse momento, vamos reativar o liveness e vamos ver o que vai acontcer.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.5"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            initialDelaySeconds: 10

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```

aplicando e observando.

```bash
ubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po
deployment.apps/goserver configured

```

então o que aconteceu foi o seguinte: a aplicação demorou 10 segundos para subir e então o readiness habilitou o status com READY. depois de 30 segundos, a aplicação começou a dar um erro 500 novamente, ambos perceberam que esta fora do ar, e entao o readiness parou o Pod, deixando ele como NAO READY.

O liveness entao ficou observando que não existia nada na requisição entao reiniciou o container que após 10 segundos ficou como Ready pelo readiness e etc . Eles nao estao conseguindo se reconciliar pois cada um tem uma taxa de intervalo!

Provavelmente, entao temos que aumentar o tempo de inicialização do liveness para que, quando reiniciar o container, exista tempo para o readiness subir no ar!

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.5"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



E aplicamos o deployment, observando-o

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po
```

READY após 10s, nao reay após 33 segundos, e reinciado,. Após 45 segundos READY novamente!

E quando nao sabemos quanto tempo demora a nossa aplicação? 10s, 20s um minuto ... há uma variação muito grande! 

E se demorar um minuto para subir a aplicação e ela subir em 10segundos?

Precisariamos esperar + 50 segundos até um novo ciclo acontecer para mandadrmos trafego para a ossa aplicação.



### Trabalhando com o startupProbe

O startupProbe surgiu a partir da versao 1.6 do k8s que auxilia no  trade-off entre o livesness e o   readiness.

O startupProbe funciona como se fosse o readiness mas somente no processo de inicialização.

O startupProbe fica fazendo a verificação até ele ficar pronto. Quando ele ficou pronto, aí sim o readness e o liveness começam a acontar. Entao nao temos mais o problema inicial citado anteriormente.

Nesse caso, temos que tomar cuidado principalmente com uma coisa. Desde o inicio podemos até colocar um initialDelay. De acordo com o periodScecond3, a cada 3 segundos ele vai tentar fazer o processo de verificação com o failureThreshold de 1. Entao, a cada 3 segundos ele vai fazer a verificação e entao o starttuProbe ele vai parar e o container nunca vai ficar pronto.

Se colocarmos heresold  10, significa que a cada tres segunos ele vai tentar uma vez, durante 10 vezes. Ou seja, ele vai tentar pelo menos por 30 segundos. É só a partir desse tempo que o liveness e o readiness são liberados para começarem a operar.

E isso é o mais interessante, pq se a aplicação demora até dois minutos ou mais para funcioonar, o strartupProbe vai ficar pergunto se a aplicação está pronta ou não. Assim que estiver pronto ele já libera!

No caso de utilizar o startupProbe, o initialDelaySeconds pode ser removido do lieness e do readiness.pq o proprio startupProbe já verificou se a aplicação está pronta para rodar.



```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.5"
          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"


```



Vamos dar o deploy e o watch

```bash
❯ kubectl apply -f k8s/deployment.yaml&& watch -n1 kubectl get po
```

Nesse momento, o container deve estar funcionando tranquilamente e provavelemte teremos o problema após os 30 segundos que não vai ficar mais pronto por causa do readness e o liveness vai reiniciar o container.

Agora, vamos ter o startup até os 90 segundos. Assim que ele testar que a aplicação ficou pronta, ele vai novamente liberar para o readiness e para o liveness novamente.

O startup verifica se está tudo ok, 

O readiness tira isso do trafego

O liveness reinicia o container

O startup verifica se está tudo ok para iniciar o processo e quando tiver ele libera o trafego novamente para nós.

Ẽ SEMPRE RECOMENDADO USARMOSO STARTUP PROBE PQ ELE Ẽ QUEM VAI FALAR A PARTIR DE QUANDO O LIVE E O READINESS VAI PODER SER UTILIZADO.



NAO DEIXAR DE VISITAR A DOC DO K8S PQ TEM MUITOS DETALHES!





## Resources e HPA

### Instalando o metrics -server

Com o k8s podemos sempre aumentar a quantidade de replicas e pods. Ma para que vamos aumentar ? QUanto deconsum e recursos cada pod vai consumir?

Quantosprecisamos para escalar? Ese um sistema tiver muito mais consumo que outro. COmoisso consege funcionar?

QUandotrabalamos comnuvem, tem o autoscaling das instancias queprecisamos de acordo com o consumo de recursos. Mas e seno soubermos oslimitesque devem atingr paracomeçcar a escalar todos esses pontos.



para isso, existe o HPA - HORIZONTAL PODS AUTOSCALE.eUSAMOS ELE COM O METRICS-SERVER. Que coletaas metricas em tempo real de quanto de recurso cada por esta consumindo emdeterminado momento.

Se quisermos salvar essas infos e utilizar las para tomaroutrasdecisoes, nos podemos. Inclusive, uma coisa muito comum para fazer isso eh o PROMETHEUS. Que gera dashboard no GRafana e nos ajuda sobre isso.

Normalmente com k8s na nuvem (gereniado), esse metrics server jã vem instalado por padrao, mas no kind nao vem. 

Por padrao, o metric server exige uma conexao segura entre todos os clusters. E precisa trabalarcom TLS. Mas o grande ponto é que vamos fazer isso com um bypass no TLS para rodarmos em ambientes de dev.



Vamos acessar o repositorio do k8s metrics-server

https://github.com/kubernetes-sigs/metrics-server

Ele é escalavel ateh 5000 node no k8s.

Na doc ee preve um deployment padrao, mas ainda ẽ necessãrio uma configura~cao para que ele funcione no kind para dev/OPs

Vamos baixar esse arquivos para o nosso diretrio do k8s:

```bash
❯ cd k8s
❯ wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Foi realizadoo download para o arquivo components.yaml

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    k8s-app: metrics-server
    rbac.authorization.k8s.io/aggregate-to-admin: "true"
    rbac.authorization.k8s.io/aggregate-to-edit: "true"
    rbac.authorization.k8s.io/aggregate-to-view: "true"
  name: system:aggregated-metrics-reader
rules:
- apiGroups:
  - metrics.k8s.io
  resources:
  - pods
  - nodes
  verbs:
  - get
  - list
  - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    k8s-app: metrics-server
  name: system:metrics-server
rules:
- apiGroups:
  - ""
  resources:
  - nodes/metrics
  verbs:
  - get
- apiGroups:
  - ""
  resources:
  - pods
  - nodes
  verbs:
  - get
  - list
  - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server-auth-reader
  namespace: kube-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: extension-apiserver-authentication-reader
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server:system:auth-delegator
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:auth-delegator
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    k8s-app: metrics-server
  name: system:metrics-server
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:metrics-server
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
---
apiVersion: v1
kind: Service
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server
  namespace: kube-system
spec:
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: https
  selector:
    k8s-app: metrics-server
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server
  namespace: kube-system
spec:
  selector:
    matchLabels:
      k8s-app: metrics-server
  strategy:
    rollingUpdate:
      maxUnavailable: 0
  template:
    metadata:
      labels:
        k8s-app: metrics-server
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        image: k8s.gcr.io/metrics-server/metrics-server:v0.6.1
        imagePullPolicy: IfNotPresent
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /livez
            port: https
            scheme: HTTPS
          periodSeconds: 10
        name: metrics-server
        ports:
        - containerPort: 4443
          name: https
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /readyz
            port: https
            scheme: HTTPS
          initialDelaySeconds: 20
          periodSeconds: 10
        resources:
          requests:
            cpu: 100m
            memory: 200Mi
        securityContext:
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          runAsNonRoot: true
          runAsUser: 1000
        volumeMounts:
        - mountPath: /tmp
          name: tmp-dir
      nodeSelector:
        kubernetes.io/os: linux
      priorityClassName: system-cluster-critical
      serviceAccountName: metrics-server
      volumes:
      - emptyDir: {}
        name: tmp-dir
---
apiVersion: apiregistration.k8s.io/v1
kind: APIService
metadata:
  labels:
    k8s-app: metrics-server
  name: v1beta1.metrics.k8s.io
spec:
  group: metrics.k8s.io
  groupPriorityMinimum: 100
  insecureSkipTLSVerify: true
  service:
    name: metrics-server
    namespace: kube-system
  version: v1beta1
  versionPriority: 100

```

E entao vamos renomea-lo para metric-server.yaml

No campo deployment desse arquivo, ele passa alguns argumentos ao criar o container.

```yaml
containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
```



UM dos args quepreceisamos passar é o --kubectl-insecure-tls

```yaml
containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
```



Entao, agora ele vai permitir rodarmos como odeulo TLS inseguro para que nao precisemos trabalhar como TLS

Agora, vamos aplicar o metrics server

```bash
❯ cd ../
❯ kubectl apply -f k8s/metric-server.yaml                                                        
serviceaccount/metrics-server unchanged
clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader unchanged
clusterrole.rbac.authorization.k8s.io/system:metrics-server unchanged
rolebinding.rbac.authorization.k8s.io/metrics-server-auth-reader unchanged
clusterrolebinding.rbac.authorization.k8s.io/metrics-server:system:auth-delegator unchanged
clusterrolebinding.rbac.authorization.k8s.io/system:metrics-server unchanged
service/metrics-server unchanged
deployment.apps/metrics-server configured
apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io unchanged
```

E agora,  como sabemos que o metricserver esta funcionando?

O k8s temalgo chamadode kubectl get apiservices, que mostra todos os serviços que temos disponveis na API e podemos observar que agora temos o kube-system/metrics-server, que é exatamente para ver se esta tudook e se a coluna available estivecom true, ẽ quedeu tudo certo a nossa instalação:

```bash
❯ kubectl get apiservices
NAME                                   SERVICE                      AVAILABLE   AGE
v1.admissionregistration.k8s.io        Local                        True        30d
v1.apps                                Local                        True        30d
v1.authentication.k8s.io               Local                        True        30d
v1.                                    Local                        True        30d
v1.apiextensions.k8s.io                Local                        True        30d
v1.authorization.k8s.io                Local                        True        30d
v1.autoscaling                         Local                        True        30d
v2.autoscaling                         Local                        True        30d
v2beta1.autoscaling                    Local                        True        30d
v2beta2.autoscaling                    Local                        True        30d
v1.batch                               Local                        True        30d
v1beta1.batch                          Local                        True        30d
v1.certificates.k8s.io                 Local                        True        30d
v1.coordination.k8s.io                 Local                        True        30d
v1beta1.discovery.k8s.io               Local                        True        30d
v1.discovery.k8s.io                    Local                        True        30d
v1beta1.events.k8s.io                  Local                        True        30d
v1.events.k8s.io                       Local                        True        30d
v1beta1.flowcontrol.apiserver.k8s.io   Local                        True        30d
v1beta2.flowcontrol.apiserver.k8s.io   Local                        True        30d
v1.networking.k8s.io                   Local                        True        30d
v1.node.k8s.io                         Local                        True        30d
v1beta1.node.k8s.io                    Local                        True        30d
v1.policy                              Local                        True        30d
v1beta1.policy                         Local                        True        30d
v1.rbac.authorization.k8s.io           Local                        True        30d
v1.scheduling.k8s.io                   Local                        True        30d
v1.storage.k8s.io                      Local                        True        30d
v1beta1.storage.k8s.io                 Local                        True        30d
v1.k3s.cattle.io                       Local                        True        3h41m
v1alpha1.traefik.containo.us           Local                        True        3h41m
v1.helm.cattle.io                      Local                        True        3h41m
v1beta1.metrics.k8s.io                 kube-system/metrics-server   True        21s
```



E foi!

Agora, estamos prontos para irms paraonossoproximo passo para trabalhasrmoscom anossa aplica~caocom gerenciamento de recursos, fazer escalonamento autoatico e muitomais!



### Entendendoutilização de Resources

Apos o metric server funcionando, vamos enntender o seguinte conceito  para utilizarmos em qualquer Pod que subirmos no k8s. Se nao usarmos isso, O Pod pode começcar a consumir todos os recursos do node/clustere ehpor conta disso que recisamos configurar os recursos do nossosistema. O quanto cada pod precisa para rodar e ateh onde é o limite que permitimos ele consumir os recursos dos nossos nodes. 

No arquivo deploymen.yaml, vamos adicionar, antes da startuProbe, a tag resources:

As requsets siginifica o quanto que o sistema que estamos trabalhando exige comoo mĩnimo para funcionar. As vezes vamos usar um sistema com s requisitos minimos necessarios para funcionar tanto em termos de cpu e memoria.

CPU tem uma unidade de medida e chamamos de vCPU. A CPU possui 1000 milicores. Ou seja, o quanto se consegue usar de uma CPU. Podemos falar que a aplicação exisge 500m, Isso significa metade de uma vCPU inteira! Entao podemos utilizar essaunidade de medida ou em porcentagem, como 0.5, por exemplo. 



Nesse caso, vamosutilizar 100m para funcionar. Quanto à memoria, estamos precisando utilziar 20MB de memõria. E como chgamos nesses numeros?

Nao tem como chegarmos nesses numeros sem testarmos. Precisamos fazer benchmarks e testes de stress para chegarmos no melhor numero possivel aqui!

Vamos falar disso tb!

POrtanto os valores aqui aplicados é o MÍNIMO que a aplicação precisa para rodar. Quando configuramos este valor, estamos "sequestrando"/RESERVANDOestes recursos donosso cluster para o nosso Pod.

Entao quando falamos que precisamos de 100milicires, o Pod vai pegar qual node que o cluster tem que disponiblize esses 100m, Entao vai ficar reservado para tal POd de acordo com o Scheduler do k8s. 

E se colocarmos um valor absurdo e nao ivermos máquina/recuro o suficiente no cluster para esse processo? Entao quando esse pod for criado ele vai ficar como pendente, esperando o tempo todo algum node/maquina teraquela capacidade para ele poder provisionar.

Quando falamosde minimo, ele vai reservar nosistemaentao ninuem pode modificar ou utilizar o espaço jã reservado no sistema.



o LImits, é at~w onde o Pod pode utilizar de recursos no cluster. Nesse caso, colcoamsoo minimo de 100m, mas as vezes vamos trer um pico de acesso e /ou a aplicação vai cair quando acessarmos muito esse cara.

Apesar de já termos reservado 100m, até onde deixamos esse cara consumir no servidor? Se deixarmos ateh 500m, quando esse Pod começar a receber requisiçao ele vai ter garantido 100m, mas com muitos acesos, ele pode chegar até 500m. Mais que isso, ele nao vai usar nem vai ter perigo de derrubar outros recursos que estao acontencendo.

DISCLAIMER!

A NOSSA MÁQUINA/CLUSTER É FINITO E TEMOS UM NUMERO DE RECURSOS. IMAGINA QUE TEOMOS 3VCPUS E PARA A APLCIACAO FUNCIONAR COM 100m, ENTAO PODEROAMOS UTILIZAR 300 Pods que vai funcionar pq temos recursos. Maso nosso limite é 500m. Entao se pegarmos esse 500m, e multiplicar pela quantidade de vCPUS que temos, percebemos que ele vai estourar abruptamente. Entao aquivimos que o que reservamos de minimo ele tem recursos para utilizar, mas o limite que chegamos, se todos trabalharem no limite, a nossa maquina nao terá recusrsos para segurar. Entao a dica é sempretentar evitar que A SOMA DE TODOS OS LIMITES ULTRAPASSE A QUANTIDADE DISPONIVEL DE RECURSOS NO NOSSO CLUSTER. Mesmo que esses limites nao sejam utilizados ao mesmo tempo, entao vamos gastar dinheiro para comprar mais maquinas e a máquina vai ficar ociosa pq nao estamos sempre no limite, entao, esse cao, vamos apostar mais umenos como overbooking. Vender mais passagens que cabe no voo pq acreditamos quenem todo mundo vai comparecere se comparecer fazemos um sorteio ara alguem deisitir e entao resolve-se o problema.

Pode-se tentarpegar uma média e deixamos que a soma doslimites vao o mãximo em tanto para podermos ter algo seguro e equilibrio enteconsumo de máquina, ociosidade e a utilização. Entao temos um limite de CPU que podesubir e descer.

Já no caso da memoria, nao eh tao parecido pq memõria ẽ algo bem fixo. Nao podemos passar de tanto limite, pq cpu as vezes consegumos estourar um pouquinho para amis ou para menos e a máquina consegue resolver asituação. Jã namemória essa situação nao pode ser resolvida tao facilmente pq memoria é algo que é muito limitada. Nao conseguimos passar um pouqinho mais em relaçao aquela parte da memoria.Aqui vamoscolocar o minimo de 20 e max de 25.

Lembrando que se tivermos 10 replicas dessa aplicaçao, vamos estar consumindo NO MÍNIMO 1 vCPU e no máx, 5vCPUscaso tudo estoureo limite. Entao em nossa máquina operando no max, deveriamos tr 5vCPUs apenas para essa aplicação, fora o restante que deve ser consumido pelo OS do k8s, pelo master, pelo scheduler,pelos controles, pelos kubeletes, dns, etc que o k8s utiliza.

Agora vamoacriar e ajustar e vamos ver o consumo que ele está fazendo.

deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v5.5"

          resources:
            requests:
              cpu: "0.3"
              memory: 20Mi
            limits:
              cpu: "0.3"
              memory: 25Mi

          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 30
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```



### Aplicando deploment om resources

Vamos aplicar o arquivo de deployment

```bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured

❯ kubectl get po
NAME                        READY   STATUS    RESTARTS      AGE
goserver-5f949ccff9-pgbs6   1/1     Running   1 (29s ago)   64s
```





Agora que esta funcionando, uma das alternativas para vermos ele funcionando é o kubectl top com o nome do Pod.

```bash
❯ kubectl top pod goserver-5f949ccff9-pgbs6
NAME                        CPU(cores)   MEMORY(bytes)   
goserver-5f949ccff9-pgbs6   0m           1Mi 
```

Entao aqui ele vai mostrar como esta o nosso consumo e como ele esta em relacao à parte de memória.

Agora que temos essa opção conosco, vamos perceber que se começarmos a utilizar muito esse Pod, ele vai chegar no limite e vai começar a usar todo esse limite, nao vai mais conseguir servir as requisições e entao vai engasgar o nosso serviço e acontecer como em qualquer outra máquina.

Logo, uma vez que fazemos isso, estamos delimitando e para podermos escalar, basta, nesse caso, criarmos mais replicas!

A partir de agora estamos prontos para poder criarmos mais replias e conforme criamos mais replicas, e começamos a acessar os services, o k8s vai cmeçar a fazer o balanceamento de carga para que tenhamos uma distribuição mas igual para trabalharmos commais pods para suportar todos os acessos, mas mesmo assim limitando o uso de recursos dentrodo nosso CLuster!

## Criando e configurando um HPA

Afinal, como fazemos para escalar?

O HPA (hORIZONTAL POD AUTOSCALING) é o responsável por pegar aquelas metricas , verificando como está o trafego e como esta a cpu e memória. E baseado no estado de CPU ele vai começar a provisionar novas replicas para podermos trabalhar.

Isso é'muuuuito importante!

O HPA nao usa apenas a CPU como ponto de escala ou nao. Podemos utilizar mais metricxas ao mesmo tempo. Inclusive metricas customizadas. Por outro lado isso vai depender muito do tipo de aplicação e na maioria das vezes apenas o hpa de cpu vai funcionar.

Se algum dia precisamos de outra metrica para escalar que nao seja o hpa de cpu, bastra acessar a documentaçao do k8s.

Vamos ver agora como utilizar o hpa para nos auxiliar nos aspectos de escala!

Criamos um novo arquivo chamado hpa.yaml:

````yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: goserver-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    name: goserver
    kind: Deployment
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 30
````



Vamos aplicar o arquivo:

````bash
❯ kubectl apply -f k8s/hpa.yaml 
horizontalpodautoscaler.autoscaling/goserver-hpa created
````

Pronto! Agora vamos verificar o hpa

````bash
❯ kubectl get hpa
NAME           REFERENCE             TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   <unknown>/30%   1         5         1          88s
````

Agora, temos como referencia o Deployment/goserver e o target temos como 30% para começar a agir. Porem, nesse momento o valor de cpu utilizado esta como desconhecido. 

Como acabamos de criar, o k8s ainda demora de 1 a 2 minutos para conseguir criar as métricas no metric server.

````bash
❯ kubectl get hpa
NAME           REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   0%/30%    1         5         1          3m26s
````



Executando novamente o comando, agora já'estamois conseguindo pegar as metricas e nesse momento estamos utilizando 0% de cpu.



No proximo video, vamos usar uma ferramente para fazer o teste de estresse para acessar a aplicaçáo e verificar o hpa funcionado!



## Versao da imagem para o teste de stress

Na aula "Teste de stress com fortio" a imagem que está sendo utilizada é a rogeriocassares/hello-go:v9.7
que é diferente da versão v5.5 usada no deployment.yaml da aula "Aplicando deployment com resources".

O que muda é que na versão 5.5 o código do server.go gerava erro após 30 segundos de execução no pod.
Na versão 9.7 este erro após 30 segundos é removido e o teste de stress funcionará normalmente.

Versão 5.5 (rogeriocassares/hello-go:5.5)

````go
func Healthz(w http.ResponseWriter, r *http.Request) {
  duration := time.Since(startedAt)

  if duration.Seconds() < 10 || duration.Seconds() > 30 {
    w.WriteHeader(500)
    w.Write([]byte(fmt.Sprintf("Duration: %v", duration.Seconds())))
  } else {
    w.WriteHeader(200)
    w.Write([]byte("ok"))
  }
}
````



Na versão 9.7 (rogeriocassares/hello-go:v9.7)

````go
func Healthz(w http.ResponseWriter, r *http.Request) {
  duration := time.Since(startedAt)

  if duration.Seconds() < 10 {
    w.WriteHeader(500)
    w.Write([]byte(fmt.Sprintf("Duration: %v", duration.Seconds())))
  } else {
    w.WriteHeader(200)
    w.Write([]byte("ok"))
  }
}
````



Build uma nova imagem Dockerfile e push to the Docker Hub!

````bash
❯ docker build -t rogeriocassares/hello-go:v9.7 .
❯ docker push rogeriocassares/hello-go:v9.7
````



Portanto, para que você consiga acompanhar a aula "Teste de stress com fortio", utilize a
imagem `rogeriocassares/hello-go:v9.7 no deployment.yaml`.

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v9.7"

          resources:
            requests:
              cpu: "0.3"
              memory: 20Mi
            limits:
              cpu: "0.3"
              memory: 25Mi

          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 30
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

````

Aplique agora o novo deployment.yaml

````bash
❯ kubectl apply -f k8s/deployment.yaml 
````



## Atualização no comando do Fortio

Com a atualização do kubectl para a versão 1.21, o parâmetro --generator do comando

````bash
kubectl run -it --generator=run-pod/v1 fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service:8080/healthz"
````


passou a não ser mais suportado, apresentando o erro "Error: unknown flag: --generator".

Para a realização do teste, execute o comando sem este parâmetro, ficando da seguinte maneira:

````bash
$ kubectl run -it fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service:8080/healthz"
````


Com isto, será possível ver os pods escalando no teste de stress.



## Teste de stress com Fortio

Essa ferramenta é uma ferramena em go e nos ajuda a passar parametros e cria threads para fazer o acesso, a quantidade de queries por segundo, a quantidade de tempo qque queremos executar e a url que queremos trabalhar vamnos ver funcionand o.

O fortio tb tem uma imagem docker. Entao vamos criar um pod do fortio e quando esse pode for criado vamos pedir para ele gerar um teste de stress e tudo isso via comando para vermos como conseguomos executar uma operaçáo usando um pod e quando terminamos essa opereçao, ele mata o pod automaticamente. 

Vamos rodar um pod do kubernets via terminal como se fosse um docker! Vamos rodar um pod no terminal e remover a imagem assim que ele acabar e essa imagem virá do docker hub fortio/fortio.

````bash
❯ kubectl run -it fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service:8080/healthz"
````

Entao criamos uma nova aba no terminal e digitamos

````bash
 ❯ watch -n1 kubectl get hpa
 ----
 Every 1,0s: kubectl get hpa                            rogerio-pc: Wed Nov 16 15:46:10 2022

NAME           REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   6%/30%    1         5         1          65m
````

Dessa forma podemos perceber que, apesar de muitos acessos, o uso da CPU nao passou de 20%, e por isso nao conseguimos ver o hpa escalar para outros Pods.

Entao, vamos siminuir o valor de cpu por request no arquivo de `deployments.yaml` de 0.3 para 0.05 de cpu.

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v9.7"

          resources:
            requests:
              cpu: "0.05"
              memory: 20Mi
            limits:
              cpu: "0.05"
              memory: 25Mi

          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 30
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true

      volumes:
        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

````



E entao aplucamos novamentre o arquivo de deployment:

````bash
❯ kubectl apply -f k8s/deployment.yaml
````

Para conseguirmos visualizar o hpa funcionando

````bash
❯ kubectl run -it fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service:8080/healthz"
````



Entao, se vericarmos novamente o `kubectl get hpa`

````bash
❯ kubectl get hpa
----
NAME           REFERENCE             TARGETS
   MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   92%/30%
   1         5         4          80m
   
````

Vemos que foram criados 4 Pods com autoscaling!

`````bàsh
❯ kubectl get po
----
NAME                        READY   STATUS    RESTARTS   AGE
goserver-7fdf7d7865-7zzmj   1/1     Running   0          10m
fortio                      1/1     Running   0          2m11s
goserver-7fdf7d7865-6z4qm   1/1     Running   0          94s
goserver-7fdf7d7865-vzvmr   1/1     Running   0          64s
goserver-7fdf7d7865-klgwg   1/1     Running   0          64s
`````



Para descer as replicas, o kubectl espera um determinado tempo para diminuir.

Também é interessante usilizar uma ferramenta chamada k6.io para fazer teste de stress, pq da [ara fazer visita de varias paginas, pausar e etc.

Vamos fazer ainda um otro deste e configurar o nosso hpa para até 30 pods:

````yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: goserver-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    name: goserver
    kind: Deployment
  minReplicas: 1
  maxReplicas: 30
  targetCPUUtilizationPercentage: 25
````

Aplicando o novo arquivo:

````bash
❯ kubectl apply -f k8s/hpa.yaml 
````

E entao executamos em um terminal co comando 

````bash
❯  watch -n1 kubectl get hpa
````

e no outro o teste de estresse por 220s:

````bash
❯ kubectl run -it fortio --rm --image=fortio/fortio -- load -qps 800 -t 220s -c 70 "http://goserver-service:8080/healthz"
````



Após um tempo, vimos que o hpa ultrapassou o limite de 25% de CPU e copmeçou a criar novas replicas do Pod. 

````bash
Every 1,0s: kubectl get hpa                     rogerio-pc: Wed Nov 16 16:15:41 2022

NAME           REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   34%/25%   1         30        4          94m


Every 1,0s: kubectl get hpa                     rogerio-pc: Wed Nov 16 16:17:05 2022

NAME           REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
goserver-hpa   Deployment/goserver   26%/25%   1         30        4          95m

````

Pronto! Lembrando que há a espera inicial de 10 segundos para os Pods ficarem prontos! E 4 pods com 5 miliCores de cpu deram conta do recado!



## Statefulsets e Volumes Persistentes

### Entendendo volumes persistentes

Tudo o que fizemos até agora foi deploy  e escala de aplicaçáo que era totralemte stateless. Isto é, uma aplicaçao sem estado e nao interessa quantas vezes as requisiçoes acontecem que as sesspes nao estao guardadas na aplicaçáo.

Mandamos requeste e recebemos response. Ao criar e remover pods os dados nao sao perdidos pois sao stateless. Nao ha problemas em perder dados.

Por outrop lado, existem aplicaóés que desejamos persistior dados em nossas aplicaóés como o caesso a um DB. Conforme as operaçoes vao acaontecendo elas vao lendo e acessando. O porblema eh que se o pod for perdido, vamos perder os dados como a remoçao de um container.\



Para isso temos um volume, que permite que montemos uma pasta dentro do nosso container e conforme vamos escrevendo nela ele vai escrevendo para um armazenamento em disco.

Com o k8s tb é totalmente psossivel fazermos isso e escrevermos em disco na amazon, no azure, e tudo mais. Mas uma oiutra coisa muito interessante que faalaremos, é em relaçáo a volumes persistentes.

Quando trabalhamos com nuvem, temos algo chamado como pool de storage.

Vamos imageniar que criuamos um pool de storage de 1TB e deixamos disponivel para o nosso cluster k8s.

Entao quando quisermos subir uma apolicaçáo e criar um hd para guardar dados, podemos fazer uma Claim de 50GB para guardar um pouco de nossos dados, disponiblizando do 1TB, 50GB solicitado.

Quando trabalhamos com volumes persistenmntes no k8s sempre teremos 2 opçoes:

Uma parte estatica e uma parte dinamica.

A parte estática é quando é criada um pool de storage para a aplicaçáo ou varios pools de storage e solicitamos uma parte desse storage e entao se disponibliza um determinado espaço. Essa é a forma mais utilizada quando se trabalha com onPremises.

Quando trabalaha-se mais com nuvem, existe algo chamado de StorageClass. Ela é uma especifica;cao que faz com que se tengha um ndriver para que,m dinamcamente se consiga provisionar volumes, espaçoes em disco para uma determianda aplicaçáo.



Vamos imageniar que agora temos uma StorageClass da AWS configurada no nosso k8s. Entao todas as vezes que eu precisar de um volume persistente, faremos uma Claim e, automaticamente a Claim vai chamar a StorageClass e a StorageClass vai disponibilizar o espaço que precisamos (BlockStorage na AWS) por exemplo. 

Claim --> StorageClass --> Dsiponibilizar o espaço --> BlockStorage

Entao, o modelo estático é quando já deixamos defindo um monte de blocos (pool de storage) definiddos e entao fazemos uma solicitaçáo. Ou temos uma opçáo de criarmos uma StorageClass e ela, para cada vcez que fizermos uma solicitaçáo, essa StorageClass fala com o driver/tipo de disco/ rede que esta configurado para disponibilizar aquele pedaço para nós. 

Todas as vezes que o Wesley trabalhou com k8s em prod, uma vez que tb ele sempre utiliza serviços gerenciados, sempre foi atraves de StorageClass e nunca atraves de um formato estático.

Como que criariamos um pool de volumes? Vamos criar para fins de testes.

Crie um arquivo pv.yaml (persistent volume):

````bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce

````



Com isso, criamos um volume persistente , isto é, teremos um espaço de volume persistente disponiblizado., Depois criamso uma Claim para solicitar um pedaço do volume persistente.

Se trabalhamos com StorageClass por padrao, nao precisamos disso pois o StorageClass vai gerar dinamicamente os pedaços de espaço que estamos pedindo. 

Normalmente quando fazemos esse tipo de coisa, podemos acrescentar um epaço como local-device que vai pegar um espaço que está'disponiblizado no disco do nosso node para utilizar como volume persistente, por exemplo

````yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: local-device
````



A grande questao nao eh essa, mas que temnos duas formas de trabalhar: estática onde o es[aço já'está criado e definido (apenas requisitamos um espaço para a nossa aplicaçáo) e a dinamica que consiste em um StoageClass que sabe como vai funcionar e tem as credenciais para obter o pedaço de espaço que precisamos ali para ele.



Agora, o grande ponto é'o seguinte. De acord com o tipo de Storage que temos, temos o tipo de acesso (AccessMode). 

Dentre os AccessModes, temos o ReadWriteOnce/Many/Only. Qual a diferença entre todos eles?

O acesso a disco é algo extremamente copmplexo.

Imaginemos que temos 3 Pods acessando um disco de 50GB dentro de um Node1. Por enquanto nao há'nenhum problema para leitrura e etc.

O grande problema é que se tivermos o Pod3 no Node2, teriamos que ser aptos a gravar em um Volume em um Node a partir de um Volume que está em outro Node! Como sabemos que o arquivo que vamos querer gravar nao está sendo usado? Teremos que dar um lock nesse arquivo? Fica dficil.



O tipo de acesso mais comum de ser utilizado é o ReadWriteOnde, que significa que podemos gravar, ler, desde que estejam,os dentro do mesmo Node. Entao os Pods que estao dentro do mesmo Node do Volume podem ler e Gravar dentro desse mesmo Node. 

Existem alguns formatos de storage que permitem que psosamos ler e gravar em diferentes Nodes acessando. Mas sao outros sistemas de arquivos. 

Existe uma tabela no site do k8s que explica para cada Volume Pluginm, o tipo de acesso que podemos ter:

https://kubernetes.io/docs/concepts/storage/persistent-volumes/



Se algum dos plugins permitirem todos os tipos, a performance cai bastante pois tem que ter lock e um monte de outros tipos de controle.

E por isso o ReadWriteOnce é o mais comum de acontecer.



Portanto aqui temos duas opçoes: Criar antecipadamente o volume que queremos para o cluster e conforme os Pods vao subindo eles vao solicitando  esses volumes que devem ser disponiblizados.

Na maioria das vexes, vamos criar um Persistent Volume Claim.

No proximo topico, saberemos qual storageClass é criada por padrao no kind/k3d  e como muda de acordo com o clud provider e faremos alguns exemplos.



### Criando Volumes persistentes e montando

Vamos executar o seguinte comando:

````bash
❯ kubectl get storageclass
NAME                   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  118d
````

Com esse comando vemos que o storageClass padrao aqui é o rancher.io/local-path. Esse local-path provalemente pega um pedaço em disco e disponibliza para que isso vire um volume persistente no k8s.

O garnde ponto é que isso sempre muda de provedor para provedor. Na digital ocean, por exemplo, o provedor é um do-block-storage.

Isso significa que todas as vezes que fizermos uma Claim, esse provedor vai ser chamado e muito provavelemente ele tem uma interface com o armazenamento da Digital Ocean que deve gerar um disco lá'dentro.

No kind ou k3d, est;a gerando um local. Mas na nuvem, vai ser gerado um novo volume dentro de cada Nuvem.

Como consegfuimos fazer testes e entao verificar os volumes que estao rodando? Vamos duplicar o squivo de pv.yaml que fizemos anteriormente e chamá-lo de pvc (persistent volime claim), que faremos a solicitaçáo de que precisamos de um determinado volume!

pvc.yaml

````yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: goserver-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
````



Com isso, dizemos ao k8s que estamos solicitando (fazendo uma Claim) de um volume persistente cujo nome é goserver-pvc que possui um AccessMode de ReadWriteOnce em que solicitaremos 5GB de recurso para ele.

Vamos aplicar esse arquivo:

````bash
❯ kubectl apply -f k8s/pvc.yaml 
persistentvolumeclaim/goserver-pvc created
````

Pronto! Criamos. Vamos verificar:

````bash
NAME           STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
goserver-pvc   Pending                                      local-path     58s
````



Vemos que a nossa goserver-pvc está'pendente e foi criada! Nesse momento ela está'pendente pq ainda nao foi realizada uma conexao. 

Vamos ver:

````bash
❯ kubectl get storageclass
NAME                   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  118d
````



No comando acima podemos ver que o VolumeBindingMode está'com um estado de WaitForConsumer. Entao ao invés de ele já sair liberando e esperando, ele espera receber o bind para fazer a liberaçáo para nós.

Para fazer o bind devemos montar entao o nosso volume. 

Vamos no nosso arquivo de deployment.yaml e na parte de volumes, vamos colocar goserver-volume (aqui poderia ser qualquer outro nome). Em pesistentVolumeClaim -> ClaimName deve ser o goserver-pvc

Entao, aqui estamos criando um volume goserver-pvc que utiliza um persistentVolumeClaim que tem o nom de goserver-pvc . 

Além disso, é necessário ainda escrever em volumeMounts o mountPath correspondente

Logo, montaremos o goserver-volume no endereço "/go/pvc" e quando chamarmos o goserver-volume, ele vai chamas o clume to persistent volume claim goserver-pvc que acabamos por criar.

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v9.7"

          resources:
            requests:
              cpu: "0.05"
              memory: 20Mi
            limits:
              cpu: "0.05"
              memory: 25Mi

          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 30
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true
            - mountPath: "/go/pvc"
              name: goserver-volume

      volumes:
        - name: goserver-volume
          persistentVolumeClaim:
            claimName: goserver-pvc

        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

````



Vamos agora atualizar o nosso deployment

````bash
❯ kubectl apply -f k8s/deployment.yaml 
deployment.apps/goserver configured
````

E agora, vamos ver como esta o nosso pvc:

````bash
❯ kubectl get pvc
NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
goserver-pvc   Bound    pvc-d667b579-d412-48d2-be0e-d5dd83d54ad2   5Gi        RWO            local-path     18m
````

Vemos que o goserver-pvc esta como bound com um certo id com a capacidade de 5GB como ReadWriteOnce!

Depois vamos discutir como faremos isso com aplicaçoes com Bando de Dados!

Nessa aplicaçáo goserver, apesar de ela ser stateless, estamos criuando um volume para podermos guardar qualquer coisa.



Agora, vamos pegar o nosso pod

````bahs
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-64695797bb-7rpg2   1/1     Running   0          3m47s
````

Vamos entrar nele e vemos que temos uma pasta denominada pvc!

````bash
❯ kubectl exec -it goserver-64695797bb-7rpg2 -- bash
root@goserver-64695797bb-7rpg2:/go# ls
bin  myfamily  pvc  server  server.go  src
````

Vamos entrar nessa pasta e criar um aquivo oi

````bash
root@goserver-64695797bb-7rpg2:/go# cd pvc/
root@goserver-64695797bb-7rpg2:/go/pvc# touch oi
root@goserver-64695797bb-7rpg2:/go/pvc# ls
oi
````

Vamos sair do Pod e removê-lo. Se esse diretório pvc nao estivesse dentro de um volume persistente, iriamos perder o aqruivo que criamos. 

````bash
root@goserver-64695797bb-7rpg2:/go/pvc# ^C
root@goserver-64695797bb-7rpg2:/go/pvc# exit
command terminated with exit code 130

❯ kubectl delete pod goserver-64695797bb-7rpg2
pod "goserver-64695797bb-7rpg2" deleted
````



Mas montamos esse volume de forma persistente entao o arquivo oi deve estar contido da mesma maneira dentro do Pod que criamos!

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-64695797bb-8gqxd   0/1     Running   0          12s

❯ kubectl exec -it goserver-64695797bb-8gqxd -- bash
root@goserver-64695797bb-8gqxd:/go# cat pvc/oi 
root@goserver-64695797bb-8gqxd:/go#
````

E lá está! Isso aconteceu pq estamos trabalhando com volumes no k8s! Entao se estamos trabalhando com uma aplicaçáo no k8s e essa aplicaçao nao pode perder inform,açoes em hipotese alguma, podemos criar um volume e mandar os dados serem guardados ali.

Lembrando que o AccessMode ''e de ReadWriteOnce. Isso significa que outros Pods que estiverem em outro Node nao conseguirao acessar necessariamente esse volume aqui!



### Entendendo Stateteless vs Stateful

Temos dois tipos basicos de aplica'coes: stateless e stateful

O grande ponto eh que as aplica'coes stateless nao precisa guardar estado ou informa'cao. Ela simplesmente recebe uma chamada, ela processa, consulta um banco de dados, uma api, retorna uma resposta e acabou.

Já uma aplicaçao stateful ela precisa manter estado e o dado. Se ela nao fizer isso nao tem como ela funcionar. A forma mais facil de entender uma aplicaçao stateful eh pensando em um banco de dados, por exemplo.

Agora, essa aplicaçáo deve ter um disco em nossa aplicaçáo. Entao tudo o que acontece no bancoi de ddados está conectado a um dusco.

Isso significa que se a aplicaçáo morrer, os dados nao serao perfifos pois estarao gravados em disco. e nisso podemos fazer exatamente como fizemos no exemplo anterior, Criamos um disco persistente, atachamos no deployment do MySQL e fiamos felizes.

Mas as vezes o MySQL nao funciona mais sozinho pois temos Nodes Slvaes do MYQL como em um cluster!

No final das contas, nessa topologia, todos vao gravar no Master e ler nos MySQL slaves.

Supondo que cada instancia do MYSQL seja um Pod, como sabemos qual será o Master/Slave? E o processo de carregamento? Primeiro o Master carrega e depois o Slvae. E depois que o Slave subiu ele vai carregar os dados do disco do master para sincronizar. E ai quando o Slave 1 estiver pronto o Slave 2 sobe e copia os dados do Slcvae 1 para todos os dados agora estarem sincronizados. Tudo isso é Pod!

Quando trabalhamos com deployment, temos uma regra NOME_DEPLOYMENT-NOME_REPLICA_SET-RANDOM.

eNTAO SE MANDARMOS SUBIR 3 REPLICAS DESSE POD, TODOS VAO SUBIR AO MESMO TEMPO. QUAL SERÁ'O MASTER E COMO SUBIREMOS UM DE CADA VEZ E SINCRONIZAMOS EM QUEM EH O MASTER? ESSE É UM GRANDE PROBLEMA!

Por isso, quando famos de uma aplicaçao stateful, precisamos que ela suba de forma ordenada. Nao da para fazermos isso com Pods! E o processo de downsizing é'pior ainda! Vamos ter que matar Pods. E se for o master?



Entao precisamos de um mecanismo que nos ajude nesse tipo de processo. Que façá essa escala horizontal mas que o faça baseado em determinada ordem!

Precisamos tambem de um mecanismo que quando desescalamos, removedo nodes, seja como uma pilha, de tras para frente.

Os nomes randomicos que o k8s gera para a gente nao funciona mais.

Por isso vamos trabalhar com statefulksets.

O Statefulsets é um objeto do k8s muito parecido com o deployment mas que ele tem essas nuancias que vaoi nos ajudar a criar os pods em determinadas areas, Nao serao mais randomicos mas seguirao um determinada ordem para conseguirmos trabalhar etc.



### Criando Statefulset

Vamos criar um arquivo chamado statefulset.yaml que subirá um mysql. Vamos fazer um teste e escrever, primeiramente a estrutura como um Deplyment:

````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
    
````

E vamos aplicá-lo

````bash
❯ kubectl apply -f k8s/statefulset.yaml 
deployment.apps/mysql created
````

E vamos ver os Pods criados

````bash
❯ kubectl get po
NAME                        READY   STATUS              RESTARTS       AGE
goserver-64695797bb-8gqxd   1/1     Running             1 (116m ago)   17h
mysql-94c4ddd88-rqc8s       0/1     ContainerCreating   0              36s
mysql-94c4ddd88-8pz2k       0/1     ContainerCreating   0              36s
mysql-94c4ddd88-wdpxt       0/1     ContainerCreating   0              36s

````

Ainda nao deu certo pq faltam passar as variaveis de ambiente.

````bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
````

Aplicando o aquivo ao k8s novamente:

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS       AGE
goserver-64695797bb-8gqxd   1/1     Running   1 (121m ago)   17h
mysql-59f7587fc-mmd8f       1/1     Running   0              9s
mysql-59f7587fc-wtj8k       1/1     Running   0              7s
mysql-59f7587fc-xxxwm       1/1     Running   0              4s
````

Agora temos 3 containers/Pods rodando nosso Mysql. Qual deles seria o master? Nao conseguimos garantir a ordem e isso nos atrapalha muito!

Vamos deletar o deploy mysql que criamos!

````bash
❯ kubectl delete deploy mysql
deployment.apps "mysql" deleted
````

E mudar o tipo do yaml para StatefulSet. Todas as vezes que formos subir um StatefulSet precisamos tb colocar um serviceName. E todas as vezes que formos trabalhar com statefulSets vamos trabalhar com algo como headless service.

````yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql-h
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
````

Vamos colocá-lo para rodar:

````bash
❯ kubectl apply -f k8s/statefulset.yaml 
statefulset.apps/mysql created
````

Pronto! Agora criamos um statedfulset e vamos ver o que esrta acontecendo.

````bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS       AGE
goserver-64695797bb-8gqxd   1/1     Running   1 (152m ago)   18h
mysql-0                     1/1     Running   0              2m1s
mysql-1                     1/1     Running   0              20s
mysql-2                     1/1     Running   0              18s
````



Olha que legal! A partir de agora o statefulset já tem uma ordem para criaçao e nao eh mais randomico! Se quisermos entao criar, já é diferente, e se colocarmos para 8 replicas, ele vai criar em sequencia, um por vez e garante a nós a ordem de criaçao dos Pods! Isso é'muito importante pois precisamos ter nocao dessa ordem de criaçao quando estamos trabalhando com StatefulSet! E caso desejemos diminuir o numero de Pods, o proprio k8s vai começar a deletar os statefulsets Pods de tras para frente pq ele precisa dessa ordem sendo trabalhada e respeitada!

O mais interessante de tudo isso é'que como esses Pods sao statefuls, eles tem a orem de criaçao e de saida, temos que definir um DNS name para ele, que nesse caso chamamos de mysql-h que na verdade é um headless service que vai nos ajudar durante todo esse processo.



Caso desejassemos que a criaçao dos Statefulsets nao precisasse seguir uma sequencia de criaçáo e pudesse ser criada de forma paralela, basta adicionarmos a tag podManagementPolicy como parallel. Vamos testar isso com 8 replicas conforme o aquivo abaixo.

````yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql-h
  podManagementPolicy: Parallel
  replicas: 8
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
````



Deletando o StatefulS e aplicando novamente o arquivo no nosso cluster

````bash
❯ kubectl delete statefulset mysql
statefulset.apps "mysql" deleted

❯ kubectl apply -f k8s/statefulset.yaml 
statefulset.apps/mysql created
````



Verificando os Pods:

````bash
❯ kubectl get po
NAME                        READY   STATUS              RESTARTS       AGE
goserver-64695797bb-8gqxd   1/1     Running             1 (165m ago)   18h
mysql-0                     0/1     ContainerCreating   0              53s
mysql-1                     0/1     ContainerCreating   0              53s
mysql-2                     0/1     ContainerCreating   0              53s
mysql-3                     0/1     ContainerCreating   0              53s
mysql-4                     0/1     ContainerCreating   0              53s
mysql-5                     0/1     ContainerCreating   0              53s
mysql-6                     0/1     ContainerCreating   0              53s
mysql-7                     0/1     ContainerCreating   0              53s
````

Podemos escalar os Pods tb via linha de comando!

````bash
❯ kubectl scale statefulset mysql --replicas=5
statefulset.apps/mysql scaled
````

Agora ele cria tudo em paralelo e nao fica esperando criar um ou outro. 

Nisso podemos controlar tb esse tipo de comportamento, apesar de que, na maioria das vezes, é'desejavel criar um após o outro!

````
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS       AGE
goserver-64695797bb-8gqxd   1/1     Running   1 (169m ago)   18h
mysql-0                     1/1     Running   0              4m1s
mysql-3                     1/1     Running   0              4m1s
mysql-2                     1/1     Running   0              4m1s
mysql-4                     1/1     Running   0              4m1s
mysql-1                     1/1     Running   0              4m1s
````



### Criando headless serviceþ



Dentre os Pods que criamos do MYSQL, apenas o Pod Master vai realizar a grvaçáo no banco de dados. Os outros devem sewr de leitura. O problema é'que sabemos que quando estamos trabalhando com k8s e colocamos um Service no meio, o Servicxe e faz o load balancer. Imaginemmos que seja criado o MySQL Service. Todos que enviarem uma requisiçáo para o MySQL Service serao apontados para os Pods, mas apenas um deles pode escrever! Nesse caso, se quisessemos gravar precisariamos enviar oara o master e para leitura, apenas para os slaves.



Com isso, é'interessante que cada Pod tenha o seu respectivo Service. Entao gravaris no MySQL-0 e leriamos no MySQL-1 para frente.

Para fazermos isso, devemos utilizar o headless service. Ele basicamente é um servi;co que é'criado e forçámos ele a nao ter um IP interno dentro da aplica;cao e ele basicamente eh apenas um apontamento de DNS.

Isso significa que qiando criamos um headless service mandando MySQL Service master, ele apponta para o Service Master. Caso apontarmos para Mysql-h (headless) slave 1, ele vai apontar para o Slave 1. Nao tem um IP feito nele. Ele simplesmente é'um apontamento de DNS que o k8s faz internamente. Precisamos fazer isso para termos a possibilidade de escolher para qual Pod precisamos ir pq sabemos que cada Pod tem uma funçao diferente dentro do nosso processp pq eles sao StatefulSet! 

Portanto, ao invés de termos uma barreira (SERVICE) que ficaria balanceando essa carga, teremos no final um service para o master e um service para cada Node MySQL slave como apenas um apontamento via DNS, isto é, um headless service!

Vamos duplicar o arquivo de service.yaml e renomear para mysql-service-h.yaml (headless)

````bash
apiVersion: v1
kind: Service
metadata:
  name: mysql-h
spec:
  selector:
    app: mysql
  ports:
    - port: 3306
  clusterIP: None
````

Quando colocamos o clusterIP como None, o service sabe que nao vai trabalhar com IP para fazer as relaçoes, mas vai trabalhar com o DNS. Nesse ponto ele vai fal;ar: Como eu sei etao quais sao todos os Pods necessários para criar esse tipo de Headless Service? Ele vai fazer isso atraves do Service Name!

No arquivo de configurtaçao do StatefulSet, foi criado um serviceName! Entao esse serviceName do statefulSet deve ser identico ao nome do nosso serviço descrito em mysql-service-h.yaml

Logo, esse serviço vai resolver tudo o que precisamos via DNS, independente da quantidade de replicas que formos tendo. 



Lembrando: O serviceName do statefulSet tem que bater com o serviceName do service que estamos criando para ele. E o clusterIP tem que ser None. Baseado nisso, vamos conseguir o tao sonhado headless service!

Vamos deletar o statefulset do Mysql

````bash
❯ kubectl delete statefulset mysql
\statefulset.apps "mysql" deleted
````

E agora vamos apluicar o statefulset com algumas replicas 

````yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  replicas: 4
  serviceName: mysql-h
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
````

E aplicamos o novo arquivo 

````bash
❯ kubectl apply -f k8s/statefulset.yaml 
statefulset.apps/mysql created
````

E agora vcamos criar o nosso service headless

````bash
❯ kubectl apply -f k8s/mysql-service-h.yaml
service/mysql created
````



Pronto! Vamos ver na pratica o que isso esta acontecendo!

````bash
❯ kubectl get po                        
NAME                        READY   STATUS    RESTARTS        AGE
goserver-64695797bb-8gqxd   1/1     Running   4 (4h39m ago)   47h
mysql-0                     1/1     Running   0               5m9s
mysql-1                     1/1     Running   0               5m9s
mysql-2                     1/1     Running   0               5m9s
mysql-3                     1/1     Running   0               5m9s

````

Nesse caso temos o mysql-0 como o master. Entao se alguem quiser escrever alguma coisa deve se conectar no mysql-0 via DNS. 

Ao executarmos o comando de verificar os serviços, percebemos que temos o nosso mysql-h com CluserIp e batendo na porta 3306 na porta external.

````bash
❯ kubectl get svc                       
NAME               TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
kubernetes         ClusterIP      10.43.0.1       <none>         443/TCP          120d
goserver-service   LoadBalancer   10.43.203.126   192.168.96.2   8080:30210/TCP   119d
mysql-h            ClusterIP      None            <none>         3306/TCP         110s
````



Uma vez que criamos esses statefulsets e eles estao sendo criados e o serviceName 'e o mysql-h, o matchlabesls ;e o app: mysql do statefulset que linka com o app: mysql do mysql-service-h.yaml, pors etc. Agora precisamos que quando vamos trabalhar com o service, ele gere inclusive o service de outros camaradas, eventualmente.

Basicamente, o que acontece agora 'e o seguinte: Todas as vezes em que precisarmos chamar o noisso Master ou qualquer coisa desse tipo, faremos o seguinte.



Vamos entrar dentro de um Pos para ficar mais facil e dar um ping no dns do servi'co que criamos!

 ````bash
 ❯ kubectl exec -it goserver-64695797bb-8gqxd -- bash
 root@goserver-64695797bb-8gqxd:/go# ping mysql-h
 PING mysql-h.default.svc.cluster.local (10.42.0.184) 56(84) bytes of data.
 64 bytes from mysql-1.mysql-h.default.svc.cluster.local (10.42.0.184): icmp_seq=1 ttl=64 time=0.137 ms
 64 bytes from mysql-1.mysql-h.default.svc.cluster.local (10.42.0.184): icmp_seq=2 ttl=64 time=0.059 ms
 64 bytes from mysql-1.mysql-h.default.svc.cluster.local (10.42.0.184): icmp_seq=3 ttl=64 time=0.055 ms
 ````

OLha só! Ele está caindo em mysql-1! Vamos ver agora para colocarmos no mysql-0.

````bash
root@goserver-64695797bb-8gqxd:/go# ping mysql-0.mysql-h
PING mysql-0.mysql-h.default.svc.cluster.local (10.42.0.183) 56(84) bytes of data.
64 bytes from mysql-0.mysql-h.default.svc.cluster.local (10.42.0.183): icmp_seq=1 ttl=64 time=0.158 ms
64 bytes from mysql-0.mysql-h.default.svc.cluster.local (10.42.0.183): icmp_seq=2 ttl=64 time=0.055 ms
64 bytes from mysql-0.mysql-h.default.svc.cluster.local (10.42.0.183): icmp_seq=3 ttl=64 time=0.060 ms
````

O que aconteceu aqui foi que, através desse serviço, baseado no nome, chamar o Pod quie queremos e isso é'o mais importante!

Entao com esse headless service, basta colcoarmos o nome do Pod que queremos chamar na frente ponto mysql-h e ele vi conseguir nos redirecionar exatamente para o Pod que estavamos querendo chegar!

Isso é incrivel pq basicamente estamos realizando apontamentos dinamicos de DNS sem a necessidade de termos algo por IP!

Agora para cada POD conseguimos chamar baseado nessa convençáo!

NOME_DO_POD.NOME_DO_SVC.NAMESPACE.svc.cluster.local

Logicamente o k8s deixa passarmos uma resoluçáo mais simples, a nao ser que tenhamos um outro namepace e entao temos que passar toda essa resoluçáo. 

Com isso, o k8s estabelece que se precisarmos falar com o master é mysql-0. Acima disso, sabemos que vamos conseguir falar com os outros Pods.

Essa ideia de conseguirmos trabalhar com statefulset e headless service sempre vai nos ajudar bastante.

O grande segredo mesmo é que o nome do servico em mysql-service-h, por exemplo, seja o mesmo nome do serviceName que está setado no statefulSet.yaml.



### Criando Volumes Dinamicamente com StatefulSet

Agora, o grande ponto é quie o nosso banco de dados vai precisar gravar os dados no banco. Vamos precisar de um volume persistente para que os dados fiquei guardados caso o Pod venha a morrer.

Entao aprendemos que podemos criar um PrsistentVolumeClaim, que entao vai gerar um volume e ele vaqi dar um bind com o nosso Pod e vai ficar tudo feliz. 

O grande ponto eh quem em alguns casos vamos querer criar um bamnco de dados por réplica. P;or exemplo, tem 4 mysql e além da aplicaçáo mysql vamos querer criar 5 voluimes persistentes para guardarmos os dados do nosso mysql. E isso nao vamos querer fazer manualmente pq quando formos querer escalar na mai nao vamos querer dizer com é um persistent volume claim que qeueremos fazer.

E se tivesse uma forma que todas as vezes que estivermos trabalhando com o Statefulset e quisermos aumentar a quantidade de replicas, automaticamewnte ele gera um novo volume persistente automaticamewnte?



Sim! Entao, ao invés de trabaltarmos no pv.yaml, basicamente o que faremos é um pedaço do template de pv,yaml dentro do template de statefulset.yaml.

E entao todas as vezes que criarmos uma nova réplica, novos olumes serao criados e atachados na nossa aplicação.



Basicamente, agora vamos criar uma área chamada volumeClaimTemplate no arquivo statefulset para ser utilizado todas as vezes que quisermos escalar uma nova replica nossa. E dentro da spec do container do mysql camos colocar o volumeMOunths com o caminho do lugar ondeos arquivos do mysql  que criamos ficam gravados e em name vamos colocar o nome do volume que colocamos no template.

Nesse momento entao queremos dizer que a todo o momento que criamos uma nova replica ele vai automaticamente criar uma claim para chamar um volume e vai atachar automaticamente esse volume para esse nosso detrerminado POd!

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  replicas: 4
  serviceName: mysql-h
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
          volumeMounts:
            - mountPath: /var/lib/mysql
              name: mysql-volume

  volumeClaimTemplates:
  - metadata:
      name: mysql-volume
    spec: 
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi

```



Vamos colcoarpara funcionar desde o começo

```bash
❯ kubectl delete statefulset mysql
statefulset.apps "mysql" deleted

❯ kubectl apply -f k8s/statefulset.yaml
statefulset.apps/mysql created
```

Podemosver aqui que os Pods foram criados.

```bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS      AGE
goserver-64695797bb-8gqxd   1/1     Running   5 (26h ago)   3d3h
mysql-0                     1/1     Running   0             55m
mysql-1                     1/1     Running   0             55m
mysql-2                     1/1     Running   0             55m
mysql-3                     1/1     Running   0             55m
```



Mas como entao saberemos que o nosso volume foi criado? Nesse caso, vamos fazer o seguinte:

```bash
❯ kubectl get pvc
NAME                   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
goserver-pvc           Bound    pvc-d667b579-d412-48d2-be0e-d5dd83d54ad2   5Gi        RWO            local-path     3d4h
mysql-volume-mysql-0   Bound    pvc-e40122d4-6fc3-4b28-acbe-c6ffcfb6e709   5Gi        RWO            local-path     57m
mysql-volume-mysql-1   Bound    pvc-dd85f32c-3791-4390-aba5-201912e75a7b   5Gi        RWO            local-path     56m
mysql-volume-mysql-2   Bound    pvc-005d8aff-d672-43db-8836-9aecc6b95a3b   5Gi        RWO            local-path     56m
mysql-volume-mysql-3   Bound    pvc-25c8cd32-590e-4b10-9b32-f32268d79ea0   5Gi        RWO            local-path     56m
```

O que temos acima sao os volumes que criamos baseados no mysql.

Nesse caso, ele criou um volume para cada Pod!

Agora vamos imaginar o seguinte. Que por algum motivo o Pod mysql-1 ẽ deletado. Oque acontece a partirde agora é que o Scheduler do k8s vai perceber que um pod nao mais existe e ele vai criar um outro e esse Volume 1 vai ser reatachado. O que aconteceu é que o volume nao foi apagado pois esta separado do POd!

Com isso conseguimos trabalhar de forma muito eficiente trabalhando com o statefulset.

POrtanto, basicamente conseguimos criar replicas de forma ordenada, matar service de forma ordenada, criar headless service para fazer a requisicao cai no serviço que desejamos e criartemplatesde volumes que a cada POd que eh criado ele gera um volume que atacha nesse emplate. E se caso viermos a atar um determinado POd o volume nao vai ser removido, mas assim que ele for recreiado ele reatacha no nosso volume!

Temos agora uma aplicaçcao mais que completa para trabalharcom aplicaçoes que precisam guardar dados, trabalhar com volumes persistentes, que precisam ter ordem de criacao e remçcao, contar com indice de identificaçao etc.



### Devo usarmeu banco de dados no kubernetes

A grabde pergunta ẽ se devemos guardar nosso db dentro k8s. Eh seguro?Eh rapido? O que devemos fazer?

Em relaçcao a parte de estrutura, o k8s tem diversos recursos para nos ajudar durante todo esse processo. 

O grande ponto eh que quando temos o nosso projeto pode ser interessante deixar o banco de dados fora do k8s.

Isso posto pois as aplicações ainda nao estao maduras o suficientes para serem escaladas ou para o limitese metricas! Banco de dados é algo muito complexo, principalmente se estamos ridando aplicações muito criticas!

Se temos um sistema simples e nao temos muita dor de cabeça, nao ha problema nenhum em colcoar dentro do k8s. Mas em uma aplicação critica, o banco de dados eh uma aplicaçcao tao tunavel e tao paralelo que o wesley tende a deixar o banco de dados fora do kubernetes.

A estrutura que ele trabalha hoje ele tem mysql postgres elasticsearch  e todos esses bancos de dados nao ficam dentro do seus cluster k8s. 

Ele usaserviços gerenciados pra ter paz pos sabe que esses serviços vao conseguir gerenciar melhor do que gerenciariamos no k8s por nao sermos especialistas. Podemos escolher um Amazon RDS ou GOgleCLoud Databases onde gerencia odbe tem zonas de disponibilidades, backups e etc. Ẽ algo tao critico que ele prefere deixar fora do k8s. Ele conhece pessoas que deixam dentro do k8s e esta tudo ok, ja trabalhou com db no k8s e nao teve problemas e etc. Mas ha uma recomendaçao pessoal principalmente se for um serviço muito critico. 

Isso nao significaque possamos ter um wordpress no nosso k8s em que deixamos o banco de dados rodando no k8s e tb o upload de arquivos com voumes persistens para o wp-content para colocarmos ali por exemplo.

Isso pq pode ser muito critico para misturar no k8s. Ẽ importante pensar nisso!

É sempreimportante tb nas clouds garantirmos que se esta fazendo um correo backup!



## Ingress

### VIsao Geral

Vimos que temos um tipo de service que o o LoadBalancer.Nesse serviço, especificamente geraumIP externo quando estamos rodando serviços gerenviaveis do k8s comoo GKE (Google)  o EKS (AWS) e o AKS (Azure). 

Agora vamos imaginar que estamos em uma arquitetura rodando microserviços e naquela arquietura temos 10 micrseriços para funcionar com acesso a Web. 

Intuitivamente colocariamos esses microsserviços para funionarem como LoadBalancer, vamos pegar o IP de cada um, confihurar o DNS e sermosfelizes.

Mas normalmente quando fazemos isso, ele vai gerar na nuvem um LOad Balancer e gerar um IP e isso tem custo! É aqula qhistoria que nao tem tanto sentido termos tantos LOaBalancers por serviço.

E é por conta disso que o k8s tem um serviço muito interessante que se chama Ingress, OINgress acaba sendo como o ponto unico de entrada na nossa aplicaao. Assim configuramos o Ingress e ele ẽ um Service LOadBalancer que vai ter um IP.

Vamos imaginar que tenhamos  10 serviços ali. ENtao todas as vezes que quisermos acessar qualquer um desses seriços vamos bater no IP do ingress. E ai baseado no hostname e no Path que a pessoa estã acessando, configuramos para que quando acesso via alguma URL vai para tal serviço ou .admin, manda para o serviço do adim; catalogo, manda para o serviço do caalogo ; busca.algumacoisa, manda para o microsserviço de busca. 

Portanto, o ingress acaba sendo o ponto unico de entrada e ele faz o roteamente nos serviços que queremos. Nesse caso ele acaba por lembrar uma API Gtaeway, que faz o roteamento das coisas tb. ELe tb lemra muito um Proxy reverso que pega a requisiçcao e roteia para onde quisermos.

N realizadade a grande sacada de se trabalhar como Ingress no k8s, ẽ que existe um contolador que ẽ o Ingress COntroler do nginx!

Entao podemos ter um nginx por baixo dos planos e ele vai receber as requisiçoese fazer os apontamentos (isso ẽ tranasparente quando usamos esse controlador do k8s).

Nesse capitulo vaos apredercomo instalar, como configurar e como fazemos isso do zero até configurarmos o DNSao vivo para conseguirmos colocarmos no ar!



### COnfigurando Aplicaçao no GKE

Primeiramente, vamos acessar o google compite plataform (GCP) e criar um cluster k8s com apenas uma maquina.

Acesse https://console.cloud.google.com/ e faça o login.

Clique em acessar o console e crie um projeto fullcycle-examples;

Depois vá em Kubernetes Engine e crie umcluster.

]AQUI EU NAO SEGUI EM DIANTE PARA NAO TER QUE GERAR CUSTOS]

Com a linha d comando instalada (gcloud), exevute o seguinte comando 

```bash
gcloud container clusters get-credentials cusros-fullcycle --zone us-central1-c --project fullcycle-examles
```

Com isso, o setup das credenciais foi realizado automaticamente.

Com o comando abaixo:

```bash
kubectl get nodes
```

Teremos o resultado dos Node queforam criados no GCP!

Nesse momento, a nossa mãquina estã atachada com o k8s do Google e nao mais do local.

Agora, vamos imaginar que estamos em nossa mãquina e queremos fazero deploy para um CLoud Provider. Ou estamos em um CLoud Provider e desejamos fazer um deploy para outro CLoud Provider.

Basta entao aplicarmos o diretorio onde estao localizados todos os nossos arquivos de manifest (yaml).

```bash
kubectl apply -f k8s/
```

E incrivelmente o kubectl vai aplicar todos os arquivos AUTOMATICAMENTE!!!

Aunicaque ee nao reconheceu foi a do kind, que foi feita para o server local mesmo. 

Equando dermos 

```bash
kubectl get po
```

Devemos ver todos os nossos POds sendo criados jã na nuvem!!! QUe incrivel isso!

Com apenasum comando jã está tudo la no ar!

Outra cosia que vamos precisar fazer tb é verificar os services. 

```bash
kubectl het svc
```

E podemos notar que pela primeira vez o EXTERNAL IP apareceu! Isso pq o serviço do goserver ẽ loadbalancer. Se tivessemos mais serviços, cada serviço iria gerar um External Ip e teriamos que pagar por cada um deles!

Se formos no nosso navegador e acessarmos esse External IP vemos que estã tudo funcionado apenas ridando os maifestos do k8s!



### Instalando o Ingress Nginx Controller

Existem diversosformasde trabalharmos com o Ingress e vamosutilizaro IngressNginx. Para fazer ainstalação existem diversas formas, mas vamos trabalhar agora com o Helm. 

Vamos digitar no GOogle  ingress nginx helm chart;

https://kubernetes.github.io/ingress-nginx/deploy/

Aqui vemos que podemos realizar a instalaçao de diversas maneiras, incluisive usando o KE (Google). 

Entao, o quepodemos fazer ẽescolher o nosso Cloud Provider e seguir o processo de instalação. A outra opçcao ehusando Helm, Mas quando formos colcoar em produçao, é muiti importante que façamos a insalaçao de acordo com cada CLoud pq ele vai trabalhar com a partede permissao, vai fazer os attaches de arbach e etc.

No nosso caos generico agora, vamos copiar a linha de instalaçao abaixo e colar. UMa coisa importante é que somente a versao 3 do helm eh permitida.

Um ponto importante eh que quando damos um helm install nginx, perceba que estamos rodando isso e ele vai instalar em um namespace padrao. Namespaces no k8s sao espaços que acabamos separando aplicaçoes e contextos. Inclusive para cadanamespace conseguimos colocar regras de permissionamento, de recursos computacionais, de quais usuaros podem acessar, regras de service accounts que podem rodar. Etao isso eh um ponto importante. COmo estamos nesse exemplo, vamos rodar dessa forma, que ele vai instalar o igress-nginx no nosso namespace default. Mas na vida real isso normalmente eh separado em um namespace chamado ingress nginx, por exeplo.

Vamos instalar o hlm para o ubuntu via apt

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

E entao

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
  
----

Release "ingress-nginx" does not exist. Installing it now.
NAME: ingress-nginx
LAST DEPLOYED: Sat Nov 19 23:28:53 2022
NAMESPACE: ingress-nginx
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The ingress-nginx controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller'

An example Ingress that makes use of the controller:
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: foo
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: exampleService
                  port:
                    number: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
      - hosts:
        - www.example.com
        secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```



Aõs a instalaçao / update, instalamos o ingress-mginx e tb eslesestã dando um template para nõs de como podemos criar um manifesto de Ingress com o host e tb com o TLS.

COmo podemos ver se o nossoIngress COntroler esta rodando? Vamos ver com o comando abaixo

```bash
❯ kubectl get svc
```

Como no nosso caso nao estamos na nuvem, o Ingress COtroler parece estar pendente aguardando um IP externo.

Ẽ no IP externo que devemos estar ligado. Todo mundo que for entrar pela nossa aplicação vaientrar pelo nosso IP externo ahora. Depois podemos atẽ mudar o service go goserver e tirar o external IP dele pq nao precisaremos do External IP mais pq usaremos apenas o External Ip do Ingress Controler.

Entao uma vez que temos o ingress contoler instalado e rodando podemos ver o Pod responsavel pelo Ingress com o comando 

```bash
❯ kubectl get po 
```

Por isso que normalmente nõsseparamos por namespace pq nao tem muito sentido o Ingress ficar rdando no mesmo namespace que estao rodando as nossas aplicações. 

### Configurando Ingress e DNS

Agora vamos criar um novo arquivo chamado Ingress.yaml. Nesse arquivo temos algumas annotations. Elas sao importantes porque cada sistema que for utilizar desse ingress pode pegar essas annotations e poder interpretar para utilizar alguma funcionalidade. No nosso caso, a nossa annotation vai informar que vamos utilizar o ingress do nginx.

Vamos aplicar esse arquivo de ingress

````bash
❯ kubectl apply -f k8s/ingress.yaml                                                                
error: resource mapping not found for name: "ingress-host" namespace: "" from "k8s/ingress.yaml": no matches for kind "Ingress" in version "networking.k8s.io/v1beta1"
ensure CRDs are installed first
````

Como em nosos caso nao temos um IP externo por estarmos testanto em um server local, o Ingress dá'esse tipo de erro pois nao foi possivel configurar um Namespace para esse service

 Uma vez que temos o ingress-host criado, ao chegar uma requisiçáo no dominio indicado, 

Caso déssemos um

````bash
kubectl get svc
````

Deveriamos visualizar o ingress-nginx-controller service como LoadBalancer e IP externo.

Com o IP copiado, colocaríamos ele no Gerenciamento de DNS e entao o IP externo estaria atachado no DNS. Ao colcoar o DNS no navegador, veriamos a aplicaçáo funcionando!

Se colocassemos /admin no ingress.svc, e aplicarmos o arquivo no k8s, bastaria acessar o endereço no navegador com /admin!

Aqui agora o mais importante é sabermos que poderiamos instancias vsrios prefixos e sufixos e urls e que cada um deles mandaria para um dos serviços do k8s e nao precisariamos mais do tipo de service do goserver-service como LoadBalancer, mas como ClusterIP e economizariamos uma grana. 

Vamos fazer isso. 

````yaml
apiVersion: v1
kind: Service
metadata:
  name: goserver-service
spec:
  selector:
    app: goserver
  type: ClusterIP
  ports:
  - name: goserver-service
    port: 8080
    targetPort: 8000
    protocol: TCP

````



E aplicamos a nova configuraçáo de service

````bash
❯ kubectl apply -f k8s/service.yaml 
service/goserver-service configured

❯ kubectl get svc                  
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes         ClusterIP   10.43.0.1       <none>        443/TCP    103m
mysql-h            ClusterIP   None            <none>        3306/TCP   101m
goserver-service   ClusterIP   10.43.191.199   <none>        8080/TCP   101m

````

É possivel que de um erro ao atualizar se for realizada essa mudanáaca na Cloud. Basta deletar e aoplicá-lo novamente. 



Nesse ponto, podemos ter agora quantos services quisermos e que vai rotear quando chegar nesse IP será o nosso service do Ingress com Load Balancer, que nos fornece um IP externo!



## Cert Manager

### Instalando o Cert Manager

Existem 2 formas gerais para fazermos a instala';çao dos cerificados no k8s.  A priemira forma é o que o ingress sugere.

Criamos um secret e passamos um crt do certificado. E ai a configuraçáo do TLS le o crt e a key e ai esta aplicado o nosso certificado. Se comprarmos im certificado normal, pgaamos ele, criamos o secret e colcoamos a configuraçáo basica ali. 



Mas hoje em dia temos o Cert Manager do lets Encrypt que gera os certificados para nós automaticamente e gerencia o tempo de validaçáo e gera os novos certificados para nós de maneira automatica.

Hohje em dia está sendo grandemente utilizado este método ao inves de ficar renovando e alterando servidor e tudo mais. Vamos ver como fazemos isso!



Acessando a documentaçao disponivel em https://cert-manager.io/v1.1-docs/installation/kubernetes/ ele mostra como fazemos a instalaçáo. Mas a ideia principal é'que instalemos o manifesto. 

Se tivermos usando o Google (GKE), ele vai pedir para rodarmos um comando para garantir que teremos permissao 

````bash
kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
````



Nota: o k8s trabalha com permissionamentoS. eSSE PERMISSIONAMENTO ESTA LIGADO DIRETAMENTE AO USU;ÁRIO QUE ESTA CADASTRADO NO SERVIÇO IDENTIDADE DO GOOGLE CLOUD.

Entao ele tem que falar que o nosso usuário, referente a tal no k8s e esse cara no k8s deve ter acesso a esse tipo de role que ele vá trabalhar.

O cert manager tem a opçáo que podemos instalar utilizando Helm ou a opçao que podemos instalar baseada no passo a passo que ele vai mostrar para nós. 

Para variar um pouco, vamos fazer pelo passo a passo. Importante: da mesma forma que estamos trabalhando com o Ingress e criamos um namesmpaxe para o Ingress, devemos criar um namespace especifico para o CertManegr para nao ficar misturando com os nossos Pods. 

Ele vai ter ali basicamente os seus permissionamentos e etc. Vamos seguir o passo a passo. 



Vamos criar um namespacce para o cert manager

````bash
❯ kubectl create namespace cert-manager
namespace/cert-manager created
````

E listar todos os namespace que temos:

````bash
❯ kubectl get ns
NAME              STATUS   AGE
cert-manager      Active   31s
default           Active   3h27m
ingress-nginx     Active   89m
kube-node-lease   Active   3h27m
kube-public       Active   3h27m
kube-system       Active   3h27m
````



Os namespaces kube* sao os padroes do k8s. O dfefault é o que estamos rodando na nossa aplicação e o cert-manager é o que acabamos de criar.

Agora. podemos usar a isntalaçáo com helm ou manual. 



Vamos fazer a manual:

````bash
❯ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.3.0/cert-manager.yaml
----
customresourcedefinition.apiextensions.k8s.io/certificaterequests.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/certificates.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/challenges.acme.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/clusterissuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/issuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/orders.acme.cert-manager.io created
Warning: resource namespaces/cert-manager is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
namespace/cert-manager configured
serviceaccount/cert-manager-cainjector created
serviceaccount/cert-manager created
serviceaccount/cert-manager-webhook created
clusterrole.rbac.authorization.k8s.io/cert-manager-cainjector created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-issuers created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-clusterissuers created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-certificates created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-orders created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-challenges created
clusterrole.rbac.authorization.k8s.io/cert-manager-controller-ingress-shim created
clusterrole.rbac.authorization.k8s.io/cert-manager-view created
clusterrole.rbac.authorization.k8s.io/cert-manager-edit created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-cainjector created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-issuers created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-clusterissuers created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-certificates created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-orders created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-challenges created
clusterrolebinding.rbac.authorization.k8s.io/cert-manager-controller-ingress-shim created
role.rbac.authorization.k8s.io/cert-manager-cainjector:leaderelection created
role.rbac.authorization.k8s.io/cert-manager:leaderelection created
role.rbac.authorization.k8s.io/cert-manager-webhook:dynamic-serving created
rolebinding.rbac.authorization.k8s.io/cert-manager-cainjector:leaderelection created
rolebinding.rbac.authorization.k8s.io/cert-manager:leaderelection created
rolebinding.rbac.authorization.k8s.io/cert-manager-webhook:dynamic-serving created
service/cert-manager created
service/cert-manager-webhook created
Warning: Autopilot set default resource requests for Deployment cert-manager/cert-manager-cainjector, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/cert-manager-cainjector created
Warning: Autopilot set default resource requests for Deployment cert-manager/cert-manager, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/cert-manager created
Warning: Autopilot set default resource requests for Deployment cert-manager/cert-manager-webhook, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/cert-manager-webhook created
Warning: AdmissionWebhookController: mutated namespaceselector of the webhooks to enforce GKE Autopilot policies.
mutatingwebhookconfiguration.admissionregistration.k8s.io/cert-manager-webhook created
validatingwebhookconfiguration.admissionregistration.k8s.io/cert-manager-webhook created


````

Agora, vamos dar um get Pots para namespaces 

````bash
❯ kubectl get po -n cert-manager
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-5f8676b596-r7zln              1/1     Running   0          2m28s
cert-manager-cainjector-59445c7dc9-vq644   1/1     Running   0          2m29s
cert-manager-webhook-55f6cd8bc6-q5qz7      1/1     Running   0          2m27s

````

Pronto! Agora que temos os Pods responsssáveis pelo Cer Manager, vamos criar o set Issue. Esse é que fará a geraçáo e o issue do nosso certificado.



### Configurando e emitindo certificado

Vamos fazer o nosso certificado funcionar. Vamos criar um tipo de objeto Ussuer pq instalamos o Cert-Manager. Esse arquivo chamaremos de cert manager que é'chamado de clusterIssuer. Ele é que vai se responsabilizar para fazer a geraçáo dos nossos certificados.

No arquivo abaixo, o kind só é possivel pq instalamos a API do cert-manager

cluster-issue.yaml

````yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
  namespace: cert-manager
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: roger10cassares@gmail.com
    privateKeySecretRef:
      name: letsencrypt-tls
    solvers:
    - http01:
        ingress:
          class: nginx

````



E agora vamos aplicar essa configuraçáo

````bash
❯ kubectl apply -f k8s/cluster-issuer.yaml
Error from server (InternalError): error when creating "k8s/cluster-issuer.yaml": Internal error occurred: failed calling webhook "webhook.cert-manager.io": failed to call webhook: Post "https://cert-manager-webhook.cert-manager.svc:443/mutate?timeout=10s": x509: certificate signed by unknown authority
````

SE NAO FOSSE ESSE ERRO, A PARTIR DE AGORA ESTARAMOS APTOS A GERAR OS NOSSOS CERTIFICADOS CONFORME FORMOS PRECISANDO.



Utilizando o ingress comum dns configurado corretamente, podemos aplicar o comando 

```bash
❯ kubectl apply -f k8s/cluster-issuer.yaml
```

A partir de aoraestamosaptosa trabalhar no Ingress efazer comque ele gere os nossos certificados conforme formos precisando. 

Vamos entao trabalhar com o Ingress e trabalhar com as annotations. As annotations vao ser uteis aqui para a gente para resolvermos exatamente sse  tipo de problema e o cert manager saber que vamos usar o ingress para a emissao desse tipo de certificado.



!!!!!!!!!!!!!!!!!!!!!!!!!!



## Namespaces e Service ACCOUNTS

## Namespaces

Agora vamos falar de alguns assuntos extremamente imopoortantes quando formos fazer deploy., colocar em produçao a nossa app com k8s ou mesmo como vamos conseguir separar os nossos projetos na hora que formos fazer a instalaçao.  

A primeira coisa a se entender é o coneceito de Namespaces. Para que serve o namepace? O namespace é basicamente uma separacao virtual logica do nosso cluster k8s para onde fazemos a instalacao de cada coisa.

Entao, por exemplo, todas as vezes que a gente estava instalanmdo qualquer coisa estavamos instalando no namespace default.

Se dermos o comando abaixo com a respectiva saida

```bash
kubectl get po
No resources found in default namespace.
```

Isso significa que sempre que vamois acessar o k8s quando fazemos uma instalaçao e quando nao falamos qual eh o namespace, ele vao instalart tudo aquilo no nosso namespace padrao.

Mas para que o namespace?



Vamos imaginar que temos um projeto1. Entao tudo o que formos instalar, vamos instalar esse projeto 1 no namespace 1.

\Se formos instalar um projeto2 no namespace2, é possivel fazer essa separaçao. 

Mas o mais bacana de tudo é que quando estamos trabvalhando com esses dois projetos, eventua;mente, se setarmos recursos do nosso k8s, podemos falar que o namespace1 tem tanto de recurso. E o namespace 2 tem menos recursos! Entao um namespace nao consegue extrapolar e prejudicar o outro ali no k8s. Tb toda a parte de segurança e de acesso nós conseguimos fazer essa separaçao tb com o namespace. 

Portanto, é sempre recomendado, toda a vez em que formos criar um projeto específico ou fazer um deploy de forma geral dentro de um projeto como um todo, cria um namespace específico para aquele projeto porque vai ficar tudo muito mais separado. 

Uma outra coisa importante tb que muita gente faz, é semprar os namepaces de um ambiente de desenvolvimento de um ambiente de produçao. No caso, o wesley, de forma geral recomenda que se ternha um cluster para cada coisa, mas se vc só tem or;camento para um cluster, vc ainda pode separar pq terás ainda que em um cluster, a mesma aplicaçao em modo de desenvolvimento e em modo de produçao apenas criando o namespace. Vamos ver isso na pratica!

```bash
❯ kubectl get ns
NAME              STATUS   AGE
cert-manager      Active   43h
default           Active   2d4h
ingress-nginx     Active   2d1h
kube-node-lease   Active   2d4h
kube-public       Active   2d4h
kube-system       Active   2d4h
```

Ele vai mostrar todos os namespaces que temos nosso cluster.

O default é sempre quando nao passamos nada. 

Se dermos o seguinte comando

```bahs
❯ kubectl get po -n=kube-system 
NAME                                                     READY   STATUS    RESTARTS   AGE
anetd-2whr4                                              1/1     Running   0          2d4h
anetd-cxptw                                              1/1     Running   0          2d1h
anetd-dpqzb                                              1/1     Running   0          2d1h
anetd-kchls                                              1/1     Running   0          2d1h
anetd-kltq6                                              1/1     Running   0          2d1h
anetd-kwqfg                                              1/1     Running   0          2d4h
anetd-rgwsl                                              1/1     Running   0          43h
antrea-controller-horizontal-autoscaler-8484b5d7-hnttk   1/1     Running   0          2d4h
egress-nat-controller-5bb85946bc-fwc9z                   1/1     Running   0          2d4h
event-exporter-gke-5dc976447f-4lc62                      2/2     Running   0          2d4h
filestore-node-7mfnl                                     3/3     Running   0          2d1h
filestore-node-869gh                                     3/3     Running   0          2d1h
filestore-node-glf5w                                     3/3     Running   0          2d1h
filestore-node-jjxw4                                     3/3     Running   0          2d4h
filestore-node-kmqg8                                     3/3     Running   0          2d1h
filestore-node-mkv9x                                     3/3     Running   0          2d4h
filestore-node-t87th                                     3/3     Running   0          43h
fluentbit-gke-big-2wzkq                                  2/2     Running   0          43h
fluentbit-gke-small-57mnv                                2/2     Running   0          2d4h
fluentbit-gke-small-bcngv                                2/2     Running   0          2d1h
fluentbit-gke-small-dfr5t                                2/2     Running   0          2d1h
fluentbit-gke-small-lxjk2                                2/2     Running   0          2d1h
fluentbit-gke-small-sl2nl                                2/2     Running   0          2d4h
fluentbit-gke-small-sr7ff                                2/2     Running   0          2d1h
gke-metadata-server-2c4d8                                1/1     Running   0          2d1h
gke-metadata-server-95p2c                                1/1     Running   0          43h
gke-metadata-server-lqtrt                                1/1     Running   0          2d1h
gke-metadata-server-rg6rv                                1/1     Running   0          2d4h
gke-metadata-server-wtwpc                                1/1     Running   0          2d4h
gke-metadata-server-x5lq8                                1/1     Running   0          2d1h
gke-metadata-server-ztr2k                                1/1     Running   0          2d1h
gke-metrics-agent-2gzpp                                  1/1     Running   0          2d4h
gke-metrics-agent-79mds                                  1/1     Running   0          2d4h
gke-metrics-agent-9scsb                                  1/1     Running   0          2d1h
gke-metrics-agent-c9w7v                                  1/1     Running   0          2d1h
gke-metrics-agent-n725x                                  1/1     Running   0          43h
gke-metrics-agent-pxh8v                                  1/1     Running   0          2d1h
gke-metrics-agent-tfml4                                  1/1     Running   0          2d1h
ip-masq-agent-d5vm4                                      1/1     Running   0          2d4h
ip-masq-agent-h2w57                                      1/1     Running   0          2d1h
ip-masq-agent-jzg6q                                      1/1     Running   0          2d4h
ip-masq-agent-mmx9d                                      1/1     Running   0          2d1h
ip-masq-agent-tmb8p                                      1/1     Running   0          43h
ip-masq-agent-wsnwz                                      1/1     Running   0          2d1h
ip-masq-agent-z5hqt                                      1/1     Running   0          2d1h
konnectivity-agent-775dbdcf94-5gst4                      1/1     Running   0          2d4h
konnectivity-agent-775dbdcf94-89hvg                      1/1     Running   0          2d1h
konnectivity-agent-775dbdcf94-bx6cr                      1/1     Running   0          2d1h
konnectivity-agent-775dbdcf94-f529g                      1/1     Running   0          2d1h
konnectivity-agent-775dbdcf94-m54rr                      1/1     Running   0          2d1h
konnectivity-agent-775dbdcf94-nvc5r                      1/1     Running   0          2d4h
konnectivity-agent-autoscaler-658b588bb6-9jr6d           1/1     Running   0          2d4h
kube-dns-598f9895c6-2x9tj                                4/4     Running   0          2d4h
kube-dns-598f9895c6-j9jdx                                4/4     Running   0          2d4h
kube-dns-autoscaler-fbc66b884-d9cvs                      1/1     Running   0          2d4h
l7-default-backend-6b99559c7d-pgwdh                      1/1     Running   0          2d4h
metrics-server-v0.5.2-9b67f66b8-fbgwf                    2/2     Running   0          2d1h
netd-4fldl                                               1/1     Running   0          43h
netd-7hn2l                                               1/1     Running   0          2d4h
netd-hwzpg                                               1/1     Running   0          2d1h
netd-j768z                                               1/1     Running   0          2d1h
netd-l79zq                                               1/1     Running   0          2d1h
netd-n5fjg                                               1/1     Running   0          2d1h
netd-zsvgm                                               1/1     Running   0          2d4h
node-local-dns-4nc8r                                     1/1     Running   0          2d1h
node-local-dns-86kbb                                     1/1     Running   0          2d4h
node-local-dns-f6tck                                     1/1     Running   0          2d1h
node-local-dns-h2jpc                                     1/1     Running   0          2d1h
node-local-dns-hx9kx                                     1/1     Running   0          43h
node-local-dns-qzmwd                                     1/1     Running   0          2d4h
node-local-dns-w2l7n                                     1/1     Running   0          2d1h
pdcsi-node-55b7c                                         2/2     Running   0          2d4h
pdcsi-node-hr4pr                                         2/2     Running   0          2d1h
pdcsi-node-hwfvd                                         2/2     Running   0          2d4h
pdcsi-node-mnddl                                         2/2     Running   0          2d1h
pdcsi-node-r5tpx                                         2/2     Running   0          2d1h
pdcsi-node-znhw7                                         2/2     Running   0          2d1h
pdcsi-node-zt8c2                                         2/2     Running   0          43h
```

 

Ele vai listar todos os Pods que estao rodando no kube-system. E até agora nós nao havíamos visto, de uma forma geral, esses namespaces rodando pq eles estao separados por namespaces. E dessa forma fica muito melhor de conseguirmos trabalhar e separar, de fato, de forma geral as nossas aplicações.

Mas como criuamos um namespcxae utilizando o k8s? 

Veja

```bash
❯ kubectl create ns dev 
namespace/dev created
```

Pronto! Vamos ver

```bash
❯ kubectl get ns
NAME              STATUS   AGE
cert-manager      Active   43h
default           Active   2d4h
dev               Active   28s
ingress-nginx     Active   2d1h
kube-node-lease   Active   2d4h
kube-public       Active   2d4h
kube-system       Active   2d4h
```

Aora vemos que temos um namespace dev que podemos trabalhar. Agora, todas as vezes que formos fazer e subir um novo deployment, basta identificarmos em qual namespace queremos trabalhar! 

```bash
❯ kubectl apply -d k8s/.yaml -n=dev 
```

E entao o k8s fará o provisionamento do manifesto no nsmaspace dev! 

Também, o que podemos fazer, é que quando estamos em um arquivo de deployment, quando chega ali nos metadados (metadata), podemos colocar o nome do namespace que queremos realizar o apply.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: dev
  name: goserver
  labels:
    app: goserver
spec:
  selector:
    matchLabels:
      app: goserver
  replicas: 1
  template:
    metadata:
      labels:
        app: "goserver"
    spec:
      containers:
        - name: goserver
          image: "rogeriocassares/hello-go:v9.7"

          resources:
            requests:
              cpu: "0.05"
              memory: 20Mi
            limits:
              cpu: "0.05"
              memory: 25Mi

          startupProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 30
            failureThreshold: 1
            # initialDelaySeconds: 10

          readinessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 3
            failureThreshold: 1
            # initialDelaySeconds: 10

          livenessProbe:
            httpGet:
              path: /healthz
              port: 8000
            periodSeconds: 5
            failureThreshold: 1
            timeoutSeconds: 1
            successThreshold: 1
            # initialDelaySeconds: 15

          envFrom:
            - configMapRef:
                name: goserver-env
            - secretRef:
                name: goserver-secret

          volumeMounts:
            - mountPath: "/go/myfamily"
              name: config
              readOnly: true
            - mountPath: "/go/pvc"
              name: goserver-volume

      volumes:
        - name: goserver-volume
          persistentVolumeClaim:
            claimName: goserver-pvc

        - name: config
          configMap:
            name: configmap-family
            items:
              - key: members
                path: "family.txt"

```





`Enato, de forma geral, nós precisamos citar sempre o processo do ns para conseguirmos =organizar. Tudo o que fizemos até agora, ou se vamos subir algo muito simples no k8s, nao tem problemna colocar no default. Mas, vioa de regra, é recomendado que exista um namespace por projeto! 

De qualquer forma a criatividade é o limite! Entao podemos organizar por namespaces por projeto, por namespace chamdo dev-proj1. dev-proj2 e dev-proj3 com prod-proj1, prod-prod2 e prod-proj3, por exemplo. Ou somente dev com tudo o que roda como dev e prod com tudo o que rodamos como prod.

Agora vamos criar um novo deployment para falar inclusive sobre segurança! POis o k8s vem bem aberto na parte de segurança e isso pode trazer alguns riscos para nós.



### Contextos por namespace

Agora vmaos ver como trabalhar com namespace no nosso dia a dia sem confundir qualk ambiente que estamos no nosso monento. 

Vamos imaginar qquer queremos fazer um deployment. Na hora que formos fazer o deployment, passamos o namespace que queremos. Mas e se esquecemos de passar, o que vai acontecer? Ja pensou se estamos fazendo um deployment que é para ser feito no dev e sem querer fazemos no ambiente de produçao?

Entao o que faremos é criar um deployment bem simples e vamos explorar os secursos de namespaces.

Vamos criar um diretorio namespaces e um arquiovo de deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server
spec:
  selector:
    matchLabels:
      app: server
  template:
    metadata:
      labels:
        app: server
    spec:
      containers:
      - name: server
        image: rogeriocassares/hello-express
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3000

```



Vamos aplicá-lo!

```bash
❯ kubectl apply -f deployment.yaml 
Warning: Autopilot set default resource requests for Deployment default/server, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/server created
```

Pronto! Criamos o nosso deployment

```bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-546c7b4948-qnmz2   1/1     Running   0          2d2h
mysql-0                     1/1     Running   0          2d2h
mysql-1                     1/1     Running   0          2d2h
server-6cd8bdcb98-826cl     1/1     Running   0          53s
```



Entretanto, temos ele rodfando no namespace default!

Vamos agora rodar o mesmo comando com -n=dev

❯ kubectl apply -f deployment.yaml -n=dev
Warning: Autopilot set default resource requests for Deployment dev/server, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/server created

O que fizemos agora foi criar esse mesmo POd / deployment no novo namespace dev.

Agora vamos criar um namespace  de produçao

```bash
❯ kubectl create namespace prod
namespace/prod created
```

E vamos rodar esse mesmo comando do deployment no namespace prod!

```bash
❯ kubectl apply -f deployment.yaml -n=prod
Warning: Autopilot set default resource requests for Deployment prod/server, as resource requests were not specified. See http://g.co/gke/autopilot-defaults.
deployment.apps/server created
```



Vamos listar os pods no namespace default

```bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS   AGE
goserver-546c7b4948-qnmz2   1/1     Running   0          2d2h
mysql-0                     1/1     Running   0          2d2h
mysql-1                     1/1     Running   0          2d2h
server-6cd8bdcb98-826cl     1/1     Running   0          7m8s
```

Agora, vamos listar  o namespace dev

❯ kubectl get po -n=dev
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-rqhz9   1/1     Running   0          3m47s

E se colcoarmos prod

```bash
❯ kubectl get po -n=prod
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-s2ggl   1/1     Running   0          2m25s
```

Ele traz o mesmo ambiente no namespace de produçao! ENtao poderiamos fazer essa separaçao.

COmo podemos ver todos os deployments juntios?

Lembra-se que trabalhamos tanto com labels nos squivos de maifestos .yaml? Entao poderiamos trabalhar e fazermos um filtro pelas labels!

Vamos ver com as labels como server

```bash
❯ kubectl get po -l app=server
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-826cl   1/1     Running   0          11m
```



Entao aqui conseguimos ver todos os Pods em que o app é igual a server.





Agora, tem umas coisas muito interessantes que devemos levar em consifderaçao!

Vamos ver o sqguinte:

Vamos imaginar que estamos trabalhando no nosso dia a dia e queremos pegar os Pos do dev

```bash
❯ kubectl get po -n=dev
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-rqhz9   1/1     Running   0          35m
```

Agora vamos rodar um deployment

```bash
❯ kubectl apply -f deployment.yaml 
Warning: Autopilot increased resource requests for Deployment default/server to meet requirements. See http://g.co/gke/autopilot-resources.
deployment.apps/server configured
```

OH naoo! Era para rodarmos no ambiente de dev e nao de produçao!

E agora? 

Vemos que quando começamos a trabalhar dessa forma começamos a ter um perigo muito grande. E é por conta disso que a gente pode utilizar um recurso do k8s que é bem interessante e que é chamado de contextos.

Quando estamos trabalhando com o k8s, existe um arquivo que é chamado de config

❯ cat ~/.kube/config

Esse arquivo traz um arquivo tb com toidas as  credenciais que estamos trabalhando no k8s.

O que podemo tb fazwer para ver essas configuraçoes?

POdemos verificar atraves dos comanbdos do kubectl.

```bash
❯ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://34.151.193.17
  name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://34.27.46.86
  name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://0.0.0.0:35809
  name: k3d-fullcycle
contexts:
- context:
    cluster: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
    user: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
- context:
    cluster: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
    user: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
  name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
- context:
    cluster: k3d-fullcycle
    user: admin@k3d-fullcycle
  name: k3d-fullcycle
- context:
    cluster: k3d-fullcycle-k3d
    user: admin@k3d-fullcycle-k3d
  name: k3d-fullcycle-k3d
- context:
    cluster: kind-fullcycle
    user: kind-fullcycle
  name: kind-fullcycle
current-context: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
kind: Config
preferences: {}
users:
- name: admin@k3d-fullcycle
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: admin@k3d-fullcycle-k3d
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args: null
      command: gke-gcloud-auth-plugin
      env: null
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      interactiveMode: IfAvailable
      provideClusterInfo: true
- name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args: null
      command: gke-gcloud-auth-plugin
      env: null
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      interactiveMode: IfAvailable
      provideClusterInfo: true
- name: kind-fullcycle
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```



E quando damos esse comando acima, eke traz as configuraçoes do cluster que estamos utilizando do GKE, nesse caso. Isso ;e um ponto. Mas essas configuraçoes, por padrao, elas fazem com que o nosso contexto principal entre direto no nosso namespace default e veja tudo o que está lá/

MAs o que podemos fazer? POdemos criar novos contexto para nos ajudar no dia a dia!

Vamos analisar o comando abaixo

❯ kubectl config current-context
gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3

o K8s nos mostra o nome do nosso  contexto atual que é o descrito acima. 

Vamos configurar um contexto agora com o nosso cluster e usuário atuais!

```bash
❯ kubectl config set-context dev --namespace=dev --cluste
r=gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3 --user=gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
Context "dev" created.
```

Nesse momento, criamos um contexto chamado dev. Agora vamos criar um contexto chamado prod!

```bash
❯ kubectl config set-context prod --namespace=prod --cluster=gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3 --user=gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
Context "prod" created.
```

Pronto!

Agora temos dois contextos! Um chamado dev e o outro chamado prod. 

Entao, quamdo damos um

❯ kubectl config view       
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://34.151.193.17
  name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://34.27.46.86
  name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://0.0.0.0:35809
  name: k3d-fullcycle
    contexts:
- context:
    cluster: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
    namespace: dev
    user: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  name: dev
- context:
    cluster: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
    user: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
- context:
    cluster: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
    user: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
  name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
- context:
    cluster: k3d-fullcycle
    user: admin@k3d-fullcycle
  name: k3d-fullcycle
- context:
    cluster: k3d-fullcycle-k3d
    user: admin@k3d-fullcycle-k3d
  name: k3d-fullcycle-k3d
- context:
    cluster: kind-fullcycle
    user: kind-fullcycle
  name: kind-fullcycle
- context:
    cluster: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
    namespace: prod
    user: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  name: prod
    current-context: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
    kind: Config
    preferences: {}
    users:
- name: admin@k3d-fullcycle
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: admin@k3d-fullcycle-k3d
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: gke_brilliant-tide-369315_southamerica-east1_autopilot-cluster-3
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args: null
      command: gke-gcloud-auth-plugin
      env: null
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      interactiveMode: IfAvailable
      provideClusterInfo: true
- name: gke_brilliant-tide-369315_us-central1_autopilot-cluster-1
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args: null
      command: gke-gcloud-auth-plugin
      env: null
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      interactiveMode: IfAvailable
      provideClusterInfo: true
- name: kind-fullcycle
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED

```bash

```

Vemos que nessas configuraçoes vamos poder trabalhar com um contexto chamado prod e um contexto chamado dev.

Agora as coisas ficam um pouco mais simples, pois podemos executar 

❯ kubectl config use-context dev
Switched to context "dev".

E agora estamos no contexto dev.  Vamos verificar

```bash
❯ kubectl config current-context
```

Agora se dermos 

```bash
❯ kubectl get po
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-rqhz9   1/1     Running   0          58m
```

Estamos vendo que ele está mostrando o nosso Pod que estava rodando agora ali.

Agora vamos fazer o deletar o deploymente server onde o namespace é o default

```bash
kubectl delete deployment server -n=default 
deployment.apps "server" deleted
```

E tb vamos matar onde o namespace é igual a prod

```bash
❯ kubectl delete deployment server -n=prod   
deployment.apps "server" deleted
```

Vamos verificar os Pod que estao rodando no nosso namespace

```bash
❯ kubectl get po
NAME                      READY   STATUS    RESTARTS   AGE
server-6cd8bdcb98-rqhz9   1/1     Running   0          62m
```

O nosso Pod ainda está rodando pois estamos em modo de dev!

Agora vamos mudar de contexto para o prod

```bash
❯ kubectl config use-context prod
Switched to context "prod".
```

Vamos verificar os Pods rodando nesse contexto que se refere ao namespace prod:

```bash
❯ kubectl get po
No resources found in prod namespace.
```

E nao veio nada aqui no nosso namespace Por pq estamos por padrao no nosso namespace PRod!

ISSO AQUI AJUDA DEMAIS QUANDO ESTAMOS TRABALHANDO NO MODO DE PRODUÇAO E NO MODO DE DESENVOLVIMENTO QUANDO ESTAMOS UTILIZANDO O K8S NO DIA A DIA!

ENTAO SE ESTAMOS FAZENDO ESSE TIPO DE SEPARAÇAO, DEVEMOS TRABALHAR MINIMAMENTE COM ALGUMACOISA DESSE TIPO POIS VAI FACILITAR MUOITO A NOSS VIDA POIS CONSEGUIMOS TER ESSAS DIFERENÇARS E CONBFIGURAR ISSO PARA O NOSSO AMBIENTE.



### Entendendo Service Accounts

Ua consideraçcao extremamente importante que devemoster e lembrar sempre antes de subir umaaplicaçcao no ar com ok8s. O que acontece é que quandosubimosuma aplica]cao nok8s, essa aplicaçao, de um forma ou de outra, precisa teruma permissao para rodar dentro do k8s.

Entao, vamos imaginar que toda vez que subimos um deployment, um Pod ou qualquer coisa desse tipo, esse caa está sendo executado no k8s. O que aconteceé que toda vez queentramos nesse Pd, esse Pod, de alguma forma necessita ter credenciais para estar rodando ali 

Entao tudo isso é feito atraves de algo que chamamos de SERVICE ACCOUNT, ISTO É, UMA CONTA DE SERVIÇO. qUALQUER COISA QUE SUBIRMOS NO k8S podemos pegar, por exemplo, um Deployment e falar que a sua conta de serviço é x e a parir das permissoes que existem naquela conta de serviço, aquele Pod vai poder secomunicar de alguma forma com a API do k8s.

Entao isso significa que,  imaginamos que sbimos a nossa aplicaão de forma tudo certinha. E alguem entao consegue invadir essanossa aplicaçao. E uma vez que ela invada essa aplicaçao, ela pode tentar falar com a API do k8s. E se ela falar com a API do k8s, ela pode, por exemplo, pegar, os Pods, deletá-los, criar deployments, rodar comandos nos nossos POds, e o pior de tudo isso é que ela consegue ir escalando tudo isso para acessar os Services, acessar o kube-system, acessar outros namespaces e dai ja da para perceber que basta uma aplicaçao ter algum problema de segurança que a pessoa ou o hacker vai conseguir escalar o acesso para o nosso cluster inteiro! 

NIsso já da para perceber o quao grave é isso!

Por padra, o k8s já possui uma Service Account que ele utiliza para todos os depoyments que fazemos,

```bash
❯ kubectl get serviceaccounts 
NAME      SECRETS   AGE
default   1         78d

```

Entao, tudo o que estamos criando no k8s, seja Deployments ou qualquer outra coisa, tudo isso está sendo gerado atraves do Service Account DEFAULT! 

Entretanto, existe um pequenino problema quando estamos rodando como Service Account Default no k8s. O problema é que a Service Account Default nos permite fazer tudo!

Isso significa que se alguem entrar na nossaaplica~cao que esteja utilizando a serviceaccount padrao, simplesmente essa pessoa vai conseguir fazer tudo! Vale a paena criar entao uma Service Account por projeto e ai cria-se uma permissao especifica para aquele serviço possa apenas, por exemplo, ler os Pods, listar os pods, paraqueee possa teruma acesso bem limitado e ele nao consiga ir mais para frente e nem executar operaçes em nosso cluster. 



Vejamos:

```bash
❯ kubectl get po
NAME                        READY   STATUS      RESTARTS        AGE
goserver-64695797bb-lnqqh   0/1     Error       53 (135m ago)   78d
mysql-2                     0/1     Pending     0               40m
goserver-64695797bb-lfqt9   0/1     Error       0               45m
goserver-64695797bb-8vs7c   0/1     Pending     0               40m
mysql-1                     0/1     Pending     0               39m
mysql-3                     0/1     Completed   0               45m
mysql-0                     0/1     Pending     0               39m

```

Vamos pegar um Pod e descrevê-lo.

```bash
❯ kubectl describe po goserver-64695797bb-lnqqh
Name:             goserver-64695797bb-lnqqh
Namespace:        default
Priority:         0
Service Account:  default
Node:             k3d-fullcycle-server-0/172.21.0.3
Start Time:       Mon, 21 Nov 2022 10:58:19 -0300
Labels:           app=goserver
                  pod-template-hash=64695797bb
Annotations:      <none>
Status:           Failed
Reason:           Evicted
Message:          The node was low on resource: ephemeral-storage. Container goserver was using 36Ki, which exceeds its request of 0. 
IP:               10.42.0.232
IPs:
  IP:           10.42.0.232
Controlled By:  ReplicaSet/goserver-64695797bb
Containers:
  goserver:
    Container ID:   containerd://efbbad677de7045b7da0ab79c2ceae8456a04b0e181db2629cdf9ae3f1411ac8
    Image:          rogeriocassares/hello-go:v9.7
    Image ID:       docker.io/rogeriocassares/hello-go@sha256:a78e4c05be9147e62eb9f19ae0bc26c9d1404605fa07dde9a9fb9a0416fa4da6
    Port:           <none>
    Host Port:      <none>
    State:          Terminated
      Reason:       Error
      Exit Code:    2
      Started:      Tue, 07 Feb 2023 19:18:14 -0300
      Finished:     Tue, 07 Feb 2023 20:48:32 -0300
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Sat, 04 Feb 2023 16:30:42 -0300
      Finished:     Tue, 07 Feb 2023 19:17:39 -0300
    Ready:          False
    Restart Count:  53
    Limits:
      cpu:     50m
      memory:  25Mi
    Requests:
      cpu:      50m
      memory:   20Mi
    Liveness:   http-get http://:8000/healthz delay=0s timeout=1s period=5s #success=1 #failure=1
    Readiness:  http-get http://:8000/healthz delay=0s timeout=1s period=3s #success=1 #failure=1
    Startup:    http-get http://:8000/healthz delay=0s timeout=1s period=30s #success=1 #failure=1
    Environment Variables from:
      goserver-env     ConfigMap  Optional: false
      goserver-secret  Secret     Optional: false
    Environment:       <none>
    Mounts:
      /go/myfamily from config (ro)
      /go/pvc from goserver-volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-nqt7v (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  goserver-volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  goserver-pvc
    ReadOnly:   false
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      configmap-family
    Optional:  false
  kube-api-access-nqt7v:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason       Age                From     Message
  ----     ------       ----               ----     -------
  Warning  Evicted      46m                kubelet  The node was low on resource: ephemeral-storage. Container goserver was using 36Ki, which exceeds its request of 0.
  Normal   Killing      46m                kubelet  Stopping container goserver
  Warning  FailedMount  42m (x5 over 43m)  kubelet  MountVolume.SetUp failed for volume "config" : object "default"/"configmap-family" not registered
  Warning  FailedMount  42m (x5 over 42m)  kubelet  MountVolume.SetUp failed for volume "kube-api-access-nqt7v" : object "default"/"kube-root-ca.crt" not registered
```

E se percebemos aqui, vemos que ele está montando um Volume dentro do nosso Pod! Isso porque além dos volumes que costumamos configurar, ele monta, automaticamente um outro volume em Mounts! E esse volume está na seguinte pasta /var/run/secrets/kubernetes.io/serviceaccount e ele está pegando, muito provavelmente os valores que estao dentro do segredo que o acompanha em kube-api-access-nqt7v dentro do serviceaccount default.

Vamos dar uma explorada nele!

```bash
❯ kubectl exec -it goserver-64695797bb-lnqqh -- bash
/usr/src/app# cd /var/run/secrets/kubernetes.io/serviceaccount
/var/run/secrets/kubernetes.io/serviceaccount# ls
ca.crt  namespace  token
```

Entao, nessa pasta, temos acesso ao qual certificado que ele está usando no k8s, acesso a qual namespace esse Pod esta rodando, e tambem acesso ao token JWT que o k8s vai utilizar para conseguirmos fazer as chamadas na API.

O problemaé que esses arquivos eles fazem parte do nosso Service Account default, que permite, simplesmente, qualque um fazer o que quiser, isto ẽ, com essas informaçoes, ele consegue simplesmente executar comandos na API do k8s sem nenhum problema ou dificuldades. Entao preceisamos começar a ter um fator de limite para que todo mundo, de uma forma ou de outra, que consiga invadir a nossa aplicaçao, nao possa escalar. Isto ẃ, queremos reduzir esse risco!

Vamos entao aprender a fazer e a atribuir uma service ccount para o nosso deployment e criar todas as permissoes que precisamos. 



### Criando Service Account e Roles



Agoravamos criar uma niva Service Account, permissões, atribuir para o nosso deployment, e fazer tudo funcionar!

Dentro de namespaces vamos criar um aquivo chamado security.yaml.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: server
```



Vamos aplicar este arquivo 

```bash
❯ kubectl apply -f k8s/namespaces/security.yaml 
serviceaccount/server created
```

E verificar os nossos Services Accounts

```bash
❯ kubectl get serviceaccounts
NAME      SECRETS   AGE
default   1         81d
server    1         56s
```



A nossa ideia é conseguirmos sair do default e ir para o nosso serice Account. Mas nao adianta nada a gente mudar o nosso service account para o server e o service tb ter a permissao para fazer tudo! 

Entao a nossa ideia é criarmos uma role. Uma role é uma funçao, um papel. Isto é., qual é a funçcai que determinada coisa que vamos criar terá. 

O k8s trabalha com algo chamado de RBAC (rOLE Access Based). Isso significa que baseado em alguns papeis podemos dar determinado acesso para cada um. 

UM api group determina quais sao os recursos que a gente vai poder trabalhar baseado na API que esse cara trabalha.

```bash
❯ kubectl get serviceaccounts
NAME      SECRETS   AGE
default   1         81d
server    1         56s
rogerio in fullcycle-kubernetes on  main [!?] 
❯ kubectl api-resources
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
pods                              po           v1                                     true         Pod
podtemplates                                   v1                                     true         PodTemplate
replicationcontrollers            rc           v1                                     true         ReplicationController
resourcequotas                    quota        v1                                     true         ResourceQuota
secrets                                        v1                                     true         Secret
serviceaccounts                   sa           v1                                     true         ServiceAccount
services                          svc          v1                                     true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io/v1        false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io/v1                false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io/v1              false        APIService
controllerrevisions                            apps/v1                                true         ControllerRevision
daemonsets                        ds           apps/v1                                true         DaemonSet
deployments                       deploy       apps/v1                                true         Deployment
replicasets                       rs           apps/v1                                true         ReplicaSet
statefulsets                      sts          apps/v1                                true         StatefulSet
tokenreviews                                   authentication.k8s.io/v1               false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io/v1                true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io/v1                false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io/v1                false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io/v1                false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling/v2                         true         HorizontalPodAutoscaler
cronjobs                          cj           batch/v1                               true         CronJob
jobs                                           batch/v1                               true         Job
certificatesigningrequests        csr          certificates.k8s.io/v1                 false        CertificateSigningRequest
leases                                         coordination.k8s.io/v1                 true         Lease
endpointslices                                 discovery.k8s.io/v1                    true         EndpointSlice
events                            ev           events.k8s.io/v1                       true         Event
flowschemas                                    flowcontrol.apiserver.k8s.io/v1beta2   false        FlowSchema
prioritylevelconfigurations                    flowcontrol.apiserver.k8s.io/v1beta2   false        PriorityLevelConfiguration
helmchartconfigs                               helm.cattle.io/v1                      true         HelmChartConfig
helmcharts                                     helm.cattle.io/v1                      true         HelmChart
addons                                         k3s.cattle.io/v1                       true         Addon
nodes                                          metrics.k8s.io/v1beta1                 false        NodeMetrics
pods                                           metrics.k8s.io/v1beta1                 true         PodMetrics
ingressclasses                                 networking.k8s.io/v1                   false        IngressClass
ingresses                         ing          networking.k8s.io/v1                   true         Ingress
networkpolicies                   netpol       networking.k8s.io/v1                   true         NetworkPolicy
runtimeclasses                                 node.k8s.io/v1                         false        RuntimeClass
poddisruptionbudgets              pdb          policy/v1                              true         PodDisruptionBudget
podsecuritypolicies               psp          policy/v1beta1                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io/v1           false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io/v1           false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io/v1           true         RoleBinding
roles                                          rbac.authorization.k8s.io/v1           true         Role
priorityclasses                   pc           scheduling.k8s.io/v1                   false        PriorityClass
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver
csinodes                                       storage.k8s.io/v1                      false        CSINode
csistoragecapacities                           storage.k8s.io/v1beta1                 true         CSIStorageCapacity
storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass
volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachment
ingressroutes                                  traefik.containo.us/v1alpha1           true         IngressRoute
ingressroutetcps                               traefik.containo.us/v1alpha1           true         IngressRouteTCP
ingressrouteudps                               traefik.containo.us/v1alpha1           true         IngressRouteUDP
middlewares                                    traefik.containo.us/v1alpha1           true         Middleware
middlewaretcps                                 traefik.containo.us/v1alpha1           true         MiddlewareTCP
serverstransports                              traefik.containo.us/v1alpha1           true         ServersTransport
tlsoptions                                     traefik.containo.us/v1alpha1           true         TLSOption
tlsstores                                      traefik.containo.us/v1alpha1           true         TLSStore
traefikservices                                traefik.containo.us/v1alpha1           true         TraefikService
```



Quando acessarmos o api-resources, vamos ver lá em cima o Nome do recurso e o respectivo api-group. Quando nao temos o api-group podemos deixá-lo em branco.

POrexeplo, o apigroup de namespace nao existe, nem o do configmap, persistenvolumes, pods ... Mas olha sõ quem tem um apigroup: deployments. O apigroup de deployments chama apps. Entao se quisermos saber qual API group que queremos liberar para a pessoa ter acesso podemos dar uma olhada nas respostas desse comando aqui tb.

Entao, em nosso arquivo yaml, o primeiro grupo de apigroups deve ser em branco,pq vamos passar algumas regras e alguns papeis que nao existem apigroups nesse nosso caso.

Qauis sao os resources que queremos trabalhar? Quais sao os verbos que eles vao poder utilizar nesses recursos que vamos dar a ele no k8s?

Todos que tiverem essa regra vao poder acionar os recursos relacionados aos verbos 

Fazemos a mesma coisa com um apiGroup de Deployments.

COm isso estamos criando uma serie de regras que deixamos todos verem os serviços, ver os pods e ver os deployments. Mas perceba que nao esta sendo permitido colocar write em nada.  E esses tipos de coisas fazem muita diferença!

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: server

---

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: server-read
rules:
- apiGroups: [""]
  resources: ["pods, services"]
  verbs: ["get", "watch", "list"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "watch", "list"]
```



Vamos verificar se tudo foi criado certinho

```bash
❯ kubectl apply -f k8s/namespaces/security.yaml
serviceaccount/server unchanged
role.rbac.authorization.k8s.io/server-read created
```

Masainda nao está pronto pq ainda precisamos pegar esse papel/role e atribuir para a nossa serviceaccount, isto é, precisamos fazer um bind, ou seja, indicar a cada sericeaccount qual role ele deve seguir!

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: server

---

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: server-read
rules:
- apiGroups: [""]
  resources: ["pods, services"]
  verbs: ["get", "watch", "list"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "watch", "list"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: server-read-bind
subjects:
- kind: ServiceAccount
  name: server
  namespace: prod
roleRef:
  kind: Role
  name: server-read
  apiGroup: rbac.authorization.k8s.io



```

Com isso, criamos a nossa serviceaccount, criamos a nossa role e agora atribuimos a nossa role ao nosso serviceaccount. Entao vamos ver se vai funcionar.

```bash
❯ kubectl apply -f k8s/namespaces/security.yaml
serviceaccount/server unchanged
role.rbac.authorization.k8s.io/server-read unchanged
rolebinding.rbac.authorization.k8s.io/server-read-bind created
```



Estã agora funcionando? Nao ainda! Pq ainda temos que falar para o nosso deployment que a serviceaccount que ele vai utilizar agora é o server!

Enatao vamos configurar o nosso deployment e, em cima das nossas especificaçoes, antes do container, inserimos a configuraçao de sericeaccount:  server

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server
spec:
  selector:
    matchLabels:
      app: server
  template:
    metadata:
      labels:
        app: server
    spec:
      serviceAccount: server
      containers:
      - name: server
        image: rogeriocassares/hello-express
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3000

```

Agora, nesse deploment estamos falando: Quando esse deployment for criado, a serviceAccount desse deployment que vai utilizar, os pods que vao ser criados em cima dessa serviceaccount vai ser o server. E o server como serviceaccount vai ter a role chamada de server-read, que dá acesso a nós listarmos, tantos os nossos pods, como os nossos services etc. Fora isso nao dá mais para fazer nada. 

Em tese, agora, a nossa aplicaçao, se alguem for ter acesso a ela, nao iria conseguir escalar esse acesso para o restante do nosso cluster k8s.

Vamos aplicar:

```bash
❯ kubectl apply -f k8s/namespaces/deployment.yaml 
deployment.apps/server created
```

```bash
❯ kubectl get po
NAME                        READY   STATUS    RESTARTS        AGE
...
server-7d6c74d698-n7xsv     1/1     Running   0               79s
```

Vamos descrevê-lo

```bash
❯ kubectl describe po server-7d6c74d698-n7xsv
Name:             server-7d6c74d698-n7xsv
Namespace:        default
Priority:         0
Service Account:  server
Node:             k3d-fullcycle-server-0/172.21.0.2
Start Time:       Fri, 10 Feb 2023 21:33:14 -0300
Labels:           app=server
                  pod-template-hash=7d6c74d698
Annotations:      <none>
Status:           Running
IP:               10.42.0.14
IPs:
  IP:           10.42.0.14
Controlled By:  ReplicaSet/server-7d6c74d698
Containers:
  server:
    Container ID:   containerd://a75dc1261860b73c218bc8d183f831a6f247f003b70b908b9a31bf47811457e3
    Image:          rogeriocassares/hello-express
    Image ID:       docker.io/rogeriocassares/hello-express@sha256:06997702425f89e928b0bc89ca3aa8db5c2b8489ed0d256607d822e5c2a07d43
    Port:           3000/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 10 Feb 2023 21:33:47 -0300
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     500m
      memory:  128Mi
    Requests:
      cpu:        500m
      memory:     128Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mxlgv (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-mxlgv:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Guaranteed
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m36s  default-scheduler  Successfully assigned default/server-7d6c74d698-n7xsv to k3d-fullcycle-server-0
  Normal  Pulling    2m24s  kubelet            Pulling image "rogeriocassares/hello-express"
  Normal  Pulled     2m4s   kubelet            Successfully pulled image "rogeriocassares/hello-express" in 20.237085134s
  Normal  Created    2m4s   kubelet            Created container server
  Normal  Started    2m4s   kubelet            Started container server
```



E agora vamos ver se ,udou em Mounts e vimos que ele nao esta mais usando o default, e sim o token! 

Entao se alguem agora de dentro daquele nosso Pod tentar acessar aapi do k8s simplesmente nao vai conseguir pq ele nao tem permissoes para isso. 

E somente de fazer isso, mesmo sendo algo muito simples como copiar, colar e aplicar em nossos pods, já teremmos uma super paz de espirito pq no minimo trouxemos um poc mais de segurança para o nosso cluster k8s.



### Cluster Role

Algumas diferenças sutis que temos nos Roles e no RoleBinding.

O que que acontece é que quando fizemos as configurações e criamos o nsso Role server-read etc, ele dá acesso aos componentes dos apiGroups baseados dentro do namespace que estávamos colocando pq o role vai trabalhar apenas no namespace que utilizamos ali por padrao. 

Agora, e se quiséssemos fazer isso em relacao ao cluster como um todo? E se quiséssemos que em algum ommento todo mundo que tivesse ali, por exemplo em nosso serviceaccount server pudesse listar, watch, get e pegar as informaçoes de todos os pods que ele está trabalhando? COmo conseguiriamos ampliar todo esse escopo ali para gente. 

Nesse caso temos um outro tipo de role que consegumis fazer e isso acaba expandindo de forma geral para todo o nosso cluster e nao somente ali focado em nosso namespace. Para isso, o objeto é similar, mas outro. 



Ao inves de colocarmos Role, colocaremos ClusterROle. Entao esse grpo de permissionamento que estamos colocando é agora em nivel de CLuster!

 E a mesma coisa ali no RoleBinding! COlocaremos ClusterRoleBinding.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: server

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: server-read
rules:
- apiGroups: [""]
  resources: ["pods, services"]
  verbs: ["get", "watch", "list"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "watch", "list"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: server-read-bind
subjects:
- kind: ServiceAccount
  name: server
  namespace: prod
roleRef:
  kind: ClusterRole
  name: server-read
  apiGroup: rbac.authorization.k8s.io



```

Entao quando aplicamos esse arquivo, esse novo nivel de permissao começa a ser aplicado a nivel de cluster.

```bash
❯ kubectl apply -f k8s/namespaces/security.yaml  
serviceaccount/server unchanged
clusterrole.rbac.authorization.k8s.io/server-read created
clusterrolebinding.rbac.authorization.k8s.io/server-read-bind created
```



Agora, por exemplo, se alguem acessar o nosso pod e tentar pegar quais sao os Pods que estao no namespace kubesystem, esse cara vai conseguir pq estamos utilizando o CLusterROle, isto é, dando acesso ao cluster de forma geral e nao mais somente àquele namespace. 

Esse tipo de coisa é muito importante entendermos pelo menos as diferen~ca. 

COmo devs de forma geral nao precisamos ir no detalhe, mas tratando0se dessa segurança basica, que nao é algo tao dificil, vale muito apena entendermos e como podemos fazer.

De forma geral, o Wesey costuma trabalhar apenas com Role e RoleBinding pq estamos focando na granularidade da nossa aplicaç"ao que estamos colocando no ar.

Normalmente, se estivermos querendo gerenciar o nosso cluster de forma mais geral, que normalmente nao é dev que façam mas pode vir a ser, ai podemos trabalhar com CLusterRole e ClusterRoleBinding!

























