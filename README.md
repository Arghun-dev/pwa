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
import React from 'react'
import { fetchWeather } from './Requests/fetchWeathe.js'

export default function App() {
  const [query, setQuery] = useState('')
  
  const search = async (e) => {
    if (e.key === 'Enter') {
      const data = await fetchWeather(query)
      
      console.log(data)
    }
  }
  
  return (
    <input
      type="text"
      value={query}
      onChange={e => setQuery(e.target.value)}
    />
  )
}
```
