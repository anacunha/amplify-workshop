# Full Stack Application with AWS Amplify - Workshop

Esse workshop é baseado no [AWS Amplify Fullstack Workshop](https://github.com/aspittel/amplify-workshop) da [Ali Spittel](https://twitter.com/aspittel).

## O que vamos construir?

Vamos usar os recursos do Amplify para construir (com pouco código) um app com uma timeline de posts de usuários:

![Screen Shot 2022-04-28 at 17 37 15](https://user-images.githubusercontent.com/1771610/165851647-e1f35a31-1bf0-4de2-a345-3c4d0f05ec1a.png)

## O que é o Amplify?

O [AWS Amplify](https://docs.amplify.aws/) é um conjunto de ferramentas e serviços que ajuda desenvolvevores front-end web e mobile a criarem aplicações full-stack na AWS.

Com o Amplify, desenvolvedores podem criar recursos de back-end (como armazenamento de arquivos, autenticação e armazenamento de dados) usando a linha de comando ou uma interface visual e, então, fazer o deploy desses recursos na nuvem.

Em seguida, desenvolvedores podem conectar seu front-end aos recursos de back-end criados ou a recursos já existentes na AWS usando as [bibliotecas do Amplify](https://docs.amplify.aws/lib/).

A ideia do Amplify é abstrair os serviços específicos que estão por trás das funcionalidades de back-end e focar no desenvolvimento da aplicação em si. E, Como Amplify não requer conhecimento muito profundo da AWS, é um ótimo lugar pra começar a desenvolver aplicações full-stack na nuvem.

### Amplify Studio

O [Amplify Studio](https://docs.amplify.aws/console/) é um ambiente de desenvolvimento visual para criação de aplicações full-stack web e mobile. Ele é uma camada de abstração em cima do [Amplify CLI](https://docs.amplify.aws/cli/) e nos permite criar recursos de back-end, componentes de UI e gerenciar dados e usuários da nossa aplicação através de uma interface gráfica.

## Passo a Passo

### Modelagem de Dados

O primeiro passo é modelarmos os dados que irão fazer parte da nossa aplicação, chamamos isso de [Modelagem de Dados](https://pt.wikipedia.org/wiki/Modelagem_de_dados).

Nessa etapa, nós iremos definir como serão os **Modelos** dos dados que nossa aplicação irá utilizar e os seus **Atributos**. Além disso, também iremos definir o **Relacionamento** entre esses dados.

Para fazermos nossa modelagem de dados, iremos acessar o [Sandbox do Amplify Studio](https://sandbox.amplifyapp.com/).

O Sandbox nos permite fazer a modelagem dos dados da nossa aplicação e testá-los localmente sem a necessidade de termos uma conta da AWS. É uma ótima forma de começar a experimentar e testar suas aplicações sem se preocupar com deploy para nuvem ou com cobranças pelo uso dos serviços.

1. Acessar o Sandbox do Amplify Studio em [https://sandbox.amplifyapp.com/](https://sandbox.amplifyapp.com/).

2. Na seção **Quick start**, selecione: *Build a **`Blank data model`** with **`React`*** e clique em **Get started**.

<img width="976" alt="Quick start Amplify Studio - Build a Blank data model with React" src="https://user-images.githubusercontent.com/1771610/165817420-2ca6ad48-8867-4854-98a3-c0ba974caecb.png">

3. Para adicionar um novo modelo, clique em **+ Add model**.

<img width="1014" alt="Add model on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165818509-be64bc7d-67a5-4652-9f5f-6292a9eca4c2.png">

4. Na nossa aplicação teremos um modelo `User` para nossos usuários. Ele vai ter os seguintes atributos:

  - **username**: tipo `String` e obrigatório.
  - **photo**: tipo `AWSUrl` e opcional.

<img width="482" alt="User model on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165818745-e2fa6c26-d0e1-4ad7-8807-0d086cc12c6a.png">

5. A nossa aplicação também terá um modelo `Post` para os posts dos nossos usuários. Os posts terão os seguintes atributos:

  - **description**: tipo `String` e opcional.
  - **contentSource**: tipo `AWSURL` e obrigatório.
  - **contentType**: enum `ContentType` e obrigatório.
  - **postedAt**: tipo `AWSDate` e obrigatório.

<img width="482" alt="Post model on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165819719-5a27062e-67df-4cad-ad45-4d6abac9f14d.png">

6. O enum `ContentType` vai ter duas opções: `IMAGE` e `VIDEO`.

<img width="481" alt="ContentType enum on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165820159-448189a5-e848-4505-a6f2-51d09cd9c8d5.png">

7. Agora que os modelos da nossa aplicação foram definidos, precisamos especificar o relacionamento entre eles. No nosso caso, iremos criar um relacionamento `1:1` entre `Post` e `User` e vamos dar o nome de `author` para esse relacionamento. 

<img width="838" alt="Relationship between Post and User on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165821370-bedcaf39-6c06-467c-bb7c-0c687ea6652c.png">

Por fim, o *schema* completo dos nossos dados ficou assim:

<img width="973" alt="Final data schema on Amplify Studio" src="https://user-images.githubusercontent.com/1771610/165821684-55b97979-0959-42c3-b1ff-e6ededb1911d.png">

### Testar Localmente

O próximo passo é testar nas nossas máquinas o back-end que acabamos de criar com Amplify Studio.

Para isso, vamos criar um app React e trabalhar de dentro do diretório dele:

```shell
npx create-react-app@latest cityjs-amplify-workshop
cd cityjs-amplify-workshop
```

Para usarmos os comandos do Amplify, precisamos instalar o [Amplify CLI](https://docs.amplify.aws/cli/), uma ferramenta de linha de comando para criar, integrar e gerenciar os serviços de nuvem da AWS usados pela nossa aplicação:

```shell
curl -sL https://aws-amplify.github.io/amplify-cli/install | bash && $SHELL
```

Com Amplify CLI instalado, vamos fazer o `pull` da nossa aplicação criada pelo Amplify Studio para o nosso ambiente local. O comando [`amplify pull`](https://docs.amplify.aws/cli/start/workflows/#amplify-pull) funciona de maneira semalhante a um `git pull`. Ele busca as mudanças que ocorreram na nossa aplicação hospedada na nuvem e traz essas mudanças para nosso ambiente local:

```shell
amplify pull --sandboxId <your-sandbox-id>
```

Por fim, vamos instalar as dependências necessárias para a nossa aplicação. O [**aws-amplify**](https://github.com/aws-amplify/amplify-js) é a biblioteca JavaScript que nos dá acesso às funcionalidades do Amplify. E o [**typescript**](https://github.com/Microsoft/TypeScript) é necessário pois nossos modelos são gerados usando essa linguagem.

```shell
npm install aws-amplify typescript
```

O mais legal é que até agora não temos absolutamente nada rodando na nuvem! Estamos apenas trabalhando localmente. Você nem mesmo precisa de uma conta na AWS até o momento em que vai fazer o deploy da sua aplicação. Essa é uma ótima maneira de experimentar e testar sua aplicação antes de fazer o deploy.

Agora, vamos abrir o nosso projeto em um editor de texto para conferirmos o que foi criado até aqui.

<img width="768" alt="Screen Shot 2022-04-28 at 16 28 27" src="https://user-images.githubusercontent.com/1771610/165830972-d1a2da5f-1507-48ad-af44-59d461cfa69d.png">

#### Configurar Amplify

Precisamos configurar o Amplify em nosso projeto. Abra o arquivo `src/index.js` e adicione as seguintes linhas:

```javascript
import { Amplify } from 'aws-amplify'
import awsconfig from './aws-exports'

Amplify.configure(awsconfig)
```

#### Criar Dados

Vamos criar um usuário localmente. Abra o arquivo `src/App.js` e adicione o seguinte:

```javascript
import { DataStore } from '@aws-amplify/datastore';
import { User } from './models';

function App() {
  const createUser = async () => {
    const newUser = await DataStore.save(
      new User({
        username: prompt('username'),
        photo: prompt('photo')
      }))
  };

  return (
    <button onClick={createUser}>Create User</button>
  );
}

export default App;
```
 
----------

```shell
npm install aws-amplify @aws-amplify/ui-react
```

```javascript
import '@aws-amplify/ui-react/styles.css';
import { Amplify } from 'aws-amplify'
import awsconfig from './aws-exports'
import { AmplifyProvider } from '@aws-amplify/ui-react';

Amplify.configure(awsconfig)

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <AmplifyProvider>
    <App />
  </AmplifyProvider>
);
```

Add font to `index.css` file:

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:slnt,wght@-10..0,100..900&display=swap');
```

On `App.js`:

```javascript
import { DataStore } from '@aws-amplify/datastore';
import { Post, User } from './models';
import { PostCard } from './ui-components'

import { useEffect, useState } from 'react'

function App() {
  const [posts, setPosts] = useState([])

  const getPosts = async() => {
    const data = await DataStore.query(Post);
    setPosts(data)
  }

  useEffect(() => {
    getPosts()
  }, [])

  return (
    <div>
      {posts.map(post => <PostCard post={post} user={post.User} key={post.id}/>)}
    </div>
  );
}

export default App;
```

### Resources

- https://github.com/aspittel/amplify-workshop
- https://docs.amplify.aws/console/
- https://ui.docs.amplify.aws/getting-started/installation
