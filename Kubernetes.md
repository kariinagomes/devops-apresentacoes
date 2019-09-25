# DevOps-Sciensa
Apresentação de Kubernetes 
***

# Introdução
### O que é Kubernetes?
  - Kubernetes é uma plataforma open-source, portatil e flexível para organizar cargas de trabalhos e serviços de "containers",isso facilita tanto automação quanto cofigurações declarativas.
### Para que usar o kubernetes?
  - Quando estamos falando de ambiente de produção, temos de assegurar de que nossas aplicações não fiquem fora do ar, não seria muito mais facil se essas informações fossem manipuladas pelo sistema?
  - O kubernetes traz para o administrador um framework para rodar sistemas distribuidos resilientes.
  - Facilitando:
  > Os requisitos de escalonamento (Pool auto-scaling)
  > Recuperação (Fail]over)
  > Padrões de deploy (Deployment patterns)
  
  # O que o kubernetes providencia?
  - Descoberta de serviços e balanceamento de cargas
	O kubernetes pode expor um container usando DNS (Domain name server) ou usando seu próprio IP.Se o tráfego estiver alto, o kubernetes distribui as cargas de forma que o deploy se mantenha estável.
  - Orquestração de armazenamento
    Permite que você monte um armazenamento de sua escolha, por exemplo: Local,Cloud e muito mais.
  - Rollout e rollback automático
    É possivel descrever o estado de seus containers do deploy, permitindo assim o kubernetes automatizar os processos de criação e remoção de containers.
  - Empacotamento automatico
    O kubernetes permite a você especificar quanta CPU e Memória cada container precisa.Quando certas requisições são especificadas, é possivel fazer decisões melhores para manipular os containers.
  - Auto-medicamento (Self-Healing)
	Kubernetes reinicia os containers que falham e substitui por outros,mata os containers que não respondem ao health check e não avisa os clientes até que esteja pronto para servir.
  - Abstração de configuração
    Permite que você guarde e manipule informações confidenciais como chaves, autenticações.Sendo possivel assim manipular seus containers de forma segura sem expor suas chaves ou autenticações na configuração.
### POD:

# pod.yml
> 	apiVersion: v1 - Versão da api, será substituida por apps/v1 futuramente.
	kind: Pod - Determinar o tipo de objeto 
	metadata: 	- Identificador para outros objetos
		name: pod-cre	- Nome do POD
		labels:			- Sub-identificadores, podem existir n labels de qualquer tipo.
			app: cre
			marca: corselLouco
	spec: 		- Escopo, contém os containers do projeto com seus respectivos nomes, imagens, portas de saidas e protocolos.
		containers:
			- name: nginx
			image: nginx
			ports:
				- name: nginxporta
				containerPort: 80
				protocol: TCP     

### DEPLOYMENT:

# deploy.yml
> 	apiVersion: apps/v1
	kind: Deployment
	metadata: -Identificador do deploy
		name: podcre-deployment
		labels:
			marca: corselLouco
	spec: - O deployment sempre pega como base um ReplicaSet, responsável por replicar em diferentes maquinas zonais ou regionais as configurações específicadas.
		replicas: 1 - Numero de réplicas
		selector: - Identificador do ReplicaSet
			matchLabels: 
				marca: corselLouco
		template: - configurações do pod
			metadata:
				labels:
					marca: corselLouco        
			spec:
				containers:
				- name: cre
				  image: nginx
				  ports:
				  - containerPort: 80		
### SERVICE:

# service.yml
> 	apiVersion: v1
	kind: Service
	metadata:
		name: apresentacaoservice
		labels:
			owner: leonardonaoki
	spec: - Especificações do objeto, pode ser um deploy ou um pod. Neste caso, um deploy
		selector:
			marca: corselLouco
		ports:
			- protocol: TCP 
			  port: 80
			  targetPort: 80
		type: LoadBalancer - "LoadBalancer" - possui IP Externo, balanceamento de carga e o ideal para testar em dev.
						   - "ClusterIP" - possui somente IP interno, concentra o seu cluster para ser reconhecido somente por um endereço. Usamos para comunicação interna, DEFAULT.
						   - "NodePort" - possui somente IP interno, configura o seu cluster para sair somente por uma porta de uma máquina específica, ideal para concentrar serviços em uma máquina mais potente ou segura.Necessário definir porta.
						   
> Comandos imperativos:
	
- kubectl expose [deployment, pod] "NomeDoObjeto" --type=[LoadBalancer,ClusterIP(default),NodePor] // equivalente ao yml acima.

						   
### VIRTUAL SERVICE

#virtualservice.yml

> 	apiVersion: networking.istio.io/v1alpha3
	kind: VirtualService - CRD, Custom resource definition - kind personalizado, usamos para disponibilizar o CLUSTERIP(IP Interno) para a internet através de roteamento do ISTIO.
	metadata:
		name: kibana
		namespace: log - NameSpace é um espaço de trabalho paralelo a zona do seu cluster.
	spec:
		hosts: - É onde fica o endereço WEB para consulta.
		 - "kibana-log.${DOMAIN}"
		gateways: - É o responsavel por dar acesso a internet e rotear.
		 - public-gateway.istio-system.svc.cluster.local
	http:
	- route:
		- destination: - É onde aponta para seu objeto, através do identificador, que pode ser um service.
			host: kibana-logging						   

### Kubernetes comandos ou informações adicionais 

- Kubectl - [GET/DELETE/APPLY -F "YML"], dentre outros

Após conectar no cluster, ele te fornece um CLI para manusear seu cluster.



			
### Conceitos aprendidos

- Istio (Service mesh)

Em suma é uma concentração de serviços , dentre eles, garantir o roteamento da aplicação, garantir segurança, aplicar politicas, etc.

- Helm (Package manager)

Manipulador de pacotes do kubernetes, seria o equivalente ao NPM.

- Cert-Manager(Let's Encrypt)

Responsável por garantir os certificados e as chaves para garantir segurança e encriptação.

- Zona x Região
  
Zona: Spot dentro de uma região.
	Possui um master (No caso a google) e quantos workers forem definidos.
Região: Possui varias zonas
	Possui um master para cada região e quantos workers forem definidos.

OBS: os workers são definidos pelos POOL's que podem ser específicos dependendo da sua demanda de máquinas.
