# Controle financeiro

Formulário de celular (HTML + JS) que grava no Supabase. Fica publicado no GitHub Pages:
https://vteodoro-py.github.io/controle-financeiro-vteodoro/

## Fase 1: base segura

- **Login por código:** a pessoa digita o e-mail, recebe um código de 6 dígitos e cola no app. Não tem senha, e o login fica guardado no próprio atalho do iPhone. Só entra quem foi convidado no painel do Supabase.
- **RLS:** cada pessoa só lê, grava, edita e apaga os próprios lançamentos. A chave anon que está no código, sozinha, não acessa nada.
- **Data:** a data padrão usa o fuso do celular, então depois das 21h ainda aparece o dia de hoje.
- **Parcelas:** a compra continua sendo uma linha só, e o `valor` é sempre o total da compra. A view `v_parcelas` expande a compra em uma linha por parcela, cada uma no seu mês. O histórico do app (e futuramente o Power BI) lê dessa view.
- **Categorias:** ficam na tabela `categorias`, uma lista para cada pessoa. Se você digitar uma categoria nova no formulário, ela entra na lista.
- **Editar e apagar:** na aba Histórico, toque num lançamento.

## Como colocar no ar

Faça tudo de uma vez, num momento em que ninguém esteja lançando. Entre o passo 4 e o passo 5, o formulário antigo para de gravar.

1. **Endereço do app:** em *Authentication > URL Configuration*, coloque `https://vteodoro-py.github.io/controle-financeiro-vteodoro/` em **Site URL**.
2. **E-mail com código:** em *Authentication > Emails > Magic Link*, troque o corpo do e-mail por:
   ```html
   <h2>Seu código de acesso</h2>
   <p>Digite este código no app: <strong style="font-size:24px">{{ .Token }}</strong></p>
   <p>Ele vale por 1 hora.</p>
   ```
3. **Convites:** em *Authentication > Users > Add user > Send invitation*, convide o seu e-mail (e o da sua namorada, se ela já for usar). Depois, em *Authentication > Sign In / Providers*, desligue **Allow new users to sign up**.
4. **SQL:** abra [`supabase/fase1.sql`](supabase/fase1.sql), troque `SEU_EMAIL@exemplo.com` pelo seu e-mail e rode no *SQL Editor*. O script passa todo o histórico atual para o seu usuário e cria as suas categorias.
5. **App:** aprove (merge) o PR. Em 1 ou 2 minutos o GitHub Pages atualiza. Abra o atalho e entre com o código.

## Próximas fases

2. Uso a dois: botão Meu/Nosso, quem pagou, visão do casal e acerto de contas
3. Metas no banco: orçamento, limite por categoria e economia
4. Gamificação e tema próprio de cada um
5. Power BI com um usuário só de leitura
