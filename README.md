# Crypto in React Native
Encryption and random numbers for projects made with React Native

**Note: Still a work-in-progress. Issues and PRs are welcome!**

# Table of Contents
1. Strategies for using libaries that were made targeting a node or browser environment
1. To eject or not to eject (from Expo)?
1. Options for encryption
1. Options for random number generation

---

### Strategies for using libaries that were made targeting a node or browser environment
If you want to use a higher-level library with its own dependencies, it's pretty likely that one of those dependencies was built assuming a node or browser environment (as opposed to a React Native environment), which means it's likely it depends on a method or global variable that is only defined in those environments. For an example that's not particularly crypto-related, node assumes access to the computer's file system with the node core `fs` module.

##### Just don't use such libraries

If you're making your own library or an app that doesn't have any high-level dependencies, the "cleanest" option is to just use dependencies that were built to work in a React Native environment in the first place.

##### `metro.config.js` and `extraNodeModules`

Using the resolver settings for the `metro` bundler that React Native uses, ensure that a given dep always resolves to a different dep in a React Native environment. The `extraNodeModules` feature in the `metro.config.js` helps with that.

##### `rn-nodeify`

`rn-nodeify` searches through your `node_modules` and recursively modifies the `package.json` for every dep and modifies which packages they depend on in a React Native env. It adds a `shim` file to your project as well for global polyfills like `process` and `Buffer`. With the `hack` flag, needed for some packages like `react-native-randombyte`s, it modifies the code directly if certain regular expressions are matched.

You'll most likely come across this approach first when searching online, but the more direct approach with `metro.config.js` is easier to reason about from both a security perspective an a DevEx perspective.

---

### To eject or not to eject (from Expo)?
`react-native-crypto` requires a peer dep of `react-native-randombytes`

`react-native-randombytes` requires ejecting, so unless we can find a replacement for that package, the repo will be an ejected project.

Subbing in `expo-random` as the source of randomness in forks of existing crypto libs seems like a good strategy around this, but handling which methods are sync vs. async is a potential sticking point to work around with that approach. (Many methods in React Native packages are async because they require a round trip across the js/native bridge.) See [this TweetNaCl.js fork](https://github.com/rajtatata/react-native-expo-tweet-nacl#readme) for an example.

---

### Options for encryption

##### Not matching node or web APIs

[`expo-crypto`](https://github.com/expo/expo/tree/master/packages/expo-crypto)

[`tweetnacl-js`](https://github.com/dchest/tweetnacl-js)
* perhaps subbing in `expo-random` as the source of randomness like was done for [this TweetNaCl.js fork](https://github.com/rajtatata/react-native-expo-tweet-nacl#readme)

##### Matching [node API](https://nodejs.org/docs/latest-v12.x/api/crypto.html)

`react-native-crypto`
* requires ejecting because it has a peer dep of `react-native-randombytes`
* installation instructions assume `rn-nodeify` strategy for polyfilling

##### Matching [web API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
`// TODO: Add me`

##### Options that may show up when searching online that are no longer actively maintained
`// TODO: Add me`

---

### Options for random number generation

##### Not matching node or web APIs
[`expo-random`](https://github.com/expo/expo/tree/master/packages/expo-random)

##### Matching node API

[`react-native-randombytes`](https://github.com/mvayngrib/react-native-randombytes/)
* requires ejecting
* installation instructions assume `rn-nodeify` strategy for polyfilling

##### Matching web API
`// TODO: Add me`

##### Options that may show up when searching online that are no longer actively maintained
`// TODO: Add me`
