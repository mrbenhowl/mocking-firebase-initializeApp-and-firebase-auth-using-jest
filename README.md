# Mocking firebase.initializeApp() and firebase.auth() using JavaScript/Jest

```javascript

import * as firebase from 'firebase'

const onAuthStateChanged = jest.fn()

const getRedirectResult = jest.fn(() => {
  return Promise.resolve({
    user: {
      displayName: 'redirectResultTestDisplayName',
      email: 'redirectTest@test.com',
      emailVerified: true
    }
  })
})

const sendEmailVerification = jest.fn(() => {
  return Promise.resolve('result of sendEmailVerification')
})

const sendPasswordResetEmail = jest.fn(() => Promise.resolve())

const createUserWithEmailAndPassword = jest.fn(() => {
  return Promise.resolve('result of createUserWithEmailAndPassword')
})

const signInWithEmailAndPassword = jest.fn(() => {
  return Promise.resolve('result of signInWithEmailAndPassword')
})

const signInWithRedirect = jest.fn(() => {
  return Promise.resolve('result of signInWithRedirect')
})

const initializeApp = jest
  .spyOn(firebase, 'initializeApp')
  .mockImplementation(() => {
    return {
      auth: () => {
        return {
          createUserWithEmailAndPassword,
          signInWithEmailAndPassword,
          currentUser: {
            sendEmailVerification
          },
          signInWithRedirect
        }
      }
    }
  })

jest.spyOn(firebase, 'auth').mockImplementation(() => {
  return {
    onAuthStateChanged,
    currentUser: {
      displayName: 'testDisplayName',
      email: 'test@test.com',
      emailVerified: true
    },
    getRedirectResult,
    sendPasswordResetEmail
  }
})

firebase.auth.FacebookAuthProvider = jest.fn(() => {})
firebase.auth.GoogleAuthProvider = jest.fn(() => {})

```

# example usage

```javascript

test('<insert your method name> that calls firebase.auth().getRedirectResult', async () => {
      const result = await yourModuleName.getRedirectResult() // (i.e. your method that calls firebase.auth().getRedirectResult)
      expect(result).toEqual({
        user: {
          displayName: 'redirectResultTestDisplayName',
          email: 'redirectTest@test.com',
          emailVerified: true
        }
      })
      expect(firebase.auth).toHaveBeenCalled()
      expect(getRedirectResult).toHaveBeenCalled()
    })
    
```
