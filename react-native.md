# React Native

## Install

Using Expo

- `npx create-expo-app@latest`
- Then follow: <https://docs.expo.dev/get-started/set-up-your-environment/>. Installing Android Simulator to preview our app on a simulator device or download Expo Go on mobile to preview on our own device.
- We can also preview app in web version.

## Component

- `View`: like a div, default is `display: flex`
- `Text`: like a p
- `Button`: 
	- default appearance on android differs from ios. 
	- Using `title` for text. 
	- `onPress`
- `TextInput`:
	- `onChangeText: (value) => setValue(value)`
	- `multiline`: turns into textarea
- `FlatList`: render list

```tsx
<FlatList
    renderItem={({ item }) => (
      <Text style={{ padding: 50 }}>{item.name}</Text>
    )}
    data={list}
    numColumns={2}
    keyExtractor={item => item.id.toString()}
/>
```

- `TouchableOpacity`, `Pressable`: like a div but with `onPress`. We can't set `onPress` on `View`
- 'ConfirmationDialog': using Alert

```tsx
onPress = () => Alert.alert('Alert Title', 'My Alert Msg', [
  {
    text: 'Cancel',
    onPress: () => console.log('Cancel Pressed'),
    style: 'cancel',
  },
  {text: 'OK', onPress: () => console.log('OK Pressed')},
]);
```

## Styling

Like styled-component

```js
const styles = StyleSheet.create({
  container: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 8,
  },
})
```

Usage

```tsx	
<View style={styles.container}>Container</View>
```

**Merge style**

Using `[]`

```tsx
<View style={[styles.container, styles.container2]}>Container</View>
```

## Icons

Import from `@expo/vector-icons`

## Font

- Install `expo-font` and `expo-splash-screen`: npx expo install expo-font expo-splash-screen
- Load font: Import font in the root layout

```tsx
import { useFonts } from 'expo-font';

SplashScreen.preventAutoHideAsync();

export default function RootLayout() {
  const colorScheme = useColorScheme();
  const [loaded] = useFonts({
    SpaceMono: require('../assets/fonts/SpaceMono-Regular.ttf'),
  });

  useEffect(() => {
    if (loaded) {
      SplashScreen.hideAsync();
    }
  }, [loaded]);

  if (!loaded) {
    return null;
  }
}
```

- Useage:

```tsx
<Text style={{ fontFamily: 'SpaceMono' }}>Demo font family</Text>
```

## Image

```tsx
<Image
  source={require('@/assets/images/partial-react-logo.png')}
  style={styles.reactLogo}
/>
```


## Navigation

If your app is using Expo, please use `expo-router`

### Define pages

Component: `<Stack>`

You can use drawer or tabs for navigations

```tsx
// <Stack> is like the wrapper
// <Stack.Screen /> defines screen
<Stack>
	{/* (tabs) and +not-found is the name of the folder and file that contains the component we want to render */}
	{/* `name` will be displayed at the top of the page */}
	{/* If we set headerShown as false, this name will be hidden */}
	<Stack.Screen name="(tabs)" options={{ headerShown: false }} />
	<Stack.Screen name="+not-found" />
</Stack>
```

### Navigate between pages

- Using component `Link`

```tsx
<Link href='other_page' aschild><Button title='go to another page' /></Link>
```

- Using method `router.replace` or `router.push` (`push` appends the url to the history while `replace` doens't)

```tsx
<Button title='Go to other page' onPress={() => router.push('other_page')} />
<Button title='Go to other page' onPress={() => router.replace('other_page')} />
```

### Passing params when navigates

```tsx
<Button title='Go to other page' onPress={() => router.push({ pathname: 'other_page', params: { item } })} />
<Link href='other_page' params={{ item }} aschild ><Button title='go to another page' /></Link>
```

In the destination route, use `const params = useLocalSearchParams();` to get the custom params
