# Reducers

In functional programming, a reducer takes your previous state object and an action and returns the updated state.

In KlayOracle, when data sources return responses to the data provider, the data could be returned in different formats: an array, an object, or an array of objects. The reducer is a function that is run on this data, manipulates it, and which in itself returns a response.

In KlayOracle’s data provider feeds, reducers are written as an array of objects, where each defined reducer acts on the response returned by the previous reducer. For example, where we have:

```jsx
"reducers":[
    {
      "function":"PARSE",
      "args":["$.RAW.KLAY.USD.PRICE"]
    },
    {
      "function":"FLOAT64_MUL_UINT64",
      "args":[
        "1000000000"
      ]
    }
  ],
```

The PARSE function is run on the data source’s JSON response, returns a response, and then the FLOAT64\_MUL\_UINT64 is then run on PARSE’s response.

\<aside> 💡 For price feeds, PARSE must be the first reducer run on the JSON response from the data source. Its output must be a single value of type: integer, float, string of float, string of integer

\</aside>

There are 6 defined reducers in KlayOracle’s data providers:

* PARSE:
* FLOAT64\_MUL\_UINT64
* …

\<aside> 💡 It is highly recommended that the final reducer object returns an unsigned integer, since this data is sent to the blockchain via the oracle contract, and the EVM only accepts positive integer values.

\</aside>

\<aside> 💡 The data subscriber should be informed of the data type that will be returned, so they know how to convert the data before passing it to their smart contract.

\</aside>
