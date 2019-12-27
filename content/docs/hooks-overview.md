---
id: hooks-overview
title: एक नज़र में Hooks
permalink: docs/hooks-overview.html
next: hooks-state.html
prev: hooks-intro.html
---

React 16.8 में *Hooks* नामक एक नया फीचर ऐड हुआ हैं। यह आपको class का इस्तेमाल बिना state और अन्य React विशेषताओं का उपयोग करने देता हैंं।

Hooks [बैकवर्डस-कम्पेटिबल](/docs/hooks-intro.html#no-breaking-changes) हैं। यह पेज अनुभवी React उपयोगकर्ताओं के लिए Hooks का ओवरव्यू प्रदान करता हैं। यह एक तेज ओवरव्यू हैं। यदि आप उलझन में पड़ जाते हैंं, तो निचे दिखाए गए पीले बॉक्स की तलाश करे:

>विस्तृत विवरण
>
>यह जानने के लिए [मोटिवेशन](/docs/hooks-intro.html#motivation) पढ़ें कि हम Hooks का React में परिचय क्यों कर रहे हैंं।

**↑↑↑ प्रत्येक खंड इस तरह से पीले बॉक्स के साथ समाप्त होता हैं।** वे विस्तृत स्पष्टीकरण से जुड़ते हैंं।

## 📌 State Hook {#state-hook}

यह उदाहरण एक काउंटर को रेंडर करता हैं। जब आप बटन पर क्लिक करते हैंं, तो यह वैल्यू को बढ़ाता हैं।

```js{1,4,5}
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

यहाँ, `useState` एक *Hook* हैं (हम इसके बारे में एक पल में बात करेंगे)। हम इसे फंक्शन कौम्पोनॅन्ट के अंदर लोकल state डालने के लिए कॉल करते हैं। री-रेंडरर्स के बीच React इस state को संरक्षित करता हैं। `useState` एक पेअर देता हैं: एक *current* state वैल्यू और एक फ़ंक्शन जो आपको इसे अपडेट करने देता हैं। आप इस फ़ंक्शन को किसी इवेंट हैंंडलर या कहीं और से कॉल कर सकते हैंं। यह एक class में `this.setState` के समान हैं, लेकिन यह पुराने और नए state को एक साथ नहीं मिलाता। (हम [State Hook उपयोग करते समय](/docs/hooks-state.html) में एक उदाहरण दिखाएंगे जिसमे `useState` की `this.state` से तुलना की गयी हैं।

`UseState` का एकमात्र पैरामीटर initial state हैं। ऊपर दिखाए हुए उदाहरण में, यह `0` हैं क्योंकि हमारा काउंटर शून्य से शुरू होता हैं। ध्यान दें कि `this.state` के विपरीत, यहाँ state के पास कोई object नहीं हैं -- हालाँकि इसके पास हो सकता हैं यदि आप चाहें। Initial state का पैरामीटर केवल पहले रेंडर के दौरान उपयोग किया जाता हैं।

#### कई state वेरिएबल्स को घोषित करना। {#declaring-multiple-state-variables}

आप एक कौम्पोनॅन्ट में एक से अधिक बार State Hook का उपयोग कर सकते हैंं:

```js
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

[ऐरे डेस्ट्रक्टरिंग] सिंटेक्स हमें state वेरिएबल्स को अलग-अलग नाम देने की सुविधा देता हैं जो हमें `useState` को घोषित करके मिलते हैं। ये नाम `useState` एपीआई का हिस्सा नहीं हैंं। इसके बजाय, React मानता हैं कि यदि आप कई बार `useState` कॉल करते हैंं, तो आप इसे हर रेंडर के दौरान उसी क्रम में करते हैंं। हम वापस आएंगे कि यह क्यों काम करता हैं और यह बाद में कहाँ उपयोगी होता हैं।

#### लेकिन hook हैं क्या? {#but-what-is-a-hook}

Hooks ऐसे फंक्शन हैंं जो आपको फ़ंक्शन कौम्पोनॅन्टस से React state "में Hooks" और लाइफ साइकिल की सुविधा देते हैंं। Hooks क्लासेस के अंदर काम नहीं करते - वे आपको क्लासेस के बिना React का उपयोग करने देते हैंं। (हम [सलाह नहीं देते](/docs/hooks-intro.html#gradual-adoption-strategy) की आप अपने मौजूदा कौम्पोनॅन्टस को रात भर में Hooks में परिवर्तित करदे लेकिन यदि आप चाहें तो नए कौम्पोनॅन्टस में Hooks का उपयोग शुरू कर सकते हैंं।)

React `useState` जैसे कुछ बने बनाये Hooks प्रदान करता हैं। आप विभिन्न कौम्पोनॅन्टस के बीच stateful बिहेवियर का पुन: उपयोग करने के लिए अपने स्वयं के Hooks भी बना सकते हैंं। हम बने बनाये Hooks को पहले देखते हैं।

>विस्तृत विवरण
>
>आप समर्पित पेज पर state hook के बारे में अधिक जान सकते हैंं: [State Hook का उपयोग करना](/docs/hooks-state.html)

## ⚡️ Effect Hook {#effect-hook}

आपने संभवतः पहले डेटा फेचिंग, सब्स्क्रिप्शन, या मैन्युअल रूप से DOM को React कौम्पोनॅन्टस से बदलकर दिखाया हुआ हैं। हम इन ऑपरेशनों को "साइड इफेक्ट्स" (या शॉर्ट के लिए "effects") कहते हैंं क्योंकि वे अन्य कौम्पोनॅन्टस को प्रभावित कर सकते हैंं और रेंडरिंग के दौरान नहीं किये जा सकते।

Effect Hook, `useEffect`, एक फ़ंक्शन कौम्पोनॅन्टस से साइड इफेक्ट करने की क्षमता देता हैं। यह उसी प्रकार में कार्य करता हैं जो `componentDidMount`, `componentDidUpdate`, और `componentWillUnmount` React classes में करते हैं, लेकिन यह एक ही API में एकीकृत हैं। (हम इन तरीकों की तुलना में `useEffect` की तुलना करने वाले उदाहरण यहाँ दिखाएंगे [Effect Hook का उपयोग करना](/docs/hooks-effect.html)।)

उदाहरण के लिए, React DOM को अपडेट करने के बाद यह कौम्पोनॅन्ट डॉक्यूमेंट का शीर्षक सेट करता हैं: 

```js{1,6-10}
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

जब आप `useEffect` कॉल करते हैंं, तो आप DOM के परिवर्तन फ्लश करने के बाद अपने "effect" फ़ंक्शन को चलाने के लिए React को बता रहे हैंं। कौम्पोनॅन्टस के अंदर effects को डिक्लेअर किया जाता हैं ताकि उनके पास इसके props और state तक पहुंच हो। डिफ़ॉल्ट रूप से, React हर रेंडर के बाद effects चलाता हैं -- पहले रेंडर *के साथ भी* (हम इस बारे में और बात करेंगे की [Effect hook इस्तेमाल करते समय](/docs/hooks-effect.html) ये class lifecycles से कैसे तुलना करता हैं।)

Effects ऑप्शनल रूप से यह भी निर्दिष्ट कर सकते हैंं कि किसी फ़ंक्शन को वापस करके उनके बाद उसका "clean up" कैसे करें। उदाहरण के लिए, यह कौम्पोनॅन्ट किसी मित्र की ऑनलाइन स्टेटस को सब्सक्राइब करने के लिए एक effect का उपयोग करता हैं, और बाद मैं इससे अनसब्सक्राइब करके क्लीन करता हैं:

```js{10-16}
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

इस उदाहरण में, React हमारे `ChatAPI` से तब अनसब्सक्राइब हो जाती हैं जब कौम्पोनॅन्ट अनमाउंट हो जाता हैं, साथ ही अगले रेंडर की वजह से effect को दुबारा चलाने से पहले। (यदि आप चाहते हैंं, यहाँ एक तरीका हैं [React को बताने का की री-सब्सक्राइबिंग को कैसे स्किप करें](/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) अगर `props.friend.id` नहीं बदलता जो हमने `ChatAPI` को पास किया हैं।)

`useState` की तरह, आप एक कौम्पोनॅन्ट में एक से अधिक effect का उपयोग कर सकते हैंं:

```js{3,8}
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
```

Hooks आपको एक कौम्पोनॅन्ट में साइड इफेक्ट को व्यवस्थित करने देते हैंं जो pieces से संबंधित होते हैंं (जैसे कि सब्सक्रिप्शन लेना और हटाना), बजाय इसके इसको लाइफसाइकिल तरीकों के आधार पर विभाजित करने के लिए मजबूर करना।

>विस्तृत विवरण
>
>आप एक समर्पित पेज पर `useEffect` के बारे में अधिक जान सकते हैंं: [Effect Hook का उपयोग करना](/docs/hooks-effect.html)

## ✌️ Hook के नियम {#rules-of-hooks}

Hooks JavaScript फंक्शन हैंं, लेकिन वे दो अतिरिक्त नियम लागू करते हैंं:

* Hooks को केवल **सबसे उपर** कॉल करें। लूप्स, कंडीशंस, या नेस्टेड फ़ंक्शन के अंदर Hook को कॉल न करें।
* Hooks को सिर्फ **React फ़ंक्शन कौम्पोनॅन्ट** मैं ही कॉल करें। नियमित जावास्क्रिप्ट फ़ंक्शंस से Hook कॉल न करें। (Hooks को कॉल करने के लिए सिर्फ एक अन्य वैध स्थान हैं -- अपने स्वयं के कस्टम Hooks। हम उनके बारे में एक क्षण में जानेंगे।)

हम एक [लिंटर प्लगइन](https://www.npmjs.com/package/eslint-plugin-react-hooks) प्रदान करते हैंं जो इन नियमों को स्वचालित रूप से लागू कर देते हैं। हम समझते हैंं कि ये नियम पहले सीमित या भ्रमित हो सकते हैंं, लेकिन वे Hooks को अच्छी तरह से काम करने के लिए आवश्यक हैंं।

>विस्तृत विवरण
>
>आप एक समर्पित पेज पर ये नियम के बारे में अधिक जान सकते हैंं: [Hooks के नियम](/docs/hooks-rules.html)

## 💡 अपने खुद के Hooks का निर्माण {#building-your-own-hooks}

कभी-कभी, हम कौम्पोनॅन्ट के बीच कुछ स्टेटफुल लॉजिक का पुन: उपयोग करना चाहते हैंं। परंपरागत रूप से, इस समस्या के दो लोकप्रिय समाधान थे: [higher-order कौम्पोनॅन्ट](/docs/higher-order-components.html) और [रेंडर प्रॉप्स](/docs/render-props.html)। लेकिन कस्टम Hooks आपको आपके tree में अधिक कौम्पोनॅन्ट को ऐड करे बिना ऐसा करने देते हैंं।

इस पेज मैं पहले, हमने एक `FriendStatus` कौम्पोनॅन्ट पेश किया हैं, जो किसी मित्र के ऑनलाइन स्टेटस की सदस्यता के लिए `useState` और `useEffect` Hook को कॉल करता हैं। मान लें कि हम इस सदस्यता तर्क को किसी अन्य कौम्पोनॅन्ट में पुन: उपयोग करना चाहते हैंं।

सबसे पहले, हम इस लॉजिक को `useFriendStatus` नामक एक कस्टम Hook में निकालेंगे:

```js{3}
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

यह एक आर्गुमेंट के रूप में `friendID` लेता हैं, और रिटर्न करता हैं कि क्या हमारा दोस्त ऑनलाइन हैं।

अब हम इसे दोनों कौम्पोनॅन्टस से उपयोग कर सकते हैंं:


```js{2}
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

```js{2}
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

इन कौम्पोनॅन्टस की स्टेट पूरी तरह से स्वतंत्र हैं। Hooks *स्टेट फुल लॉजिक* का पुन: उपयोग करने का एक तरीका हैं, न कि state का। वास्तव में, एक Hook करने के लिए प्रत्येक *कॉल* में एक पूरी तरह से अलग state हैं -- जिस से आप एक ही कौम्पोनॅन्ट में दो बार एक कस्टम Hook का उपयोग कर सकते हैंं।

कस्टम Hook एक विशेषता से ज़्यादा एक कन्वेंशन हैंं। यदि किसी फ़ंक्शन का नाम "`use`" से शुरू होता हैं और इसे अन्य Hook कॉल करते हैं, तो हम कहते हैंं कि यह एक कस्टम Hook हैं। हमारे लिंटर प्लगइन `useSomething` नामकरण कन्वेंशन का उपयोग करके कोड में बग ढूंढने में मदद करता हैं।

आप कस्टम Hook लिख सकते हैंं जो फॉर्म हैंंडलिंग, एनीमेशन, डेक्लेरेटिव सब्सक्रिप्शन, टाइमर और शायद और अधिक मामलों मे जिसपे हमने विचार नहीं किया, जैसे उपयोग मामलों की एक विस्तृत श्रृंखला को कवर करते हैंं। हम यह देखने के लिए उत्साहित हैंं कि React समुदाय और कौन कौन से कस्टम Hooks लेकर आएंगे।

>विस्तृत विवरण
>
>आप एक समर्पित पेज पर कस्टम Hook के बारे में अधिक जान सकते हैंं: [अपने खुद के Hooks का निर्माण](/docs/hooks-custom.html).

## 🔌 अन्य Hooks {#other-hooks}

कुछ कम इस्तेमाल किए जाने वाले बिल्ड इन Hooks हैंं जो आपको उपयोगी लग सकते हैंं। उदाहरण के लिए, [`useContext`](/docs/hooks-reference.html#usecontext) यह आपको नेस्टिंग के बिना React context को सब्सक्राइब करने देता हैं:

```js{2,3}
function Example() {
  const locale = useContext(LocaleContext);
  const theme = useContext(ThemeContext);
  // ...
}
```

और [`useReducer`](/docs/hooks-reference.html#usereducer) आपको एक reducer के साथ जटिल कौम्पोनॅन्टस के लोकल state का प्रबंधन करने देता हैं:

```js{2}
function Todos() {
  const [todos, dispatch] = useReducer(todosReducer);
  // ...
```

>विस्तृत विवरण
>
>आप एक समर्पित पेज पर सब बिल्ड इन Hooks के बारे में अधिक जान सकते हैंं: [Hooks API संदर्भ](/docs/hooks-reference.html)

## अगला कदम {#next-steps}

अगर आपको कुछ चीजें समझ में नहीं आईं या आप अधिक विस्तार से सीखना चाहते हैंं, आप अगले पेज पढ़ सकते हैंं, [State Hook](/docs/hooks-state.html) डॉक्यूमेंटेशन के साथ शुरू करते हुए।

आप इन पेजों को भी देख सकते हैं: [Hooks API संदर्भ](/docs/hooks-reference.html) और [Hooks FAQ](/docs/hooks-faq.html)

अंत में, [introduction page](/docs/hooks-intro.html) को देखना ना भूले जो बताता हैं कि *हम* Hooks क्यों बना रहे हैंं और कैसे हम classes के साथ-साथ उनका उपयोग करना शुरू करेंगे -- हमारे एप्लिकेशन को फिरसे लिखे बिना।
