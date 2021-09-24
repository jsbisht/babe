### firestore security rules

https://firebase.google.com/docs/reference/security/database

### functions debug with emulator

The following config sets the port at which the emulator will start functions on and start listening to debugger on 9229 (if port not specified).

```json
"emulators": {
  "functions": {
    "host": "localhost",
    "port": "8585"
  }
}
```

## firebase cli

Sign in to firebase through browser

> firebase login:ci

Listing the projects

> firebase projects:list

To switch the currently active project

> firebase use --add

## CRUD

### Add

https://firebase.google.com/docs/firestore/manage-data/add-data

```
// Add a new document with a generated id.
const res = await db.collection('cities').add({
  name: 'Tokyo',
  country: 'Japan'
});
```

### Get

https://firebase.google.com/docs/firestore/query-data/get-data

```
const citiesRef = db.collection('cities');
const snapshot = await citiesRef.where('capital', '==', true).get();
if (snapshot.empty) {
  console.log('No matching documents.');
  return;
}  

snapshot.forEach(doc => {
  console.log(doc.id, '=>', doc.data());
});
```

### Put

https://firebase.google.com/docs/firestore/manage-data/add-data#set_a_document

To create or overwrite a single document, use the set() method:

> const res = await db.collection('cities').doc('LA').set(data);

### Delete

https://firebase.google.com/docs/firestore/manage-data/delete-data

> const res = await db.collection('cities').doc('DC').delete();index.js
