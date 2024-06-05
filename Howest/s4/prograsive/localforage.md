# localforage

command:  `npm install localforage -s`   `<script src="">  </script>`

coppy content from [localforagehere libery](https://github.com/RubenAspeslagSchool/tutorials/blob/main/Howest/s4/prograsive/localforage.js)

```js
// Set a localforage instance
localforage.config({
    driver: localforage.INDEXEDDB, // Force IndexedDB; same as using setDriver()
    name: 'myApp',
    version: 1.0,
    storeName: 'keyvaluepairs', // Should be alphanumeric, with underscores.
    description: 'some description'
});

/**
 * Function to store data
 * @param {string} key - The key under which the data is stored.
 * @param {string} value - The value to store.
 */
async function storeData(key, value) {
    try {
        await localforage.setItem(key, value);
        console.log('Data has been stored');
    } catch (err) {
        console.error('Error storing data:', err);
    }
}

/**
 * Function to get data
 * @param {string} key - The key of the data to retrieve.
 * @return {Promise} - Resolves with the retrieved value.
 */
async function getData(key) {
    try {
        const value = await localforage.getItem(key);
        return value;
    } catch (err) {
        console.error('Error getting data:', err);
    }
}



```
