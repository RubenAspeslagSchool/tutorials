# push notifications


sw:

```js
self.addEventListener('push', e => {
  const data = e.data.json();

  const text = `${data['user']} catched a ${data['pokemon']}`;
  const img = '/icons/pokebal-192x192.png';
  self.registration.showNotification('Pokédex', {body: text, icon: img});
});

```


in js client:

```js
import { registerUser } from "./friends.js";

const VAPID_PUBLIC_KEY = 'BPNcLmvj3iXcTo9-P-7Tmz0pjE4pv5XwJslxmIE9iM3_dyJEosEFuzE2bK1EtrxFI_4mITNmcKgcn9KmJrll_4Y';

async function initPushNotifications() {
  if (await registerNotifications) {
    registerPush();
  }
}

async function registerNotifications() {
  await navigator.serviceWorker.ready;
  if ("Notification" in window) {
    const permission = Notification.requestPermission();
    return permission === "granted";
  }
  return false;
}

async function registerPush() {

  const subscribeOptions = { 
    userVisibleOnly: true, 
    applicationServerKey: urlBase64ToUint8Array(VAPID_PUBLIC_KEY) 
  };

  const registration = await navigator.serviceWorker.ready;
  const subscription = await registration.pushManager.subscribe(subscribeOptions);
  
  registerUser(subscription);
}

function urlBase64ToUint8Array(base64String) {
  var padding = '='.repeat((4 - base64String.length % 4) % 4);
  var base64 = (base64String + padding)
      .replace(/\-/g, '+')
      .replace(/_/g, '/');
  var rawData = window.atob(base64);
  var outputArray = new Uint8Array(rawData.length);
  for (var i = 0; i < rawData.length; ++i) {
      outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}

export { initPushNotifications }

```


in js server


```js
import webpush from 'web-push';

/*
console.log( webpush.generateVAPIDKeys() );

{
  publicKey: 'BOvCzJwV2vJQXW6PslPd-ANhJY2giVXYE3avqC9pqRr58oYa--uws_2bCc6OAqXR3m3iuQxZcSdCUS9vxqLXqXg',
  privateKey: 'ZRtc2oY1OjJQ50iFdGzmCo1ayf-HCEAPPP8MEMly8kA'
}

*/


const vapidKeys = {
  publicKey: "BOvCzJwV2vJQXW6PslPd-ANhJY2giVXYE3avqC9pqRr58oYa--uws_2bCc6OAqXR3m3iuQxZcSdCUS9vxqLXqXg",
  privateKey: "ZRtc2oY1OjJQ50iFdGzmCo1ayf-HCEAPPP8MEMly8kA"
};

webpush.setVapidDetails("mailto:frederic.vlummens@howest.be", vapidKeys.publicKey, vapidKeys.privateKey);

const subscription = {"endpoint":"https://fcm.googleapis.com/fcm/send/ezchGJToIDY:APA91bEOdQwmYHIfUtPQltB8BjED0uAWmrb2y52c2XTZDv0khfcxrrVz1C1yg_BGiC8RU6SQnj0HFHu5FiLctseUxbTcW90codpn9Vsr8UPs7AnKxYF_BuNg1sxSfiEyTNFgWyLu8LxP","expirationTime":null,"keys":{"p256dh":"BEXPcMu5G3SDE5iCi5jITVUe1y7huKm9Nki5Wqgb8ewNTJDOaojNwAA6j3JZ9CMxrHfEfsvfP-HbWm-jl819FOs","auth":"_tcnbwPvI9aAkLOvW2_CPw"}};

const payload = {
  title: "Hello world",
  body: "Hello world from Howest",
  url: "http://www.howest.be"
};

webpush.sendNotification(subscription, JSON.stringify(payload));
```
