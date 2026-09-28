APS 2 · API RESTful para gerenciar eventos acadêmicos, participantes e inscrições, desenvolvida com Python, FastAPI e Pydantic. Os dados ficam em memória (são apagados ao reiniciar o servidor).
 Estrutura (MVC)
aps2_api/
├── main.py
├── requirements.txt
├── controller/   # rotas HTTP
├── model/        # modelos e validações (Pydantic)
└── service/      # regras de negócio e armazenamento
​
 Como executar
git clone <link-do-repositorio>
cd aps2_api
python -m venv venv
venv\Scripts\activate        # Linux/macOS: source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload
​
Swagger: http://127.0.0.1:8000/docs
🔗 Rotas
Método
Rota
Descrição
Sucesso
POST
/eventos
Cadastrar evento
201
GET
/eventos
Listar eventos
200
GET
/eventos/{id}
Consultar evento
200
PUT
/eventos/{id}
Atualizar evento
200
DELETE
/eventos/{id}
Excluir evento
204
POST
/participantes
Cadastrar participante
201
GET
/participantes
Listar participantes
200
GET
/participantes/{id}
Consultar participante
200
PUT
/participantes/{id}
Atualizar participante
200
DELETE
/participantes/{id}
Excluir participante
204
POST
/eventos/{evento_id}/inscricoes/{participante_id}
Inscrever participante
201
GET
/eventos/{evento_id}/inscricoes
Listar inscritos do evento
200
 
 Validações e regras
Título, descrição, local, nome e curso não podem ser vazios.
E-mail deve ser válido; capacidade deve ser maior que zero; data e horário devem ser válidos.
Categoria: Palestra, Workshop, Minicurso, Seminário ou Competição.
Na inscrição, a API verifica se o evento existe, se o participante existe, se ele já está inscrito e se há vagas.
Excluir evento ou participante remove as inscrições relacionadas.
A capacidade não pode ser menor que o número de inscritos.
💡 Exemplos
Cadastrar evento · POST /eventos
{
  "titulo": "Workshop de Python",
  "descricao": "Introdução ao Python e FastAPI",
  "data": "2026-10-15",
  "horario": "19:00",
  "local": "Laboratório 01",
  "capacidade": 30,
  "categoria": "Workshop"
}
​
Resposta 201: os mesmos campos, com "horario": "19:00:00" e "id": 1.
Cadastrar participante · POST /participantes
{
  "nome": "Maria Silva",
  "email": "maria@email.com",
  "curso": "Análise e Desenvolvimento de Sistemas"
}
​
Inscrever · POST /eventos/1/inscricoes/1 → 201
{ "evento_id": 1, "participante_id": 1 }
​
 Erros
Situação
Status
Resposta
Evento inexistente
404
{"detail": "Evento não encontrado."}
Participante inexistente
404
{"detail": "Participante não encontrado."}
Já inscrito
400
{"detail": "Participante já está inscrito neste evento."}
Sem vagas
400
{"detail": "Não existem vagas disponíveis para este evento."}
Dados inválidos
422
Lista de campos inválidos em detail
