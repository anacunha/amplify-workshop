# Full Stack Application with AWS Amplify - Workshop

Esse workshop é baseado no [AWS Amplify Fullstack Workshop](https://github.com/aspittel/amplify-workshop) da [Ali Spittel](https://twitter.com/aspittel).

## O que é Computação em Nuvem?

## O que é o Amplify?

O [AWS Amplify](https://docs.amplify.aws/) é um conjunto de ferramentas e serviços que ajuda desenvolvevores front-end web e mobile a criarem aplicações full-stack na AWS.

Com o Amplify, desenvolvedores podem criar recursos de back-end (como armazenamento de arquivos, autenticação e armazenamento de dados) usando a linha de comando ou uma interface visual e, então, fazer o deploy desses recursos na nuvem.

Em seguida, desenvolvedores podem conectar seu front-end aos recursos de back-end criados ou a recursos já existentes na AWS usando as [bibliotecas do Amplify](https://docs.amplify.aws/lib/). Essas bibliotecas oferecem suporte a frameworks web, incluindo JavaScript, React, Angular, Vue, Next.js e plataformas móveis, incluindo Android, iOS, React Native, Ionic e Flutter. 

Também é possível usar o Amplify para hospedar aplicações web estáticas com apenas alguns cliques e gerenciar o conteúdo da sua aplicação. As diferentes ferramentas do Amplify podem ser usadas independentemente, conforme necessário, ou todas juntas.

A ideia do Amplify é abstrair os serviços específicos que estão por trás das funcionalidades de back-end e focar no desenvolvimento da aplicação em si. E, Como Amplify não requer conhecimento muito profundo da AWS, é um ótimo lugar pra começar a desenvolver aplicações full-stack na nuvem.

- Amplify Framework (open source)
  - CLI: adicionar e configurar diferentes recursos ao back-end nossa aplicação (Auth, API)
  - Libs: pra diferentes clientes, que fazem a integração entre o front-end da aplicação e os recursos no back-end
  - UI Components: simplificam a construção de aplicações responsivas e acessíveis (e.g. componente pra autenticação, UI de chatbot, temas, etc)
- Serviço da AWS
  - Amplify Console

### Amplify Studio

## Passo a Passo

[https://sandbox.amplifyapp.com](https://sandbox.amplifyapp.com)

```shell
npx create-react-app@latest cityjs-app
cd cityjs-app
```

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

- https://docs.amplify.aws/console/
- https://ui.docs.amplify.aws/getting-started/installation
