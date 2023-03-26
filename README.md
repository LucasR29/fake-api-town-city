##### OBS: não deletar o síndico e não commitar nesse repo (além do Lucas), fazer o clone do repo e rodar em sua máquina. Na aplicação já está setada base URL "http://localhost:3001"

### Considerações:

Todas as rotas precisam ser 660, pois o síndico precisa conseguir modificá-las (métodos de patch e delete). Com exeção de login e register, todas as rotas necessitam de token.

#### /register: 
Faz o cadastro de um usuário com os dados abaixo e retorna um objeto com as proriedades "accsessToken" e "user" (mesmas infos enviadas no cadastro, mas sem o password e com um id único gerado automaticamente. O isSyndic é adicionado no front-end apenas, se testar no insomnia fica sem)

```
{
      "email": "teste2@mail.com",
      "password": "teste",
      "name": "Teste da Silva",
			"aptNumber": "122",
			"contact": "1111111",
      "cpf": "1111111"
}
```
#### /login: 
Faz o login com as informações abaixo e retorna a mesma info retornada no cadastro.
##### Dados do síndico:

```
{
      "email": "alyssonLindao@mail.com",
      "password": "bestPassw0rd"
}
```
#### /issues: 
Registra um issue no sistema passando os dados abaixo (o user vem do state user na aplicação declarado no contexto de autenticação).É gerado um id automaticamente. Nessa rota, além do POST, teremos as opções PATCH (para o síndico poder adicionar uma resposta "response" OU alterar a propriedade "description" de seus avisos E moradores poderem alterar a propriedade "description" de suas solicitações/reclamações) e DELETE (para síndicos poderem excluir seus avisos e moradores poderem excluir suas solicitações/reclamações). No PATCH e no DELETE é preciso passar na URL o id do issue específico.

```
{
	"type": "reclamação" ou "solicitação" ou "aviso",
  "description": "descrição",
  "data": "02/11/2022",
  "user": {
    "email": "mandacosta94@gmail.com",
		"name": "Amanda Costa",
		"aptNumber": "122",
		"contact": "111111",
		"cpf": "111111",
		"isSyndic": false,
		"id": 6
      }
}
```
#### Reservas
São dois endpoints para essa feature, um para a reserva em si, com a descrição do evento e dono, e outra com as datas reservadas para alimentar o calendário. No total são 6 endpoints para 3 áreas comuns (pool, grill, saloon).
Dado às limitações poderemos fazer apenas alteração na descrição do evento e não em sua data (PATCH passando o id do evento no /area_reservation)
Para deletar uma reserva temos que fazer duas requisições de delete, uma no /area_reserved_dates e outra no /area_reservation.
Os dois endpoints geram ids únicos também.

#### /area_reserved_dates:
Para reservar uma data em uma área específica, enviar os dados abaixo:

```
{
	"year": 2022,
      "month": 11,
      "day": 15
}
```


#### /area_reservation:
Para cadastrar um evento em uma área específica enviando os dados abaixo:

```
{
	"title": "festão da carol",
	"user": {
    "email": "mandacosta94@gmail.com",
		"name": "Amanda Costa",
		"aptNumber": "122",
		"contact": "111111",
		"cpf": "111111",
		"isSyndic": false,
		"id": 6
      },
	"reserved_date": {
      "year": 2022,
      "month": 11,
      "day": 15
      }
    }
}
```

#### /users
Rota que usaremos no método delete para eliminar um usuário (função do síndico). Basta passar na URL o id do usuário que se deseja apagar dos dados usando o DELETE como verbo.
