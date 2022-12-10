# Integração Continua ou CI

- É o processo de integrar modificações do codebase de forma contínua e automatizada, evitando assim erros humanos de verificação, garantindo mais agilidade e segurança no processo de desenvolvimento de um software
- Principais processos:
    - Execução de testes
    - Linter
    - Verificação de qualidade de código
    - Verificação de segurança
    - Geração de artefatos prontos para o processo de deploy
    - Identificação de próxima versão a ser gerada no software
    - Geração de tags e releases
- Status Check
    - É a garantia de que uma Pull Request não poderá ser mergeada ao repositório sem antes ter passado pelo processo de CI ou mesmo no processo de Code Review
- Ferramentas
    - Jenkins
    - Github Actions
    - Circle CI
    - AWS Code Build
    - Azure DevOps
    - Google Cloud Build
    - GitLab Pipelines/CI
- Dinâmica
    - Workflow:
        - São conjuntos de processos definidos por você
        - É possivel ter mais doq eu um workflow por repositório
        - Definidos em arquivos “.yml” em .github/workflows
        - Possui um ou mais “Jobs”
        - É iniciado baseado em eventos do GitHub ou através de agendamento
        - Evento → Filtros → Ambiente → Ações
- Actions:
    - É a ação que de fato será executada em um dos Steps de um Job em um Workflow. Ela pode ser criada do zero ou ser reutilizada de actions pre-existentes
    

[Documentação do GitHub Actions - GitHub Docs](https://docs.github.com/pt/actions)

# Configurações

No Projeto:

- Criar a pasta .github/workflows
- Criar o arquivo ci.yaml
- 

```yaml
name: [NOME DA INTEGRAÇÃO]
on: [PROCESSOS QUE EXECUTARAM A INTEGRAÇÃO]
	pull_request: [REGRA PARA BRANCHS EXPECIFICAS]
		branchs: 
			- develop
jobs: [JOBS EXECUTADOS]
	check-application:
		runs-on: ubuntu-latest [ONDE SERÀ EXECUTADO O JOB]
		strategy: [INFORMA DIFERENTES LOCAIS DE TESTE COMO VERSÕES]
			matrix: 
				go: ['1.14', '1.15']
		steps: 
			- uses: action/checkout@v2 [ACTIONS DO GITHUB]
			- uses: action/setup-go@v2
				with: 
					go-version: ${{matrix.go}} ou [INFORMAR A VERSAO EXPECIFICA DO TESTE]
			- run: go test [EXECUTA COMANDOS]
			- run: go run math.go

			- name: Set up QEMU [NOMEIA O PASSO EXECUTADO]
        uses: docker/setup-qemu-action@v1 [USANDO DOCKER]

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
			
			- name: Login to DockerHub
        uses: docker/login-action@v1 
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }} [CHAVES CRIADA NO PAINEL DE CONFIGURAÇÕES DO GITHUB]
          password: ${{ secrets.DOCKERHUB_TOKEN }}

			- name: Build and push
        id: docker_build [ID DO PASSO PARA PODER SER EXECUTADO EM OUTRAS STACKS]
        uses: docker/build-push-action@v2
        with:
          push: true [EFETUA O PUSH APÓS O BUILD]
          tags: [DOCKERHUB TAG]
```

[https://github.com/docker/setup-qemu-action](https://github.com/docker/setup-qemu-action)

## Qualidade de Código

[Code Quality and Code Security | SonarQube](https://www.sonarqube.org/)

- Rules: definição de regras para cada linguagem
- Quality Profiles: padrões que definem qualidades de código baseado nas regras criadas
- Quality Gate: “portã” de qualidade que determina se passa ou não

Criando Projeto

- Dentro da pasta Sonarcube crie o repositorio do projeto
- Adicione manualmente o projeto pela interface
- Gere o Token do projeto
- Informe a linguagem de programação
- Instale o SonarScanner
- Rode o comando informado pelo sonarqube

Cobertura de Códigos:

- Gere um arquivo de testes da aplicação
- Crie o arquivo [sonar-project.properties](http://sonar-project.properties) e Configure o arquivo criado
- 

Qualidade de código na cloud

[As a service | SonarCloud](https://www.sonarsource.com/products/sonarcloud/)

- Configure o projeto do github no SonarClound
- Selecione a ferramenta de integração continua
- Crie a chave informada pelo SonarClound
- Selecione a linguagem do projeto
- Configure o sonar-project.properties
