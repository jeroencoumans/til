# Mocking scoped packages

This is a simple recipe for mocking a function in a node package. Let's say you have export a bunch of functions from your library:

```js
// @company/client-api

export function saveUserData (data) {
  return fetch('/api/user/data', {
    method: 'POST',
    body: JSON.stringify(data)
  })
}

export function getUserData () {
  return fetch('/api/user/data')
}

// ... etc

```

Now in your app, you call this library:

```js
import { saveUserData } from '@company/client-api'

class App {
  // ... snipped

  sync () {
    let data = this.getData()
    saveUserData(data)
      .then(response => {
        // ... snipped
      })
  }
}

```

When you test it, you don't want to actually call your API. So you need to mock your `@company/client-api` library. This works as follows:

```js
jest.mock('@myorg/cool-library', () => {
  // whatever we return here should match what we're using in our App
  // since we only use `saveUserData`, that is sufficient:
  return {
    saveUserData: () => Promise.resolve({ result: 'it works' })
  }
})

```