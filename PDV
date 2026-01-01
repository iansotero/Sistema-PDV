<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<title>Sistema Corporativo — PDV</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:#0e1828;
  color:#e8f1ff;
}
header{
  background:#142742;
  padding:14px 20px;
  font-size:20px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
nav{
  display:flex;
  gap:10px;
  padding:10px;
  background:#12243a;
}
nav button{
  background:#1c3356;
  color:#e8f1ff;
  border:1px solid #264a7d;
  padding:8px 14px;
  cursor:pointer;
  border-radius:6px;
}
nav button.active{background:#2d5bb1;}
section{padding:15px;}
.hidden{display:none;}
table{
  width:100%;
  border-collapse:collapse;
  margin-top:10px;
}
td,th{
  border:1px solid #264a7d;
  padding:6px;
}
input,select{
  padding:6px;
  border-radius:6px;
  border:1px solid #315a93;
  background:#0e1828;
  color:#fff;
}
button.action{
  background:#2d5bb1;
  border:none;
  padding:7px 12px;
  border-radius:6px;
  cursor:pointer;
  color:white;
}
.editBtn{cursor:pointer;}
.calendar{
  max-width:450px;
  background:#152a45;
  border-radius:12px;
  padding:12px;
  margin-top:15px;
}
.calendar header{
  display:flex;
  gap:10px;
  align-items:center;
  justify-content:space-between;
  background:none;
  padding:0;
}
.calendar select{flex:1;}
#calendarGrid{
  margin-top:10px;
  display:grid;
  grid-template-columns:repeat(7,1fr);
  gap:4px;
}
.dayHeader{
  text-align:center;
  font-weight:bold;
  color:#9db2d6;
}
.dayCell{
  text-align:center;
  padding:8px 0;
  border-radius:8px;
  background:#1b2f4f;
  cursor:pointer;
}
.dayCell:hover{background:#2d5bb1;}
.emptyDay{background:transparent;}
.card{
  margin-top:12px;
  padding:10px;
  border-radius:10px;
  background:#1b2f4f;
}

/* formulário usuários */
#userForm{
  background:#1b2f4f;
  padding:12px;
  border-radius:10px;
  margin-top:10px;
  display:none;
}
</style>
</head>

<body>

<header>
  📊 Sistema Corporativo — PDV
  <span id="usuarioLogado"></span>
</header>

<nav>
  <button onclick="show('produtos')" class="active">Produtos / Estoque</button>
  <button onclick="show('pdv')">Frente de Caixa</button>
  <button onclick="show('clientes')">Clientes</button>
  <button onclick="show('financeiro')">Financeiro / Relatórios</button>
  <button onclick="show('usuarios')">Usuários</button>
</nav>

<!-- ================= PRODUTOS ================= -->
<section id="produtos">
  <h2>Produtos & Estoque</h2>

  <input id="filtroProduto" placeholder="Filtrar por código ou produto" oninput="renderProdutos()">

  <div style="margin-top:10px">
    Código <input id="pCodigo">
    Nome <input id="pNome">
    Preço <input id="pPreco" type="number">
    Estoque <input id="pEstoque" type="number">
    <button class="action" onclick="addProduto()">Adicionar Produto</button>
  </div>

  <table id="prodTable">
    <thead>
      <tr>
        <th>Código</th><th>Produto</th><th>Preço</th><th>Estoque</th><th>Editar</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <p>Total de produtos: <b id="relProdutos">0</b></p>
</section>

<!-- ================= PDV ================= -->
<section id="pdv" class="hidden">
  <h2>Frente de Caixa</h2>

  Código <input id="pdvCodigo" oninput="buscarPorCodigo()">
  Produto <input id="pdvProdutoNome" disabled>
  Valor <input id="pdvValor" disabled>

  <br><br>

  Quantidade:
  <button onclick="changeQtd(-1)">-</button>
  <input id="pdvQtd" type="number" value="1" style="width:60px">
  <button onclick="changeQtd(1)">+</button>

  <button class="action" onclick="addItem()">Adicionar</button>

  <table id="pdvTable">
    <thead>
      <tr>
        <th>Código</th><th>Produto</th><th>Qtd</th><th>Preço</th><th>Total</th><th>Remover</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <h3>Total: R$ <span id="pdvTotal">0,00</span></h3>
  <button class="action" onclick="finalizarVenda()">Finalizar Venda</button>
</section>

<!-- ================= CLIENTES ================= -->
<section id="clientes" class="hidden">
  <h2>Clientes</h2>

  Nome <input id="cNome" oninput="this.value=this.value.toUpperCase()">
  Telefone <input id="cTelefone" oninput="mascaraTelefone(this)">
  CPF <input id="cCpf" oninput="mascaraCPF(this)">
  <button class="action" onclick="addCliente()">Cadastrar</button>

  <br><br>
  Filtrar: <input id="filtroCliente" placeholder="Nome ou CPF" oninput="renderClientes()">

  <ul id="clienteList"></ul>

  <p>Total clientes: <b id="relClientes">0</b></p>
</section>

<!-- ================= FINANCEIRO / RELATÓRIOS ================= -->
<section id="financeiro" class="hidden">
  <h2>Financeiro & Relatórios</h2>

  <div class="card">
    Total vendido: <b id="finTotal">R$ 0,00</b><br>
    Registro de vendas: <b id="relVendas">0</b>
  </div>

  <div class="calendar">
    <header>
      <select id="calendarMonth"></select>
      <select id="calendarYear"></select>
    </header>

    <div id="calendarGrid"></div>

    <div class="card">
      <b>Data selecionada:</b> <span id="selectedDate">—</span><br>
      <b>Faturamento:</b> <span id="selectedRevenue">R$ 0,00</span>
    </div>
  </div>
</section>

<!-- ================= USUÁRIOS ================= -->
<section id="usuarios" class="hidden">
  <h2>Usuários</h2>

  <button class="action" onclick="toggleUserForm()">Adicionar Usuário</button>

  <div id="userForm">
    Nome: <input id="uNome"><br><br>
    Senha: <input id="uSenha" type="password"><br><br>
    Cargo: <input id="uCargo"><br><br>
    <button class="action" onclick="addUsuario()">Salvar</button>
  </div>

  <ul id="usuarioList"></ul>
</section>

<script>
let produtos = [
  {id:1,codigo:"101",nome:"Café 500g",preco:15,estoque:20},
  {id:2,codigo:"102",nome:"Açúcar 1kg",preco:8,estoque:50}
];

let clientes = [];
let usuarios = [];
let usuarioAtual = "ADMIN";

let venda = [];
let vendasTotais = 0;
let historicoVendas = [];

function salvarDados(){
  localStorage.setItem("produtos", JSON.stringify(produtos));
  localStorage.setItem("clientes", JSON.stringify(clientes));
  localStorage.setItem("usuarios", JSON.stringify(usuarios));
  localStorage.setItem("vendasTotais", JSON.stringify(vendasTotais));
  localStorage.setItem("historicoVendas", JSON.stringify(historicoVendas));
}
function carregarDados(){
  let p = JSON.parse(localStorage.getItem("produtos"));
  let c = JSON.parse(localStorage.getItem("clientes"));
  let u = JSON.parse(localStorage.getItem("usuarios"));
  let v = JSON.parse(localStorage.getItem("vendasTotais"));
  let h = JSON.parse(localStorage.getItem("historicoVendas"));
  if(p) produtos = p;
  if(c) clientes = c;
  if(u) usuarios = u;
  if(v) vendasTotais = v;
  if(h) historicoVendas = h;
}

function show(id){
  document.querySelectorAll("nav button").forEach(b=>b.classList.remove("active"));
  event.target.classList.add("active");
  document.querySelectorAll("section").forEach(s=>s.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
  render();
}

function buscarPorCodigo(){
  let codigo = pdvCodigo.value.trim();
  let p = produtos.find(x=>x.codigo===codigo);
  if(p){
    pdvProdutoNome.value=p.nome;
    pdvValor.value="R$ "+p.preco.toFixed(2);
  }else{
    pdvProdutoNome.value="";
    pdvValor.value="";
  }
}

function changeQtd(d){
  let v=Number(pdvQtd.value)+d;
  if(v<1)v=1;
  pdvQtd.value=v;
}

/* PRODUTOS */
function renderProdutos(){
  let tbody=document.querySelector("#prodTable tbody");
  tbody.innerHTML="";
  let filtro=(document.getElementById("filtroProduto")?.value||"").toLowerCase();
  produtos
    .filter(p=>p.nome.toLowerCase().includes(filtro)||p.codigo.toLowerCase().includes(filtro))
    .forEach((p,i)=>{
      let tr=document.createElement("tr");
      tr.innerHTML=
      `<td>${p.codigo}</td>
       <td>${p.nome}</td>
       <td>R$ ${p.preco}</td>
       <td>${p.estoque}</td>
       <td><span class="editBtn" onclick="editarProduto(${i})">✏️</span></td>`;
      tbody.appendChild(tr);
    });
  relProdutos.textContent=produtos.length;
}
function addProduto(){
  if(produtos.some(x=>x.codigo===pCodigo.value.trim()))
    return alert("Este código já existe!");
  if(produtos.some(x=>x.nome.toLowerCase()===pNome.value.trim().toLowerCase()))
    return alert("Este produto já está cadastrado!");
  produtos.push({
    id:Date.now(),
    codigo:pCodigo.value,
    nome:pNome.value,
    preco:Number(pPreco.value),
    estoque:Number(pEstoque.value)
  });
  salvarDados();
  render();
}
function editarProduto(i){
  let p=produtos[i];
  let nome=prompt("Nome do produto:",p.nome);
  if(nome===null)return;
  let preco=Number(prompt("Preço:",p.preco));
  let estoque=Number(prompt("Estoque:",p.estoque));
  p.nome=nome; p.preco=preco; p.estoque=estoque;
  salvarDados(); render();
}

/* VENDA */
function addItem(){
  let codigo=pdvCodigo.value.trim();
  let qtd=Number(pdvQtd.value);
  let p=produtos.find(x=>x.codigo===codigo);
  if(!p)return alert("Produto não encontrado!");
  if(p.estoque<qtd)return alert("Estoque insuficiente!");
  venda.push({produto:p,qtd});
  p.estoque-=qtd;
  pdvCodigo.value=""; pdvProdutoNome.value=""; pdvValor.value=""; pdvQtd.value=1;
  salvarDados(); render();
}
function renderVenda(){
  let tbody=document.querySelector("#pdvTable tbody");
  tbody.innerHTML="";
  let total=0;
  venda.forEach((i,idx)=>{
    let t=i.qtd*i.produto.preco;
    total+=t;
    let tr=document.createElement("tr");
    tr.innerHTML=
    `<td>${i.produto.codigo}</td>
     <td>${i.produto.nome}</td>
     <td>${i.qtd}</td>
     <td>R$ ${i.produto.preco}</td>
     <td>R$ ${t}</td>
     <td><button onclick="removeItem(${idx})">X</button></td>`;
    tbody.appendChild(tr);
  });
  pdvTotal.textContent=total.toFixed(2);
}
function removeItem(i){
  let item=venda[i];
  item.produto.estoque+=item.qtd;
  venda.splice(i,1);
  salvarDados(); render();
}
function finalizarVenda(){
  if(!venda.length)return alert("Nenhum item!");
  let total=venda.reduce((s,i)=>s+i.qtd*i.produto.preco,0);
  vendasTotais+=total;
  let data=new Date().toISOString().slice(0,10);
  historicoVendas.push({data,total});
  venda=[];
  alert("Venda concluída!");
  salvarDados(); render();
}

/* CLIENTES */
function mascaraTelefone(el){
  let v=el.value.replace(/\D/g,"").slice(0,11);
  v=v.replace(/^(\d{2})(\d)/,"($1) $2");
  v=v.replace(/(\d{5})(\d)/,"$1-$2");
  el.value=v;
}
function mascaraCPF(el){
  let v=el.value.replace(/\D/g,"").slice(0,11);
  v=v.replace(/(\d{3})(\d)/,"$1.$2");
  v=v.replace(/(\d{3})(\d)/,"$1.$2");
  v=v.replace(/(\d{3})(\d{1,2})$/,"$1-$2");
  el.value=v;
}
function addCliente(){
  clientes.push({
    nome:cNome.value.toUpperCase(),
    telefone:cTelefone.value,
    cpf:cCpf.value
  });
  salvarDados(); render();
}
function editarCliente(i){
  let c=clientes[i];
  let nome=prompt("Nome:",c.nome)?.toUpperCase();
  if(nome===null)return;
  let telefone=prompt("Telefone:",c.telefone);
  let cpf=prompt("CPF:",c.cpf);
  clientes[i]={nome,telefone,cpf};
  salvarDados(); render();
}
function excluirCliente(i){
  if(confirm("Deseja realmente excluir este cliente?")){
    clientes.splice(i,1);
    salvarDados(); render();
  }
}
function renderClientes(){
  let ul=clienteList;
  ul.innerHTML="";
  let filtro=(document.getElementById("filtroCliente")?.value||"").toUpperCase();
  clientes
    .filter(c =>
      c.nome.includes(filtro) ||
      c.cpf.replace(/\D/g,"").includes(filtro.replace(/\D/g,""))
    )
    .forEach((c,i)=>{
      let li=document.createElement("li");
      li.innerHTML=
        `${c.nome} | ${c.telefone} | ${c.cpf}
         <span style="margin-left:10px;cursor:pointer" onclick="editarCliente(${i})">✏️</span>
         <span style="margin-left:6px;cursor:pointer" onclick="excluirCliente(${i})">🗑️</span>`;
      ul.appendChild(li);
    });
  relClientes.textContent=clientes.length;
}

/* FINANCEIRO — agora calcula por mês selecionado */
function renderFinanceiro(){
  let m = Number(calendarMonth?.value ?? new Date().getMonth()) + 1;
  let y = Number(calendarYear?.value ?? new Date().getFullYear());
  let prefix = `${y}-${String(m).padStart(2,"0")}`;

  let vendasMes = historicoVendas.filter(v=>v.data.startsWith(prefix));
  let totalMes = vendasMes.reduce((s,v)=>s+v.total,0);

  finTotal.textContent="R$ "+totalMes.toFixed(2);
  relVendas.textContent=vendasMes.length;
}

/* USUÁRIOS */
function toggleUserForm(){
  let f=document.getElementById("userForm");
  f.style.display = f.style.display==="none"||!f.style.display ? "block" : "none";
}
function addUsuario(){
  usuarios.push({
    nome:uNome.value,
    senha:uSenha.value,
    cargo:uCargo.value
  });
  salvarDados();
  renderUsuarios();
  toggleUserForm();
}
function renderUsuarios(){
  usuarioList.innerHTML="";
  usuarios.forEach(u=>{
    let li=document.createElement("li");
    li.textContent = `${u.nome} — ${u.cargo}`;
    usuarioList.appendChild(li);
  });
}

/* CALENDÁRIO */
function initCalendario(){
  if(!calendarMonth)return;
  const monthSelect=calendarMonth;
  const yearSelect=calendarYear;
  const now=new Date();
  const anoAtual=now.getFullYear();

  for(let m=0;m<12;m++){
    let opt=document.createElement("option");
    opt.value=m;
    opt.textContent=new Date(2000,m,1).toLocaleString("pt-BR",{month:"long"});
    monthSelect.appendChild(opt);
  }
  for(let a=anoAtual-5;a<=anoAtual+5;a++){
    let opt=document.createElement("option");
    opt.value=a;
    opt.textContent=a;
    yearSelect.appendChild(opt);
  }

  monthSelect.value=now.getMonth();
  yearSelect.value=anoAtual;
  monthSelect.onchange=()=>{renderCalendario(); renderFinanceiro();}
  yearSelect.onchange=()=>{renderCalendario(); renderFinanceiro();}
  renderCalendario();
}

function renderCalendario(){
  const grid=calendarGrid;
  if(!grid)return;
  grid.innerHTML="";

  const month=Number(calendarMonth.value);
  const year=Number(calendarYear.value);
  const primeiro=new Date(year,month,1);
  const ultimo=new Date(year,month+1,0);

  const dias=["S","T","Q","Q","S","S","D"];
  dias.forEach(d=>{
    let div=document.createElement("div");
    div.className="dayHeader";
    div.textContent=d;
    grid.appendChild(div);
  });

  let start=primeiro.getDay();
  for(let i=0;i<start;i++){
    let e=document.createElement("div");
    e.className="emptyDay";
    grid.appendChild(e);
  }

  for(let d=1;d<=ultimo.getDate();d++){
    let dataStr=`${year}-${String(month+1).padStart(2,"0")}-${String(d).padStart(2,"0")}`;
    let soma=historicoVendas.filter(v=>v.data===dataStr).reduce((s,v)=>s+v.total,0);

    let div=document.createElement("div");
    div.className="dayCell";
    div.textContent=d;

    div.onclick=()=>{
      let [y,m,dd]=dataStr.split("-");
      selectedDate.textContent=`${dd}/${m}/${y}`;
      selectedRevenue.textContent="R$ "+soma.toFixed(2);
    };

    grid.appendChild(div);
  }
}

function render(){
  usuarioLogado.textContent = "Usuário: " + usuarioAtual;
  renderProdutos();
  renderVenda();
  renderClientes();
  renderUsuarios();
  renderFinanceiro();
  renderCalendario();
}

carregarDados();
render();
initCalendario();
</script>

</body>
</html>
