# Full Stack Application with AWS Amplify - Workshop

Esse workshop é baseado no [AWS Amplify Fullstack Workshop](https://github.com/aspittel/amplify-workshop) da [Ali Spittel](https://twitter.com/aspittel).

## O que vamos construir?

Vamos usar os recursos do [Amplify](https://docs.amplify.aws/) para construir (com pouco código) um app com uma timeline de posts de usuários:

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

### Deploy na AWS

Agora podemos testar os modelos localmente ou fazer o deploy diretamente. Vamos direto para o deploy nesse momento.

<img width="1120" alt="Screen Shot 2022-04-28 at 18 53 59" src="https://user-images.githubusercontent.com/1771610/165854047-93665612-3672-433a-884b-984f85d99811.png">

Vamos dar um nome para nosso app e escolher uma região para fazer o deploy:

<img width="881" alt="Screen Shot 2022-04-28 at 18 54 53" src="https://user-images.githubusercontent.com/1771610/165854337-996650cb-5a75-4d2d-b1c8-d387959b9a06.png">

Agora, o Amplify vai começar a fazer o deploy do back-end que definimos:

<img width="553" alt="Screen Shot 2022-04-28 at 18 56 04" src="https://user-images.githubusercontent.com/1771610/165854482-2d99a5e2-83f7-4b99-b38f-088498c99336.png">

Quando tudo estiver pronto, vamos acessar o Amplify Studio em **Launch Studio**:

<img width="1069" alt="Screen Shot 2022-04-28 at 18 56 57" src="https://user-images.githubusercontent.com/1771610/165854545-53036990-8a01-486c-8f7e-e68c65709c04.png">

### Criar Dados

O Amplify nos permite criar dados através do Amplify Studio. Vamos criar alguns usuários e posts:

- Usuario 1
  - anacunha
  - https://github.com/anacunha.png

- Usuario 2
  - tempestade
  - https://unsplash.com/photos/75715CVEJhI

- Usuario 3
  - boris
  - https://unsplash.com/photos/9LkqymZFLrE

### UI Library

Na seção de **UI Library** do Amplify Studio, vamos duplicar o [template do Figma do Amplify](https://www.figma.com/community/file/1047600760128127424) e modificar um componente para nossa aplicação.

Vamos usar o componente **Ampligram** e adaptá-lo para atender as nossas necessidades. Vamos remover os ícones e renomeá-lo para **PostCard**:

<img width="682" alt="Screen Shot 2022-04-28 at 19 01 25" src="https://user-images.githubusercontent.com/1771610/165855134-dec2b9bb-25b6-4345-9ec2-e14eb73bdfd7.png">

Feito isso, iremos importar o nosso template modificado do Figma de volta no Amplify Studio e aceitaremos as mudanças da UI Library.

<img width="617" alt="Screen Shot 2022-04-28 at 19 02 59" src="https://user-images.githubusercontent.com/1771610/165855210-399b5867-5534-429c-90be-6585fecaf0d6.png">

Agora, vamos associar os dados dos nossos modelos ao componente de UI que acabamos de importar:

<img width="908" alt="Screen Shot 2022-04-28 at 19 13 37" src="https://user-images.githubusercontent.com/1771610/165856429-33db5edd-3a6e-44ae-8a46-7b1ad184198f.png">

### Criando React App

Vamos criar nosso projeto React:

```shell
npx create-react-app@latest cityjs-amplify-workshop
cd cityjs-amplify-workshop
```

Vamos instalar nossas dependências. O [**aws-amplify**](https://github.com/aws-amplify/amplify-js) é a biblioteca JavaScript que nos dá acesso às funcionalidades do Amplify.

```shell
npm install aws-amplify @aws-amplify/ui-react
```

Agora, vamos fazer o `pull` da nossa aplicação criada pelo Amplify Studio para o nosso ambiente local. O comando [`amplify pull`](https://docs.amplify.aws/cli/start/workflows/#amplify-pull) funciona de maneira semalhante a um `git pull`. Ele busca as mudanças que ocorreram na nossa aplicação hospedada na nuvem e traz essas mudanças para nosso ambiente local:

```shell
amplify pull --appId <appId> --envName <envName>
```

Agora, vamos abrir o nosso projeto em um editor de texto para conferirmos o que foi criado até aqui.

<img width="768" alt="Screen Shot 2022-04-28 at 16 28 27" src="https://user-images.githubusercontent.com/1771610/165830972-d1a2da5f-1507-48ad-af44-59d461cfa69d.png">

#### Configurar Amplify

Precisamos configurar o Amplify em nosso projeto. Abra o arquivo `src/index.js` e adicione as seguintes linhas:

```javascript
import { Amplify } from 'aws-amplify';
import "@aws-amplify/ui-react/styles.css";
import {AmplifyProvider} from "@aws-amplify/ui-react";
import awsconfig from './aws-exports';

Amplify.configure(awsconfig);
```

E o nosso `<App />` ficará dentro do `<AmplifyProvider>`:

```javascript
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <AmplifyProvider>
    <App />
  </AmplifyProvider>
);
```

Para finalizar a configuração da UI, no `index.css` adicionamos a fonte usada pelos componentes UI do Amplify:

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:slnt,wght@-10..0,100..900&display=swap');
```

#### Listar Dados com os componentes da UI

Vamos criar um usuário localmente. Abra o arquivo `src/App.js` e adicione o seguinte:

```javascript
import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';
import { PostCard } from './ui-components';

import {useEffect, useState} from 'react'

function App() {
  const [posts, setPosts] = useState([])

  const getPosts = async () => {
    const data = await DataStore.query(Post)
    setPosts(data)
  }

  useEffect(() => {
    getPosts()
  })

  return (
    <div className="App">
      {posts.map(
        post => <PostCard post={post} user={post.author} key={post.id}/>
      )}
    </div>
  );
}

export default App;
```

### Resources

- https://github.com/aspittel/amplify-workshop
- https://docs.amplify.aws/console/
- https://ui.docs.amplify.aws/getting-started/installation


## Recipes Demos

### Model

![Screen Shot 2022-04-29 at 21 20 11](https://user-images.githubusercontent.com/1771610/166082965-8d80140f-3a44-4bd0-9c66-4a2dc9bd46e9.png)

### Data

`App.js`:

```javascript
import { DataStore } from '@aws-amplify/datastore';
import { useEffect, useState } from 'react';
import { Recipe } from './models';

function App() {
  const [recipes, setRecipes] = useState([])

  const getRecipes = async() => {
    const models = await DataStore.query(Recipe);
    setRecipes(models);
  }

  useEffect(() => {getRecipes()})
  const createRecipe = async() => {
    const recipe = {
      name: prompt('Name'),
      description: prompt('Description'),
      photo: prompt('Photo')
    }

    await DataStore.save(
      new Recipe(recipe)
    );
  }
  return (
    <div className="App">
      <button onClick={createRecipe}>Create Recipe</button>
      {recipes.map(
        recipe => <div key={recipe.id}>
          <h1>{recipe.name}</h1>
          <p>{recipe.description}</p>
          <img src={recipe.photo} />
        </div>
      )}
    </div>
  );
}

export default App;
```

### Image Credits

- Brigadeiro: 
  - https://unsplash.com/photos/jGh5Qj4jQ-s 
  - https://images.unsplash.com/photo-1630953900039-960a64b49176?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1035&q=80

- Tacos:
  - https://unsplash.com/photos/iU26ZSJh9yw
  - https://images.unsplash.com/photo-1627564803215-ad55bad5c5ea?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80

- Carbonara:
  -  https://unsplash.com/photos/dvgjof0_TP8
  -  https://images.unsplash.com/photo-1608756687911-aa1599ab3bd9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80

- IPA:
  - https://unsplash.com/photos/arDu4d4xc4E
  - https://images.unsplash.com/photo-1561559094-5838ef543170?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1974&q=80

- Pizza:
  - https://unsplash.com/photos/Oxb84ENcFfU
  - https://images.unsplash.com/photo-1590947132387-155cc02f3212?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80

- Mojito
  - https://unsplash.com/photos/gtDYwUIr9Vg
  - https://images.unsplash.com/photo-1513558161293-cdaf765ed2fd?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80
