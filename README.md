# Gifted Chat

[![npm downloads](https://img.shields.io/npm/dm/react-native-gifted-chat.svg)](https://www.npmjs.com/package/react-native-gifted-chat)
[![npm version](https://img.shields.io/npm/v/react-native-gifted-chat.svg)](https://www.npmjs.com/package/react-native-gifted-chat)
[![Latest GitHub tag](https://img.shields.io/github/tag/FaridSafi/react-native-gifted-chat.svg)](https://github.com/FaridSafi/react-native-gifted-chat)

The most complete chat UI for React Native (formerly known as Gifted Messenger).

![screenshot 1](https://raw.githubusercontent.com/FaridSafi/react-native-gifted-chat/master/screenshots/gifted-chat-1.png)
![screenshot 2](https://raw.githubusercontent.com/FaridSafi/react-native-gifted-chat/master/screenshots/gifted-chat-2.png)

## Features

- Fully customizable components
- Composer actions (to attach photos, etc.)
- Load earlier messages
- Copy messages to clipboard
- Touchable links using [react-native-parsed-text](https://github.com/taskrabbit/react-native-parsed-text)
- Avatar as user's initials
- Localized dates
- Multiline TextInput
- InputToolbar avoiding keyboard
- Redux support

## Dependency

- Use version `0.2.x` for RN `>= 0.44.0`
- Use version `0.1.x` for RN `>= 0.40.0`
- Use version `0.0.10` for RN `< 0.40.0`

## Installation

- Using [npm](https://www.npmjs.com/#getting-started): `npm install react-native-gifted-chat --save`
- Using [Yarn](https://yarnpkg.com/): `yarn add react-native-gifted-chat`

## Example

```jsx
import { GiftedChat } from 'react-native-gifted-chat';

class Example extends React.Component {

  state = {
    messages: [],
  };

  componentWillMount() {
    this.setState({
      messages: [
        {
          _id: 1,
          text: 'Hello developer',
          createdAt: new Date(),
          user: {
            _id: 2,
            name: 'React Native',
            avatar: 'https://facebook.github.io/react/img/logo_og.png',
          },
        },
      ],
    });
  }

  onSend(messages = []) {
    this.setState((previousState) => ({
      messages: GiftedChat.append(previousState.messages, messages),
    }));
  }

  render() {
    return (
      <GiftedChat
        messages={this.state.messages}
        onSend={(messages) => this.onSend(messages)}
        user={{
          _id: 1,
        }}
      />
    );
  }

}
```

## Advanced example

See [example/App.js](example/App.js) for a working demo!

## Message object

e.g.

```js
{
  _id: 1,
  text: 'My message',
  createdAt: new Date(Date.UTC(2016, 5, 11, 17, 20, 0)),
  user: {
    _id: 2,
    name: 'React Native',
    avatar: 'https://facebook.github.io/react/img/logo_og.png',
  },
  image: 'https://facebook.github.io/react/img/logo_og.png',
  // Any additional custom parameters are passed through
}
```

## Props

- **`messages`** _(Array)_ - Messages to display
- **`text`** _(String)_ - Input text; default is `undefined`, but if specified, it will override GiftedChat's internal state (e.g. for redux; [see notes below](#notes-for-redux))
- **`placeholder`** _(String)_ - Placeholder when `text` is empty; default is `'Type a message...'`
- **`messageIdGenerator`** _(Function)_ - Generate an id for new messages. Defaults to UUID v4, generated by [uuid](https://github.com/kelektiv/node-uuid)
- **`user`** _(Object)_ - User sending the messages: `{ _id, name, avatar }`
- **`onSend`** _(Function)_ - Callback when sending a message
- **`locale`** _(String)_ - Locale to localize the dates
- **`timeFormat`** _(String)_ - Format to use for rendering times; default is `'LT'`
- **`dateFormat`** _(String)_ - Format to use for rendering dates; default is `'ll'`
- **`isAnimated`** _(Bool)_ - Animates the view when the keyboard appears
- **`loadEarlier`** _(Bool)_ - Enables the "Load earlier messages" button
- **`onLoadEarlier`** _(Function)_ - Callback when loading earlier messages
- **`isLoadingEarlier`** _(Bool)_ - Display an `ActivityIndicator` when loading earlier messages
- **`renderLoading`** _(Function)_ - Render a loading view when initializing
- **`renderLoadEarlier`** _(Function)_ - Custom "Load earlier messages" button
- **`renderAvatar`** _(Function)_ - Custom message avatar; set to `null` to not render any avatar for the message
- **`showUserAvatar`** _(Function)_ - Whether to render an avatar for the current user; default is `false`, only show avatars for other users
- **`onPressAvatar`** _(Function(`user`))_ - Callback when a message avatar is tapped
- **`renderAvatarOnTop`** _(Bool)_ - Render the message avatar at the top of consecutive messages, rather than the bottom; default is `false`
- **`renderBubble`** _(Function)_ - Custom message bubble
- **`onLongPress`** _(Function(`context`, `message`))_ - Callback when a message bubble is long-pressed; default is to show an ActionSheet with "Copy Text" (see [example using `showActionSheetWithOptions()`](https://github.com/FaridSafi/react-native-gifted-chat/blob/master@%7B2017-09-25%7D/src/Bubble.js#L96-L119))
- **`renderMessage`** _(Function)_ - Custom message container
- **`renderMessageText`** _(Function)_ - Custom message text
- **`renderMessageImage`** _(Function)_ - Custom message image
- **`imageProps`** _(Object)_ - Extra props to be passed to the [`<Image>`](https://facebook.github.io/react-native/docs/image.html) component created by the default `renderMessageImage`
- **`lightboxProps`** _(Object)_ - Extra props to be passed to the `MessageImage`'s [Lightbox](https://github.com/oblador/react-native-lightbox)
- **`renderCustomView`** _(Function)_ - Custom view inside the bubble
- **`renderDay`** _(Function)_ - Custom day above a message
- **`renderTime`** _(Function)_ - Custom time inside a message
- **`renderFooter`** _(Function)_ - Custom footer component on the ListView, e.g. `'User is typing...'`; see [example/App.js](example/App.js) for an example
- **`renderChatFooter`** _(Function)_ - Custom component to render below the MessageContainer (separate from the ListView)
- **`renderInputToolbar`** _(Function)_ - Custom message composer container
- **`renderComposer`** _(Function)_ - Custom text input message composer
- **`renderActions`** _(Function)_ - Custom action button on the left of the message composer
- **`renderSend`** _(Function)_ - Custom send button; you can pass children to the original `Send` component quite easily, for example to use a custom icon ([example](https://github.com/FaridSafi/react-native-gifted-chat/pull/487))
- **`renderAccessory`** _(Function)_ - Custom second line of actions below the message composer
- **`onPressActionButton`** _(Function)_ - Callback when the Action button is pressed (if set, the default `actionSheet` will not be used)
- **`bottomOffset`** _(Integer)_ - Distance of the chat from the bottom of the screen (e.g. useful if you display a tab bar)
- **`minInputToolbarHeight`** _(Integer)_ - Minimum height of the input toolbar; default is `44`
- **`listViewProps`** _(Object)_ - Extra props to be passed to the messages [`<ListView>`](https://facebook.github.io/react-native/docs/listview.html); some props can't be overridden, see the code in `MessageContainer.render()` for details
- **`keyboardShouldPersistTaps`** _(Enum)_ - Determines whether the keyboard should stay visible after a tap; see [`<ScrollView>`](https://facebook.github.io/react-native/docs/scrollview.html) docs
- **`onInputTextChanged`** _(Function)_ - Callback when the input text changes
- **`maxInputLength`** _(Integer)_ - Max message composer TextInput length
- **`parsePatterns`** _(Function)_ - Custom parse patterns for [react-native-parsed-text](https://github.com/taskrabbit/react-native-parsed-text) used to linkify message content (like URLs and phone numbers), e.g.:

    ```js
    <GiftedChat
      parsePatterns={(linkStyle) => [
        { type: 'phone', style: linkStyle, onPress: this.onPressPhoneNumber },
        { pattern: /#(\w+)/, style: { ...linkStyle, styles.hashtag }, onPress: this.onPressHashtag },
      ]}
    />
    ```

## Imperative methods

- `focusTextInput()` - Open the keyboard and focus the text input box

## Notes for [Redux](https://github.com/reactjs/redux)

The `messages` prop should work out-of-the-box with Redux. In most cases this is all you need.

If you decide to specify a `text` prop, GiftedChat will no longer manage its own internal `text` state and will defer entirely to your prop.
This is great for using a tool like Redux, but there's one extra step you'll need to take:
simply implement `onInputTextChanged` to receive typing events and reset events (e.g. to clear the text `onSend`):

```js
<GiftedChat
  text={customText}
  onInputTextChanged={(text) => this.setCustomText(text)}
  /* ... */ />
```

## Notes for Android

If you are using Create React Native App / Expo, no Android specific installation steps are required -- you can skip this section. Otherwise we recommend modifying your project configuration as follows.

- Make sure you have `android:windowSoftInputMode="adjustResize"` in your `AndroidManifest.xml`:

    ```xml
    <activity
      android:name=".MainActivity"
      android:label="@string/app_name"
      android:windowSoftInputMode="adjustResize"
      android:configChanges="keyboard|keyboardHidden|orientation|screenSize">
    ```

- If you plan to use `GiftedChat` inside a `Modal`, see [#200](https://github.com/FaridSafi/react-native-gifted-chat/issues/200).

## Notes for local development

You can use [`wml`](https://github.com/wix/wml) to keep the example app in sync
with any changes you make to the library during development. Steps:

1. Install it: `npm install -g wml`
2. Configure it: `wml add . example/node_modules/react-native-gifted-chat` from the root directory
3. `cd example`
4. `npm start`
5. `wml start` in another terminal window (doesn't matter where)

Note that it's important for `wml start` to come **after** `npm start`, or you'll get `Can't find entry file index.js` errors.
If you have any issues, you can clear your watches using `watchman watch-del-all` and try again.

## License

- [MIT](LICENSE)

## Author

Feel free to ask me questions on Twitter [@FaridSafi](https://www.twitter.com/FaridSafi)!
