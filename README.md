// Lista que irá armazenar os nomes adicionados
let amigos = [];

// Adiciona um novo amigo à lista
function adicionarAmigo() {
    const input = document.getElementById('amigo');
    let nome = input.value.trim();

    // Validação: não permite vazio ou repetido
    if (!nome) {
        alert('Digite um nome válido!');
        return;
    }
    if (amigos.includes(nome)) {
        alert('Este nome já foi adicionado!');
        input.value = '';
        return;
    }

    amigos.push(nome);
    input.value = '';
    atualizarLista();
}

// Atualiza a lista de amigos exibida na tela
function atualizarLista() {
    const lista = document.getElementById('listaAmigos');
    lista.innerHTML = '';

    amigos.forEach((amigo, i) => {
        const li = document.createElement('li');
        li.textContent = amigo + ' ';
        
        // Botão para remover amigo
        const btnRemove = document.createElement('button');
        btnRemove.textContent = 'Remover';
        btnRemove.style.marginLeft = '10px';
        btnRemove.style.background = '#ff4444';
        btnRemove.style.color = '#fff';
        btnRemove.style.border = 'none';
        btnRemove.style.borderRadius = '10px';
        btnRemove.style.padding = '3px 10px';
        btnRemove.style.cursor = 'pointer';
        btnRemove.onclick = () => {
            amigos.splice(i, 1);
            atualizarLista();
            limparResultado();
        };

        li.appendChild(btnRemove);
        lista.appendChild(li);
    });
}

// Limpa o resultado do sorteio
function limparResultado() {
    document.getElementById('resultado').innerHTML = '';
}

// Realiza o sorteio do amigo secreto
function sortearAmigo() {
    limparResultado();

    if (amigos.length < 2) {
        alert('Adicione pelo menos dois amigos para sortear!');
        return;
    }

    // Faz uma cópia da lista para embaralhar
    let sorteados = [...amigos];

    // Algoritmo de embaralhamento (Fisher-Yates)
    for (let i = sorteados.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [sorteados[i], sorteados[j]] = [sorteados[j], sorteados[i]];
    }

    // Garante que ninguém tire a si mesmo
    for (let i = 0; i < amigos.length; i++) {
        if (amigos[i] === sorteados[i]) {
            // Se alguém tirou a si mesmo, faz novo sorteio
            sortearAmigo();
            return;
        }
    }

    // Exibe o resultado
    const ul = document.getElementById('resultado');
    ul.innerHTML = '';
    for (let i = 0; i < amigos.length; i++) {
        const li = document.createElement('li');
        li.textContent = `${amigos[i]} → ${sorteados[i]}`;
        ul.appendChild(li);
    }
}

// Permite adicionar pressionando Enter no input
document.getElementById('amigo').addEventListener('keyup', function (event) {
    if (event.key === 'Enter') {
        adicionarAmigo();
    }
});
