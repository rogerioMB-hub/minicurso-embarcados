# Como ativar o GitHub Pages para este repositório

Siga os passos abaixo após fazer o push do repositório para o GitHub.

## 1. Ajustar o _config.yml

Abra o arquivo `_config.yml` e substitua `seu-usuario` pelo seu nome de usuário real do GitHub:

```yaml
baseurl: "/minicurso-embarcados"
url: "https://seu-usuario.github.io"   # ← substitua aqui
```

## 2. Ativar o GitHub Pages

1. No repositório GitHub, clique em **Settings** (engrenagem)
2. No menu lateral, clique em **Pages**
3. Em *Source*, selecione **Deploy from a branch**
4. Em *Branch*, selecione **main** e a pasta **/ (root)**
5. Clique em **Save**

Após alguns minutos, o site estará disponível em:

```
https://seu-usuario.github.io/minicurso-embarcados
```

## 3. Incorporar no Google Sites

1. No Google Sites, abra a página onde deseja exibir o curso
2. No painel direito, clique em **Incorporar**
3. Cole a URL do GitHub Pages
4. Ajuste o tamanho do iframe conforme necessário

Alternativamente, use um botão de link direto:
- *Inserir → Botão → URL do GitHub Pages*

## 4. Atualizar o conteúdo

Qualquer alteração feita nos arquivos `.md` e enviada via `git push` será publicada automaticamente no site em 1 a 2 minutos — sem nenhuma ação adicional no Google Sites.
