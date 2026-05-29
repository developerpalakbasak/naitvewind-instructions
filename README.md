# 🌪️ NativeWind (Tailwind CSS) Setup Guide for Expo

A comprehensive, step-by-step developer guide to successfully installing, configuring, and running **NativeWind (v4)** in a modern **React Native Expo** application.

---

## 🚀 Quick Start & Installation

Follow these steps sequentially to set up NativeWind in your Expo project.

### 1. Initialize the Expo App
If you haven't created a project yet, initialize a new Expo app in your current directory:
```bash
npx create-expo-app@latest ./
```

*(Optional)* Reset the default template project boilerplate if you prefer a clean slate:
```bash
npm run reset-project
```

### 2. Install NativeWind & Peer Dependencies
Install the required packages for NativeWind, including compatible versions of Reanimated and Safe Area Context:
```bash
npm install nativewind react-native-reanimated@~3.17.4 react-native-safe-area-context@5.4.0
```

### 3. Install Tailwind CSS & Prettier (Dev Dependencies)
Install Tailwind CSS and the Prettier plugin for automatic Tailwind class sorting:
```bash
npm install --dev tailwindcss@^3.4.17 prettier-plugin-tailwindcss@^0.5.11
```

---

## 🛠️ Configuration Files

### 1. Initialize & Configure Tailwind CSS
Generate your Tailwind configuration file:
```bash
npx tailwindcss init
```

Open the newly created `tailwind.config.js` file and replace its contents with the configuration below. Make sure to specify the paths to all components and files containing your utility classes:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  // Update this array to include the paths to all files that contain Nativewind classes
  content: [
    "./App.{js,jsx,ts,tsx}",
    "./app/**/*.{js,jsx,ts,tsx}",
    "./src/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}"
  ],
  presets: [require("nativewind/preset")],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### 2. Create the Global CSS File
Create a new file named `global.css` in your root directory and add the Tailwind directives:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 3. Generate and Configure Babel & Metro configs
First, customize your configuration files by running the Expo CLI tool:
```bash
npx expo customize
```
*   Select `babel.config.js`
*   Select `metro.config.js`

Now, update these files with the configurations below to support NativeWind v4 compiling:

#### 📄 `babel.config.js`
Configure Babel to use NativeWind as the JSX import source:
```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [
      ["babel-preset-expo", { jsxImportSource: "nativewind" }],
      "nativewind/babel",
    ],
  };
};
```

#### 📄 `metro.config.js`
Wrap the default Expo Metro configuration with the `withNativeWind` helper:
```javascript
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require("nativewind/metro");

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, { input: "./global.css" });
```

---

## 🔌 Import Global Styles

Finally, import your CSS stylesheet into your root layout file (usually `app/_layout.tsx` or `app/_layout.js`) to apply Tailwind styles across the entire application:

```typescript
import "../global.css";

// Your root layout component goes here
```

---

## 💡 Pro-Tip: TypeScript Support
If you are using TypeScript, create a `nativewind-env.d.ts` file in the root of your project to get auto-complete and full type support for NativeWind classes:
```typescript
/// <reference types="nativewind/types" />
```

---

### 🎉 All Done!
Now you can style your React Native components using standard Tailwind CSS utility classes:
```tsx
import { Text, View } from 'react-native';

export default function HomeScreen() {
  return (
    <View className="flex-1 items-center justify-center bg-slate-900">
      <Text className="text-2xl font-bold text-sky-400">
        NativeWind is working! 🌪️
      </Text>
    </View>
  );
}
```
