# Votação Culinária — instalação

Este projeto contém uma página estática (`index.html`) e regras de segurança do Firestore (`firestore.rules`).

Funcionalidades:
- Login com conta Google.
- Uma avaliação por conta Google.
- 3 equipes e 7 critérios por equipe; cada nota deve ser de 1 a 10.
- Cada pessoa envia as 21 notas de uma vez.
- Soma de todas as notas de todos os votos, com atualização em tempo real.
- Regras do banco bloqueiam segundo envio, alteração e exclusão de votos.

## 1. Criar o projeto Firebase

1. Acesse https://console.firebase.google.com/ e crie um projeto.
2. Na visão geral do projeto, adicione um app Web (`</>`).
3. Copie os valores do objeto de configuração mostrado pelo Firebase.
4. Abra `index.html` e substitua os valores em `firebaseConfig`:
   - `apiKey`
   - `authDomain`
   - `projectId`
   - `appId`

A configuração web do Firebase não é uma senha administrativa. A segurança vem das regras do Firestore e da autenticação; nunca coloque chaves privadas de servidor neste arquivo.

## 2. Ativar login com Google

1. No Console do Firebase, abra **Authentication**.
2. Entre em **Sign-in method** (Método de login).
3. Ative **Google** e salve.

## 3. Criar o banco de dados e publicar as regras

1. No Console do Firebase, abra **Firestore Database** e crie um banco de dados.
2. Depois da criação, abra a aba **Rules** (Regras).
3. Copie todo o conteúdo do arquivo `firestore.rules`, cole no editor e clique em **Publish**.

Não use regras abertas de teste como `allow read, write: if true`. As regras incluídas permitem leitura pública dos votos para somar os resultados, mas só permitem que uma pessoa autenticada crie seu próprio voto e proíbem atualização e exclusão.

## 4. Publicar gratuitamente com GitHub Pages

1. Crie ou entre na sua conta em https://github.com/.
2. Crie um repositório público, por exemplo `votacao-culinaria`.
3. Envie o arquivo `index.html` para a raiz do repositório. O arquivo deve se chamar exatamente `index.html`.
4. Abra **Settings > Pages**.
5. Em **Build and deployment**, escolha **Deploy from a branch**; selecione a branch `main` e a pasta `/(root)`, depois salve.
6. Aguarde o GitHub Pages publicar o site. O endereço costuma ser `https://SEU-USUARIO.github.io/votacao-culinaria/`.

A publicação pode levar alguns minutos. Consulte a seção Pages do repositório para ver o endereço exato.

## 5. Autorizar o endereço no Firebase

1. Volte ao Console do Firebase > **Authentication** > **Settings** (Configurações).
2. Em **Authorized domains** (Domínios autorizados), adicione o domínio do site, sem `https://` e sem o caminho. Exemplo: `SEU-USUARIO.github.io`.
3. Salve.

## 6. Testar antes de compartilhar

1. Abra o link publicado.
2. Entre com uma conta Google.
3. Selecione todas as 21 notas e envie o voto.
4. Abra o link em outra janela/dispositivo e entre com outra conta Google. Confirme que os totais mudam em tempo real.
5. Tente enviar novamente com a primeira conta: ela deve aparecer como já votou.

## Como é calculada a pontuação

Cada voto adiciona as sete notas de cada equipe à soma daquela equipe. Um participante pode acrescentar no máximo 70 pontos por equipe. O placar mostra a soma acumulada de todos os votos e o número de avaliações recebidas.

## Limitações importantes

- “Um voto por pessoa” significa, tecnicamente, um voto por conta Google. Alguém pode tentar votar de novo usando outra conta Google; nenhum login comum consegue garantir que duas contas pertencem à mesma pessoa.
- Os resultados são públicos, e as notas individuais ficam armazenadas no Firestore para que o navegador possa calcular os totais em tempo real. O app não grava o nome nem o e-mail do votante no documento do voto.
- Não há botão administrativo para apagar votos. Para reiniciar a votação, o organizador precisa remover os documentos da coleção `votes` no Firestore Console. Faça isso somente depois de encerrar a votação.
- O Firebase tem limites de uso gratuitos e pode alterar condições. Confira os limites do plano no Console do Firebase. Para um evento pequeno, esta arquitetura normalmente é simples e econômica, mas não é uma garantia de custo zero em qualquer volume.
- O arquivo não funciona como votação compartilhada enquanto `firebaseConfig`, Authentication, Firestore e as regras não estiverem configurados.

## Documentação oficial

- Firebase Web setup: https://firebase.google.com/docs/web/setup
- Login Google: https://firebase.google.com/docs/auth/web/google-signin
- Regras de segurança do Firestore: https://firebase.google.com/docs/firestore/security/get-started
- GitHub Pages: https://docs.github.com/pages/quickstart
