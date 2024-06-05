# localforage

command:  `npm install localforage`   `<script src="">  </script>`

coppy content from [localforagehere libery]()


```js
// Set a localforage instance
localforage.config({
    driver: localforage.INDEXEDDB, // Force IndexedDB; same as using setDriver()
    name: 'myApp',
    version: 1.0,
    storeName: 'keyvaluepairs', // Should be alphanumeric, with underscores.
    description: 'some description'
});

// Function to store data
function storeData() {
    const key = document.getElementById('key').value;
    const value = document.getElementById('value').value;

    localforage.setItem(key, value).then(function () {
        console.log('Data has been stored');
    }).catch(function (err) {
        console.error('Error storing data:', err);
    });
}

// Function to get data
function getData() {
    const key = document.getElementById('key').value;

    localforage.getItem(key).then(function (value) {
        document.getElementById('output').innerText = value ? `Value: ${value}` : 'No value found for this key';
    }).catch(function (err) {
        console.error('Error getting data:', err);
    });
}

```
