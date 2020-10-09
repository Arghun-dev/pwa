# pwa

**PWAs, can be easily created using HTML, CSS & JS, they can be run on all browsers, operating systems and devices, PWA's also have access to native features: they can work offline, they can be installed to your mobile home screen and they have access to mobile features**

`React Tip`:

**in React you can implement your requests in separate file and the use them inside your components**

for example i create a folder called `requests` and inside that I can declare several `js files` to hanle my requests

**for example I create a file called in this project I create a file called `fetchWeather.js` inside the `requests` folder just like this**


fetchWeather.js
```js
import axios from 'axios'

const URL = "https://community-open-weather-map.p.rapidapi.com/weather";
const API_KEY = "f33a484cf794d08d0148764789aaba32";

export const fetchWeather = async (query) => {
  try {
    const { data } = await axios.get(URL, {
      params: {
        q: query,
        units: "metric",
        APPID: API_KEY,
      },
    })
    
    return data
  } catch (err) {
    console.log(err)
  }
}
```

And then you can call the `fetchWeather` function in your component to get data just like below:
for example I want to call fetchWeather when user hit enter

App.js
```js
import React, { useState } from 'react'
import { fetchWeather } from './requests/Requests'

export default function App() {
  const [query, setQuery] = useState('')
  const [weather, setWeather] = useState({})
  console.log(weather)

  const search = async (e) => {
    if (e.key === 'Enter') {
      const data = await fetchWeather(query)
      setWeather(data)
      setQuery('')
    }
  }

  return (
    <>
    <input
      value={query}
      onChange={e => setQuery(e.target.value)}
      onKeyPress={search} 
    />
    {weather.main && (
      <div>
      <h2>
        <span>{weather.name}</span>
        <sup>{weather.sys.country}</sup>
      </h2>
      <div>
        {Math.round(weather.main.temp)}
        <sup>&deg; C</sup>
      </div>
      </div>
    )}
    </>
  )
}
```


**till now was the React side of the project, now we want to add the `pwa` side of the application**

**Now we want to add the functionality to see the application offline and make the app installable on the `web` and `mobile devices`**

**in this part we're not concerned with the `src` folder, we're done with that**

**Now we have to focus on `public` folder**

**what we're going to do, is to delete everything in `public` folder besides `index.html`, so we can re-create these things ourselves to understand what's going on**

### main part (adding the service worker)

**Service Worker is a Javascript file which runs all the time, as soon as you open the page it starts running, and it keeps running even after you closed the page, for that reason service workers cam help us with a lot of features, for example they can push notificaions from your mobile phone**

**right now we're going to register our first service worker, we do that exactly inside `index.html` just inside `body` of `index.html` we're going to create `script tag`**

1. first we have to check weather the service worker is supported by our current browser

index.html
```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <script>
      if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
          navigator.serviceWorker.register('./serviceworker.js')
            .then((reg) => console.log('Success :', reg.scope))
            .catch((err) => console.log(err))
        })
      }
    </script>
  </body>
</html>
```

2. now we're create the file `serviceworker.js` inside `public` folder
