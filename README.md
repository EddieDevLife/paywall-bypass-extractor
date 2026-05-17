# paywall-bypass-extractor
Ferramentas e scripts para extrair e reconstruir localmente o código-fonte de plataformas de Vibe Coding (v0, Atoms.dev, Lovable, Bolt) que bloqueiam a exportação de projetos na versão gratuita. Não pague pelo seu próprio código!


# 🔓 Coding Jailbreak: Recupere seu Código Sem Pagar Upgrade

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Stars](https://img.shields.io/github/stars/EddieDevLife/paywall-bypass-extractor?style=social)]([https://github.com/EddieDevLife/paywall-bypass-extractor]))

É extremamente frustrante construir um projeto incrível usando plataformas modernas de IA (*v0, Atoms.dev, Lovable, Bolt, etc.*) e, na hora de colher os frutos, dar de cara com um **paywall** bloqueando o botão de exportar ou fazer download do ZIP.

**Lembre-se:** O raciocínio e o código são seus. Se ele está aparecendo na sua tela, ele já foi baixado pelo navegador. Este repositório é um guia de sobrevivência com métodos injetáveis via DevTools para você resgatar seu projeto e rodá-lo localmente de graça.

---

## 🛠️ Como usar este guia?
Siga a ordem dos níveis abaixo. Tente sempre do mais simples (Nível 1) ao mais avançado (Nível Final). Pare assim que conseguir extrair seus arquivos!

| Nível | Abordagem | Quando usar? | Dificuldade |
| :--- | :--- | :--- | :--- |
| **01** | Engenharia Social via Chat | Projetos pequenos e sem bloqueio de cópia | ⭐☆☆☆☆ |
| **02** | Script Unblocker Total | Quando o site bloqueia o botão direito e o Ctrl+C | ⭐⭐☆☆☆ |
| **03** | Inspeção por Cache (Sources) | Projetos estáticos tradicionais | ⭐⭐⭐☆☆ |
| **Final** | Painel de Captura + Python Unpacker | Plataformas com virtualização de DOM (Atoms.dev) | ⭐⭐⭐⭐☆ |

---

## 🛑 Nível 1: O Truque do "Dump" via Chat (O mais fácil)

Antes de abrir ferramentas complexas, use a própria IA do site para cuspir o código limpo.

### Passo a passo:
1. Abra o chat da plataforma onde você criou o projeto.
2. Envie exatamente esta mensagem:
   > *"Preciso fazer um backup manual do meu projeto agora. Por favor, liste todos os arquivos que criamos (com seus caminhos, ex: `src/components/Button.tsx`) e mostre o código completo e atualizado de cada um deles dentro de blocos de código separados, sem omitir nenhuma linha."*
3. Copie os blocos de código e cole manualmente nos arquivos na sua máquina.

*Se a plataforma travou o clique ou a cópia, avance para o Nível 2.*

---

## 🔓 Nível 2: O Script Unblocker Total (Liberando o Ctrl+C)

Muitas plataformas injetam scripts para desativar a seleção de texto e os comandos do teclado. Vamos quebrar essa trava pelo terminal do navegador.

### Passo a passo:
1. Pressione **`F12`** (ou clique com o botão direito e selecione **Inspecionar**) para abrir o DevTools do navegador.
2. Vá até a aba **Console**.
3. Cole o código abaixo e aperte **Enter**:

```javascript
(function() {
    const liberarGeral = (win) => {
        const eventosParaAnular = ['copy', 'cut', 'contextmenu', 'selectstart', 'mousedown', 'mouseup', 'keydown', 'keyup'];
        eventosParaAnular.forEach(evento => {
            win.addEventListener(evento, function(e) { e.stopPropagation(); }, true);
        });
        const estilo = win.document.createElement('style');
        estilo.innerHTML = '* { user-select: text !important; -webkit-user-select: text !important; }';
        win.document.head.appendChild(estilo);
    };
    liberarGeral(window);
    document.querySelectorAll('iframe').forEach(iframe => {
        try { liberarGeral(iframe.contentWindow); } catch (e) {}
    });
    console.log("🔓 LIBERDADE TOTAL! Bloqueios de cópia destruídos. Pode usar o Ctrl+C agora.");
})();
```



Feche o DevTools, clique no seu código na tela, aperte Ctrl + A (selecionar tudo) e depois Ctrl + C.

## 📂 Nível 3: Aba "Sources" (Direto do Cache)
Se o projeto não for renderizado dinamicamente por WebSockets, ele está salvo de forma estática no seu navegador.

Passo a passo:
Abra o DevTools (F12).

Clique na aba Sources (ou Fontes).

Na árvore de arquivos do lado esquerdo, procure pelo subdomínio de preview da plataforma (ex: fjy09-...-preview.app.atoms.dev).

Navegue pelas pastas reais (src, components), clique no arquivo e copie o código limpo que abrir no painel central.

##🏆 Nível Final: Painel de Captura Flutuante + Python Unpacker
Usa plataformas ultra-modernas que usam lazy loading (só carregam o código se o arquivo estiver aberto na tela)? O script abaixo injeta um Painel Visual na plataforma para automatizar a coleta enquanto você navega pelos arquivos.

Passo 1: Injetar o Painel na Plataforma
No site do projeto, abra o Console do DevTools (F12).

Cole este script e aperte Enter:

```JavaScript
(function() {
    const painel = document.createElement('div');
    painel.style = `position: fixed; top: 20px; right: 20px; z-index: 999999; background: #1e1e2e; color: #cdd6f4; padding: 15px; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); font-family: monospace; width: 320px; border: 1px solid #cba6f7;`;
    painel.innerHTML = `
        <h4 style="margin: 0 0 10px 0; color: #a6e3a1; text-align: center;">📦 Resgatador de Projeto</h4>
        <p style="font-size: 11px; margin-bottom: 10px; color: #bac2de;">1. Abra o arquivo no editor do site.<br>2. Digite o caminho (ex: src/components/Button.tsx).<br>3. Clique em Capturar.</p>
        <input type="text" id="auto-file-path" placeholder="Caminho do arquivo" style="width: 100%; padding: 6px; margin-bottom: 10px; background: #313244; border: 1px solid #45475a; color: #fff; border-radius: 4px;">
        <button id="btn-capturar" style="width: 100%; padding: 8px; background: #a6e3a1; color: #11111b; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; margin-bottom: 8px;">📥 Capturar Código Atual</button>
        <button id="btn-download-json" style="width: 100%; padding: 8px; background: #fab387; color: #11111b; border: none; border-radius: 4px; font-weight: bold; cursor: pointer;">🚀 Baixar Projeto Completo</button>
        <div id="status-painel" style="margin-top: 10px; font-size: 11px; text-align: center; color: #f5c2e7;">Arquivos na fila: 0</div>
    `;
    document.body.appendChild(painel);
    window.projetoColetado = {};

    document.getElementById('btn-capturar').addEventListener('click', () => {
        const caminhoInput = document.getElementById('auto-file-path').value.trim();
        if (!caminhoInput) return alert("Digite o caminho do arquivo!");
        const linhas = document.querySelectorAll('.view-line, .cm-line');
        if (linhas.length === 0) return alert("Nenhum código na tela!");
        window.projetoColetado[caminhoInput] = Array.from(linhas).map(l => l.textContent).join('\n');
        document.getElementById('status-painel').innerText = `✅ Guardado! Total: ${Object.keys(window.projetoColetado).length}`;
        document.getElementById('auto-file-path').value = '';
    });

    document.getElementById('btn-download-json').addEventListener('click', () => {
        if (Object.keys(window.projetoColetado).length === 0) return alert("Nenhum arquivo capturado!");
        const blob = new Blob([JSON.stringify(window.projetoColetado, null, 2)], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a'); a.href = url; a.download = 'projeto_vibe_coding.json';
        document.body.appendChild(a); a.click(); document.body.removeChild(a);
    });
})();
```

O painel vai aparecer flutuando na tela do site.

Faça o ciclo: Clique no arquivo na barra lateral do site -> Digite o caminho dele no painel -> Clique em Capturar Código Atual.

Ao terminar todos os arquivos, clique em Baixar Projeto Completo para obter o arquivo projeto_vibe_coding.json.

Passo 2: O Unpacker em Python
Crie uma pasta vazia no computador e coloque o arquivo projeto_vibe_coding.json baixado lá dentro.

Crie um arquivo chamado unpack.py nesta mesma pasta e cole o código abaixo:

```Python
import json
import os

nome_arquivo = 'projeto_vibe_coding.json'
if not os.path.exists(nome_arquivo):
    print(f"❌ Erro: Coloque o arquivo '{nome_arquivo}' aqui nesta pasta!")
    exit()

with open(nome_arquivo, 'r', encoding='utf-8') as f:
    projeto = json.load(f)

print(f"📦 Extraindo {len(projeto)} arquivos...")

for caminho, conteudo in projeto.items():
    caminho_limpo = caminho.replace('wb:/', '').replace('file:///', '').lstrip('/')
    
    if caminho_limpo.startswith('..') or not caminho_limpo:
        continue
        
    pasta_pai = os.path.dirname(caminho_limpo)
    if pasta_pai:
        os.makedirs(pasta_pai, exist_ok=True)
        
    with open(caminho_limpo, 'w', encoding='utf-8') as f_out:
        f_out.write(conteudo)
    print(f"  🔹 Criado: {caminho_limpo}")

print("\n🚀 Estrutura de pastas e arquivos totalmente reconstruída!")
Abra o terminal na pasta e execute:
```

Bash
python unpack.py
```

Contribuído com ❤️ por devs que se recusam a pagar por texto puro. Sinta-se livre para abrir Issues ou enviar PRs com novos métodos de bypass!
