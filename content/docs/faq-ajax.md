---
id: faq-ajax
title: AJAX और APIs
permalink: docs/faq-ajax.html
layout: docs
category: FAQ
---

### मैं AJAX कॉल कैसे कर सकता हूं? {#how-can-i-make-an-ajax-call}

आप किसी भी AJAX लाइब्रेरी का उपयोग रिएक्ट के साथ कर सकते हैं। यह कुछ लोकप्रिय AJAX लाइब्रेरी हैं [Axios](https://github.com/axios/axios), [jQuery AJAX](https://api.jquery.com/jQuery.ajax/), और ब्राउज़र अंतर्निहित [window.fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

### कौम्पोनॅन्ट जीवन चक्र में मुझे AJAX कॉल कहां करना चाहिए? {#where-in-the-component-lifecycle-should-i-make-an-ajax-call}

आपको AJAX कॉल्स के साथ डेटा को [`componentDidMount`](/docs/react-component.html#mounting) लाइफ़ साइकिल विधि में पॉप्युलेट करना चाहिए। ऐसा तब होता है जब आप डेटा पुनर्प्राप्त होने पर अपने कौम्पोनॅन्ट को अपडेट करने के लिए `setState` का उपयोग कर सकते हैं।

### उदाहरण: AJAX के उपयोग से लोकल स्टेट सेट होती है {#example-using-ajax-results-to-set-local-state}

नीचे दिया गया कौम्पोनॅन्ट दर्शाता है कि लोकल कौम्पोनॅन्ट स्टेट को पॉप्युलेट करने के लिए `componentDidMount` में AJAX कॉल कैसे करें।

यह उदाहरण API इस तरह एक JSON object देता है:

```
{
  "items": [
    { "id": 1, "name": "Apples",  "price": "$2" },
    { "id": 2, "name": "Peaches", "price": "$5" }
  ] 
}
```

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: null,
      isLoaded: false,
      items: []
    };
  }

  componentDidMount() {
    fetch("https://api.example.com/items")
      .then(res => res.json())
      .then(
        (result) => {
          this.setState({
            isLoaded: true,
            items: result.items
          });
        },
        // Note: it's important to handle errors here
        // instead of a catch() block so that we don't swallow
        // exceptions from actual bugs in components.
        (error) => {
          this.setState({
            isLoaded: true,
            error
          });
        }
      )
  }

  render() {
    const { error, isLoaded, items } = this.state;
    if (error) {
      return <div>Error: {error.message}</div>;
    } else if (!isLoaded) {
      return <div>Loading...</div>;
    } else {
      return (
        <ul>
          {items.map(item => (
            <li key={item.id}>
              {item.name} {item.price}
            </li>
          ))}
        </ul>
      );
    }
  }
}
```

यहाँ हुक के साथ बराबर है: [Hooks](https://reactjs.org/docs/hooks-intro.html): 

```js
function MyComponent() {
  const [error, setError] = useState(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [items, setItems] = useState([]);

  // Note: the empty deps array [] means
  // this useEffect will run once
  // similar to componentDidMount()
  useEffect(() => {
    fetch("https://api.example.com/items")
      .then(res => res.json())
      .then(
        (result) => {
          setIsLoaded(true);
          setItems(result);
        },
        // Note: it's important to handle errors here
        // instead of a catch() block so that we don't swallow
        // exceptions from actual bugs in components.
        (error) => {
          setIsLoaded(true);
          setError(error);
        }
      )
  }, [])

  if (error) {
    return <div>Error: {error.message}</div>;
  } else if (!isLoaded) {
    return <div>Loading...</div>;
  } else {
    return (
      <ul>
        {items.map(item => (
          <li key={item.id}>
            {item.name} {item.price}
          </li>
        ))}
      </ul>
    );
  }
}
```
