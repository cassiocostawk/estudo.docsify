<font face="Calibri">

# ⚡ Corrigir Commit com Autor errado

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../README.md)

---

1. **Inicie uma reescrita interativa dos commits:**
   - Abra o terminal ou prompt de comando na pasta do seu repositório Git.
   - Execute o comando:

    ```bash
    git rebase -i HEAD~n
    ```

    *Substitua "n" pelo número de commits anteriores ao último commit que deseja alterar.*

    > ❔ Por exemplo, se deseja alterar os últimos três commits, você pode usar:
    > `git rebase -i HEAD~3`

2. **O Git abrirá um editor de texto com uma lista de commits:**
   - No editor, os commits aparecerão em ordem cronológica inversa, do mais recente para o mais antigo.

   - Ao lado de cada commit, você verá a palavra "pick".
    *Isso indica que o commit será mantido sem alterações.*

3. **Altere os commits desejados para "edit":**
    - No editor (no CMD)
        - aperte "i" para iniciar o modo INSERT
        - substitua a palavra "pick" pelo comando "edit" para os commits que deseja alterar
    - Salve e feche o editor de texto.

    > - ESC para sair do modo INSERT
    > - escreva `:wq` para salvar e sair (write and quit)

4. **O Git irá pausar no primeiro commit que você deseja editar.**
    Neste ponto, você pode usar o comando abaixo para cada commit que deseja alterar o autor:
    `git commit --amend --author="Novo Autor <email@exemplo.com>" -m "Minha mensagem"`

    Por exemplo:

    ```bash
    git commit --amend --author="Nome Autor <email@email.com>" -m "Minha mensagem"
    ```

    *Substitua "Nome Autor" e "email@email.com" pelo Autor correto, alem da mensagem.*

5. **Após fazer as alterações desejadas em cada commit, continue o rebase:**
    - Use o comando:

    ```bash
    git rebase --continue
    ```

7. **Repita o passo 4 para cada commit que precisa ter o autor alterado.**

8. **Quando terminar de editar todos os commits, estes terão o novo autor e será possível fazer o push.**

---

[`^ topo`](#-corrigir-commit-com-autor-errado)
</font>
