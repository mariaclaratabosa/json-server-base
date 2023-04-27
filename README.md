# json-server-base

Esse é o backend da aplicação HelPets - Uma interface voltada para doação para ONGs da causa animal.

## Endpoints

A API tem um total de 7 endpoints, sendo em volta principalmente do usuário e admin, onde o usuário pode cadastrar e logar, já o admin pode adicionar novas ONGs, editar as ONGs já existentes e também deletar.

## Rotas que não precisam de autenticação

### Listando usuários

`GET /users - FORMATO DA RESPOSTA - STATUS 200`
```json
[
  {
    "email": "kenzinho@mail.com",
    "password": "$2a$10$YQiiz0ANVwIgpOjYXPxc0O9H2XeX3m8OoY1xk7OGgxTnOJnsZU7FO",
    "name": "Kenzinho",
    "id": 1
  },
  {
    "email": "admin@mail.com",
    "password": "$2a$10$7AvSvQKCcHvKKy03l0FIw.S0iH2616PkPv6MeVm99BW.fVOppk/r.",
    "name": "admin",
    "id": 2,
    "isAdmin": true
  }
]
```

### Cadastro

`POST /users - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "kenzinho@mail.com",
  "password": "123456",
  "name": "Kenzinho",
  "id": 1
}
```

Caso dê tudo certo, a resposta será assim:
`POST /users - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "email": "maria@mail.com",
  "password": "maria10*",
  "name": "maria"
}
```

Caso você acabe errando e mandando algum campo errado, a resposta do erro será assim:
No exemplo, a requisição foi feita faltando o campo name.

`POST /users - FORMATO DA RESPOSTA - STATUS 400 `

```json
{
  "status": "error",
  "message": ["name is required"]
}
```

A senha necessita de 8 caracteres.
`POST /users - FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": ["password: minimum is 8 characters"]
}
```

Email já cadastrado
`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": ["Email already exists"]
}
```

### Login

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "kenzinho@mail.com",
  "password": "123456"
}
```

Caso tudo dê certo, a resposta será assim:
`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImtlbnppbmhvQG1haWwuY29tIiwiaWF0IjoxNjgyNTQ0NzYwLCJleHAiOjE2ODI1NDgzNjAsInN1YiI6IjEifQ.6s9HJpiHPhxrJ7dIJeukxhgfmo73EaPF0LXXE7HD5dY",
  "user": {
    "email": "kenzinho@mail.com",
    "name": "Kenzinho",
    "id": 1
  }
}
```

Com essa resposta, vemos que temos duas informações, o user e o token respectivo, dessa forma você pode guardar o token e o usuário logado no localStorage para fazer a gestão.

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

Após o usuário estar logado, ele deve conseguir visualizar as ONGs cadastradas.

### Listando ONGs

`GET /ongs - FORMATO DA RESPOSTA - STATUS 200`

```json
[
	{
		"name": "Projeto TAMAR",
		"id": 1,
		"description": "É um projeto conservacionista brasileiro que atua na preservação das tartarugas-marinhas ameaçadas de extinção. É uma entidade de direito privado, sem fins lucrativos e fica sediado na Praia do Forte, no município de Mata de São João, no interior do estado da Bahia.",
		"link": "https://www.tamar.org.br/",
		"logo": "https://www.tamar.org.br/imagens/logo_tamar.jpg",
		"userId": 2
	},
	{
		"name": "PeTA",
		"id": 2,
		"description": "People for the Ethical Treatment of Animals (PeTA), em português: Pessoas pelo Tratamento Ético dos Animais, é uma organização não governamental de ambiente (ONGA) fundada em 1980, a qual já conta com mais de 2 milhões de membros e se dedica aos direitos animais, tendo como lema: 'Animais não são nossos para comer, vestir, usar em experiências ou para entretenimento' .",
		"link": "https://www.peta.org/",
		"logo": "https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Peta_logo.svg/250px-Peta_logo.svg.png",
		"userId": 2
	},
	{
		"name": "AMPARA animal",
		"id": 3,
		"description": "Lutamos contra o abandono de animais domésticos e a negligência com os animais silvestres da nossa fauna. Não medimos esforços para amparar e dar suporte onde for necessário. Seja com suprimentos, tratamentos, campanhas de conscientização, ou até ir mata adentro para salvar vidas.",
		"link": "https://amparanimal.org.br/",
		"logo": "https://colabore-fichas-production.s3.amazonaws.com/og_image/1188/images/1249/og_image/main_AMPARAPrancheta_8_c%C3%B3pia_9_3x.png",
		"userId": 2
	}
]
```

Apenas o admin consegue adicionar, editar e deletar ongs

### Cadastro de ONGs

`POST /ongs - FORMATO DA REQUISIÇÃO`

```json
[
  {
    "name": "Projeto TAMAR",
    "description": "É um projeto conservacionista brasileiro que atua na preservação das tartarugas-marinhas ameaçadas de extinção. É uma entidade de direito privado, sem fins lucrativos e fica sediado na Praia do Forte, no município de Mata de São João, no interior do estado da Bahia.",
    "link": "https://www.tamar.org.br/",
    "logo": "https://www.tamar.org.br/imagens/logo_tamar.jpg",
  }
]
```

Caso tudo dê certo, a resposta será assim:
`POST /ongs - FORMATO DA RESPOSTA - STATUS 201`

```json
{
    "name": "Projeto TAMAR",
    "description": "É um projeto conservacionista brasileiro que atua na preservação das tartarugas-marinhas ameaçadas de extinção. É uma entidade de direito privado, sem fins lucrativos e fica sediado na Praia do Forte, no município de Mata de São João, no interior do estado da Bahia.",
    "link": "https://www.tamar.org.br/",
    "logo": "https://www.tamar.org.br/imagens/logo_tamar.jpg",
    "userId": 2,
    "id": 7
  }
```

### Editar ONGs

`PATCH /ongs/:ongId - FORMATO DA REQUISIÇÃO`

```json
[
  {
    "name": "Projeto TAMAR",
    "description": "É um projeto conservacionista brasileiro que atua na preservação das tartarugas-marinhas ameaçadas de extinção. É uma entidade de direito privado, sem fins lucrativos e fica sediado na Praia do Forte, no município de Mata de São João, no interior do estado da Bahia.",
    "link": "https://www.tamar.org.br/"
  }
]
```
Caso tudo dê certo, a resposta será assim:
`PATCH /ongs/:ongId - FORMATO DA RESPOSTA - STATUS 200`

```json
{
    "name": "Projeto TAMAR",
    "id": 7,
    "description": "É um projeto conservacionista brasileiro que atua na preservação das tartarugas-marinhas ameaçadas de extinção. É uma entidade de direito privado, sem fins lucrativos e fica sediado na Praia do Forte, no município de Mata de São João, no interior do estado da Bahia.",
    "link": "https://www.tamar.org.br/",
    "logo": "https://www.tamar.org.br/imagens/logo_tamar.jpg",
    "userId": 2
  }
```

### Deletar ONG

`DELETE /ongs/:ongId - FORMATO DA REQUISIÇÃO`

```
Não é necessário um corpo da requisição.
```
