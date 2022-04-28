# Full Stack Application with AWS Amplify - Workshop

Esse workshop é baseado no [AWS Amplify Fullstack Workshop](https://github.com/aspittel/amplify-workshop) da [Ali Spittel](https://twitter.com/aspittel).

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

- https://docs.amplify.aws/console/uibuilder/figmatocode/
- https://ui.docs.amplify.aws/getting-started/installation
