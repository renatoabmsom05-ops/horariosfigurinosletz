<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Marcação de Horários - Curso de Teatro</title>
<!-- Fonte divertida e alegre do Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Comic+Neue&display=swap" rel="stylesheet">
<style>
  /* Estilo geral da página */
  body {
    font-family: 'Comic Neue', cursive;
    background-color: #fffbe0; /* cor de fundo alegre */
    margin: 0;
    padding: 20px;
    color: #333;
  }

  h1 {
    text-align: center;
    color: #d23669; /* cor vibrante */
    margin-bottom: 10px;
  }

  /* Cabeçalho com elementos decorativos */
  header {
    text-align: center;
    margin-bottom: 30px;
  }

  /* Seção de marcação */
  section {
    max-width: 900px;
    margin: 0 auto;
    background-color: #fff;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }

  /* Estilo para o formulário de marcação */
  #form-marcacao {
    display: flex;
    flex-direction: column;
    gap: 15px;
    margin-bottom: 30px;
  }

  label {
    font-weight: bold;
  }

  select, button {
    padding: 10px;
    font-size: 1em;
    border-radius: 8px;
    border: 1px solid #ccc;
  }

  button {
    background-color: #d23669;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  button:hover {
    background-color: #a1224f;
  }

  /* Estilo para lista de marcações */
 #lista-marcar {
    margin-top: 20px;
  }

 table {
    width: 100%;
    border-collapse: collapse;
  }

 th, td {
    border: 1px solid #ccc;
    padding: 8px;
    text-align: center;
  }

 th {
    background-color: #f7b733;
    color: #fff;
  }

 /* Seção de visualização para professores */
 #visualizar {
   margin-top: 40px;
 }

 h2 {
   color: #d23669;
   margin-bottom: 10px;
 }

 /* Estilo para a lista de marcações dos professores */
 #lista-professor {
   max-height: 300px;
   overflow-y: auto;
   background: #f0f0f0;
   padding: 10px;
   border-radius: 10px;
 }

 /* Elementos decorativos com tema de teatro */
 .teatro {
   text-align: center;
   font-size: 1.2em;
   margin: 20px 0;
   color: #d23669;
   font-weight: bold;
 }
</style>
</head>
<body>

<header>
  <h1>🎭 Aulas de Figurino - Curso de Teatro UFU 🎭</h1>
  <div class="teatro"></div>
</header>

<section>
  <h2>Marque sua aula</h2>
  <!-- Formulário de marcação -->
  <form id="form-marcacao">
    <label for="dia">Dia da semana:</label>
    <select id="dia" required>
      <option value="">Selecione o dia</option>
      <option value="Segunda-feira">Segunda-feira</option>
      <option value="Terça-feira">Terça-feira</option>
      <option value="Quarta-feira">Quarta-feira</option>
      <option value="Quinta-feira">Quinta-feira</option>
      <option value="Sexta-feira">Sexta-feira</option>
    </select>

    <label for="hora">Hora:</label>
    <select id="hora" required>
      <option value="">Selecione a hora</option>
      <!-- As opções de hora serão carregadas dinamicamente -->
    </select>

    <button type="submit">Marcar horário</button>
  </form>

  <!-- Lista de horários marcados por alunos -->
  <div id="lista-marcar">
    <h3>Horários marcados</h3>
    <table id="tabela-marcacoes">
      <thead>
        <tr>
          <th>Dia</th>
          <th>Hora</th>
          <th>Aluno</th>
        </tr>
      </thead>
      <tbody>
        <!-- Linhas de marcações serão inseridas aqui -->
      </tbody>
    </table>
  </div>
</section>

<!-- Seção para professores visualizar marcações -->
<section id="visualizar">
  <h2>Lista de marcações (Para professores)</h2>
  <div id="lista-professor">
    <!-- Lista de marcações será carregada aqui -->
  </div>
</section>

<script>
  // JavaScript para lógica da página

  // Lista de horários disponíveis (de 8h às 17h)
  const horarios = [];
  for(let h=8; h<=17; h++) {
    horarios.push(`${h}:00`);
  }

  // Carregar opções de horários no select
  const selectHora = document.getElementById('hora');
  horarios.forEach(h => {
    const option = document.createElement('option');
    option.value = h;
    option.textContent = h;
    selectHora.appendChild(option);
  });

  const form = document.getElementById('form-marcacao');
  const tbody = document.querySelector('#tabela-marcacoes tbody');
  const listaProf = document.getElementById('lista-professor');

  // Função para obter marcações do localStorage
  function obterMarcacoes() {
    const marcacoes = localStorage.getItem('marcacoes');
    return marcacoes ? JSON.parse(marcacoes) : [];
  }

  // Função para salvar marcações no localStorage
  function salvarMarcacoes(marcacoes) {
    localStorage.setItem('marcacoes', JSON.stringify(marcacoes));
  }

  // Função para atualizar a tabela de marcações
  function atualizarTabela() {
    tbody.innerHTML = '';
    const marcacoes = obterMarcacoes();
    marcacoes.forEach(m => {
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${m.dia}</td>
        <td>${m.hora}</td>
        <td>${m.aluno}</td>
      `;
      tbody.appendChild(tr);
    });
  }

  // Função para atualizar a lista para professores
  function atualizarListaProf() {
    const marcacoes = obterMarcacoes();
    if(marcacoes.length === 0) {
      listaProf.innerHTML = '<p>Nenhuma marcação registrada.</p>';
    } else {
      let html = '<ul>';
      marcacoes.forEach(m => {
        html += `<li><strong>${m.dia} às ${m.hora}:</strong> ${m.aluno}</li>`;
      });
      html += '</ul>';
      listaProf.innerHTML = html;
    }
  }

  // Evento de submit do formulário
  form.addEventListener('submit', function(e){
    e.preventDefault();
    const dia = document.getElementById('dia').value;
    const hora = document.getElementById('hora').value;
    const aluno = prompt('Digite seu nome para confirmação:');
    if(!aluno) {
      alert('Nome é obrigatório para marcar.');
      return;
    }

    // Verificar se o horário já está marcado para aquele dia
    const marcacoes = obterMarcacoes();
    const jaMarcado = marcacoes.some(m => m.dia===dia && m.hora===hora);
    if(jaMarcado) {
      alert('Este horário já está marcado. Por favor, escolha outro.');
      return;
    }

    // Adicionar nova marcação
    marcacoes.push({dia, hora, aluno});
    salvarMarcacoes(marcacoes);
    atualizarTabela();
    atualizarListaProf();

    alert(`Horário para ${aluno} marcado com sucesso!\n${dia} às ${hora}`);
    form.reset();
  });

  // Inicializar a página com dados salvos
  atualizarTabela();
  atualizarListaProf();

</script>

</body>
</html>
