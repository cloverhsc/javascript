# Airbnb JavaScript Style Guide() {

*一份彙整了在 JavasScript 中被普遍使用的風格指南。*

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

其他風格指南
 - [ES5](es5/)
 - [React](react/)
 - [CSS & Sass](https://github.com/airbnb/css)
 - [Ruby](https://github.com/airbnb/ruby)

翻譯自 [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) 。

<a name="table-of-contents"></a>
## 目錄

  1. [資料型態](#types)
  1. [參考](#references)
  1. [物件](#objects)
  1. [陣列](#arrays)
  1. [解構子](#destructuring)
  1. [字串](#strings)
  1. [函式](#functions)
  1. [箭頭函式](#arrow-functions)
  1. [建構子](#constructors)
  1. [模組](#modules)
  1. [迭代器及產生器](#iterators-and-generators)
  1. [屬性](#properties)
  1. [變數](#variables)
  1. [提升](#hoisting)
  1. [條件式與等號](#comparison-operators--equality)
  1. [區塊](#blocks)
  1. [控制陳述式](#control-statements)
  1. [註解](#comments)
  1. [空格](#whitespace)
  1. [逗號](#commas)
  1. [分號](#semicolons)
  1. [型別轉換](#type-casting--coercion)
  1. [命名規則](#naming-conventions)
  1. [存取器](#accessors)
  1. [事件](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 相容性](#ecmascript-5-compatibility)
  1. [ECMAScript 6 風格](#ecmascript-6-styles)
  1. [標準程式庫](#standard-library)
  1. [測試](#testing)
  1. [效能](#performance)
  1. [資源](#resources)
  1. [誰在使用](#in-the-wild)
  1. [翻譯](#translation)
  1. [JavaScript 風格指南](#the-javascript-style-guide-guide)
  1. [和我們討論 Javascript](#chat-with-us-about-javascript)
  1. [貢獻者](#contributors)
  1. [授權許可](#license)
  1. [Amendments](#amendments)

<a name="types"></a>
## 資料型態

  - [1.1](#1.1) <a name='1.1'></a> **基本**：你可以直接存取基本資料型態。

    + `字串`
    + `數字`
    + `布林`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **複合**：你需要透過引用的方式存取複合資料型態。

    + `物件`
    + `陣列`
    + `函式`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="references"></a>
## 參考

  - [2.1](#2.1) <a name='2.1'></a> 對於所有的參考使用 `const`；避免使用 `var`。eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

    > 為什麼？因為這能確保你無法對參考重新賦值，也不會讓你的程式碼有錯誤或難以理解。

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> 如果你需要可變動的參考，使用 `let` 代替 `var`。eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > 為什麼？因為 `let` 的作用域是在區塊內，而不像 `var` 是在函式內。

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> 請注意，`let` 與 `const` 的作用域都只在區塊內。

    ```javascript
    // const 及 let 只存在於他們被定義的區塊內。
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="objects"></a>
## 物件

  - [3.1](#3.1) <a name='3.1'></a> 使用簡潔的語法建立物件。eslint rules: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html).

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> 別使用[保留字](http://es5.github.io/#x7.6.1)當作鍵值，他在 IE8 上不會被執行。[了解更多](https://github.com/airbnb/javascript/issues/61)。不過在 ES6 模組及伺服器端程式碼中使用是可行的。jscs: [`disallowIdentifierNames`](http://jscs.info/rule/disallowIdentifierNames)

    ```javascript
    // bad
    const superman = {
      default: { clark: 'kent' },
      private: true,
    };

    // good
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> 使用同義詞取代保留字。jscs: [`disallowIdentifierNames`](http://jscs.info/rule/disallowIdentifierNames)

    ```javascript
    // bad
    const superman = {
      class: 'alien',
    };

    // bad
    const superman = {
      klass: 'alien',
    };

    // good
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> 建立具有動態屬性名稱的物件時請使用可被計算的屬性名稱。

    > 為什麼？因為這樣能夠讓你在同一個地方定義所有的物件屬性。

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> 使用物件方法的簡寫。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> 使用屬性值的簡寫。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    > 為什麼？因為寫起來更短且更有描述性。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> 請在物件宣告的開頭將簡寫的屬性分組。

    > 為什麼？因為這樣能夠很簡單的看出哪些屬性是使用簡寫。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  - [3.8](#3.8) <a name="3.8"></a> 只在無效的鍵加上引號。eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

    > 為什麼？整體來說，我們認為這在主觀上更容易閱讀。它會改善語法高亮，也能讓多數的 JS 引擎更容易最佳化。

    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="arrays"></a>
## 陣列

  - [4.1](#4.1) <a name='4.1'></a> 使用簡潔的語法建立陣列。eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> 如果你不知道陣列的長度請使用 Array#push。

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> 使用陣列的擴展運算子 `...` 來複製陣列。

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> 如果要轉換一個像陣列的物件至陣列，可以使用 Array#from。

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

  - [4.5](#4.5) <a name='4.5'></a> 在陣列方法的回呼使用 return 宣告。若函式本體是如 [8.2](#8.2) 的單一語法，那麼省略 return 是可以的。eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = memo.concat(item);
    });

    // good
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="destructuring"></a>
## 解構子

  - [5.1](#5.1) <a name='5.1'></a> 存取或使用多屬性的物件時，請使用物件解構子。jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

    > 為什麼？因為解構子能夠節省你對這些屬性建立暫時的參考。

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> 使用陣列解構子。jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> 需要回傳多個值時請使用物件解構子，而不是陣列解構子。

    > 為什麼？因為你可以增加新的屬性或改變排序且不須更動呼叫的位置。

    ```javascript
    // bad
    function processInput(input) {
      // 這時神奇的事情出現了
      return [left, right, top, bottom];
    }

    // 呼叫時必須考慮回傳資料的順序。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // 這時神奇的事情出現了
      return { left, right, top, bottom };
    }

    // 呼叫時只需選擇需要的資料
    const { left, right } = processInput(input);
    ```


**[⬆ 回到頂端](#table-of-contents)**

<a name="strings"></a>
## 字串

  - [6.1](#6.1) <a name='6.1'></a> 字串請使用單引號 `''`。eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> 如果字串超過 100 個字元，請使用字串連接符號換行。
  - [6.3](#6.3) <a name='6.3'></a> 注意：過度的長字串連接可能會影響效能。[jsPerf](http://jsperf.com/ya-string-concat) 及[討論串](https://github.com/airbnb/javascript/issues/40)。

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> 當以程式方式建構字串時，請使用模板字串而不是字串連接。eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

    > 為什麼？因為模板字串更有可讀性，正確的換行符號及字串插值功能讓語法更簡潔。

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```
  - [6.5](#6.5) <a name='6.5'></a> 千萬不要在字串中使用 `eval()`，會造成許多的漏洞。

**[⬆ 回到頂端](#table-of-contents)**

<a name="functions"></a>
## 函式

  - [7.1](#7.1) <a name='7.1'></a> 使用函式宣告而不是函式表達式。jscs: [`requireFunctionDeclarations`](http://jscs.info/rule/requireFunctionDeclarations)

    > 為什麼？因為函式宣告是可命名的，所以他們在呼叫堆疊中更容易被識別。此外，函式宣告自身都會被提升，而函式表達式則只有參考會被提升。這個規則使得[箭頭函式](#arrow-functions)可以完全取代函式表達式。

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> 立即函式：eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

    > 為什麼？一個立即函式是個獨立的單元－將函式及呼叫函式的括號包起來明確表示這一點。注意在模組世界的任何地方，你都不需要使用立即函式。

    ```javascript
    // 立即函式（IIFE）
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  - [7.3](#7.3) <a name='7.3'></a> 絕對不要在非函式的區塊（if、while 等等）宣告函式。你可以將函式賦予至變數解決這個問題。瀏覽器會允許你這麼做，但不同瀏覽器產生的結果可能會不同。eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

  - [7.4](#7.4) <a name='7.4'></a> **注意：**ECMA-262 將`區塊`定義為陳述式。函式宣告則不是陳述式。[閱讀 ECMA-262 關於這個問題的說明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> 請勿將參數命名為 `arguments`，這樣會將覆蓋掉函式作用域傳來的 `arguments`。

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> 絕對不要使用 `arguments`，可以選擇使用 rest 語法 `...` 替代。[`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

    > 為什麼？使用 `...` 能夠明確指出你要將參數傳入哪個變數。再加上 rest 參數是一個真正的陣列，而不像 `arguments` 似陣列而非陣列。

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> 使用預設參數的語法，而不是變動函式的參數。

    ```javascript
    // really bad
    function handleThings(opts) {
      // 不！我們不該變動函式的參數。
      // Double bad: 如果 opt 是 false ，那們它就會被設定為一個物件，
      // 或許你想要這麼做，但是這樣可能會造成一些 Bug。
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> 使用預設參數時請避免副作用。

    > 為什麼？因為這樣會讓思緒混淆。

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  - [7.9](#7.9) <a name='7.9'></a> 永遠將預設參數放置於最後。

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  - [7.10](#7.10) <a name='7.10'></a> 千萬別使用建構函式去建立一個新的函式。

    > 為什麼？透過這種方式建立一個函數來計算字串類似於 eval()，會造成許多的漏洞。

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  - [7.11](#7.11) <a name="7.11"></a> 在函式的標示後放置空格。

    > 為什麼？一致性較好，而且你不應該在新增或刪除名稱時增加或減少空格。

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  - [7.12](#7.12) <a name="7.12"></a> 切勿變更參數。eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > 為什麼？操作作為參數傳入的物件可能導致變數產生原呼叫者不期望的副作用。

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    };

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    };
    ```

  - [7.13](#7.13) <a name="7.13"></a> 切勿重新賦值給參數。eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > 為什麼？將參數重新賦值可能導致意外的行為，尤其在存取 `arguments` 物件時。它可能會引起最佳化的問題，尤其在 V8。

      ```javascript
      // bad
      function f1(a) {
        a = 1;
      }

      function f2(a) {
        if (!a) { a = 1; }
      }

      // good
      function f3(a) {
        const b = a || 1;
      }

      function f4(a = 1) {
      }
      ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="arrow-functions"></a>
## 箭頭函式

  - [8.1](#8.1) <a name='8.1'></a> 當你必須使用函式表達式（或傳遞一個匿名函式）時，請使用箭頭函式的符號。eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

    > 為什麼？它會在有 `this` 的內部建立了一個新版本的函式，通常功能都是你所想像的，而且語法更為簡潔。

    > 為什麼不？如果你已經有一個相當複雜的函式時，或許你該將邏輯都移到一個函式宣告上。

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> 如果函式適合只使用一行，你可以很隨性的省略大括號及使用隱藏的回傳。否則請使用 `return` 語法。eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

    > 為什麼？因為語法修飾。這樣能夠在多個函式鏈結在一起的時候更易讀。

    > 為什麼不？如果你打算回傳一個物件。

    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });
    ```

  - [8.3](#8.3) <a name='8.3'></a> 如果表達式跨了多行，請將它們包在括號中增加可讀性。

    > 為什麼？這麼做更清楚的表達函式的開始與結束的位置。

    ```js
    // bad
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
      `${number} became much longer. So we needed to break it over multiple ` +
      'lines.'
    );

    // good
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```


  - [8.4](#8.4) <a name='8.4'></a> 如果你的函式只使用一個參數，那麼可以很隨意的省略括號。否則請在參數兩側加上括號。eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

    > 為什麼？減少視覺上的混亂。

    ```js
    // bad
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we’ve broken it ` +
      'over multiple lines!'
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [8.5](#8.5) <a name='8.5'></a> 避免混淆箭頭函式語法（`=>`）及比較運算子（`<=`、`>=`）。eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

    ```js
    // bad
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = item => { return item.height > 256 ? item.largeSize : item.smallSize; }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="constructors"></a>
## 建構子

  - [9.1](#9.1) <a name='9.1'></a> 總是使用 `class`。避免直接操作 `prototype`。

    > 為什麼？因為 `class` 語法更簡潔且更易讀。

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }


    // good
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }
      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
    ```

  - [9.2](#9.2) <a name='9.2'></a> 使用 `extends` 繼承。

    > 為什麼？因為他是一個內建繼承原型方法的方式，且不會破壞 `instanceof`。

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> 方法可以回傳 `this` 幫助方法鏈結。

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - [9.4](#9.4) <a name='9.4'></a> 可以寫一個 toString() 的方法，但是請確保它可以正常執行且沒有函式副作用。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  - [9.5](#9.5) <a name='9.5'></a> 若類別沒有指定建構子，那它會擁有預設的建構子。一個空的建構子函式或只委派給父類別是不必要的。[`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**


<a name="modules"></a>
## 模組

  - [10.1](#10.1) <a name='10.1'></a> 總是使用模組（`import`/`export`）勝過一個非標準模組的系統。你可以編譯為喜歡的模組系統。

    > 為什麼？模組就是未來的趨勢，讓我們現在就開始前往未來吧。

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> 請別使用萬用字元引入。

    > 為什麼？這樣能夠確保你只有一個預設導出。

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a> 然後也不要在引入的地方導出。

    > 為什麼？雖然一行程式相當的簡明，但是讓引入及導出各自有明確的方式能夠讓事情保持一致。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="iterators-and-generators"></a>
## 迭代器及產生器

  - [11.1](#11.1) <a name='11.1'></a> 不要使用迭代器。更好的做法是使用 JavaScript 的高階函式，像是 `map()` 及 `reduce()`，替代如 `for-of ` 的迴圈語法。eslint: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html)

    > 為什麼？這加強了我們不變的規則。處理純函式的回傳值讓程式碼更易讀，勝過它所造成的函式副作用。

    eslint rules: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html).

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach(num => sum += num);
    sum === 15;

    // best (使用 javascript 的高階函式)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> 現在還不要使用產生器。

    > 為什麼？因為它現在編譯至 ES5 還沒有編譯得非常好。

**[⬆ 回到頂端](#table-of-contents)**

<a name="properties"></a>
## 屬性

  - [12.1](#12.1) <a name='12.1'></a> 使用點 `.` 來存取屬性。eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> 需要帶參數存取屬性時請使用中括號 `[]`。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="variables"></a>
## 變數

  - [13.1](#13.1) <a name='13.1'></a> 為了避免污染全域的命名空間，請使用 `const` 來宣告變數，如果不這麼做將會產生全域變數。Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> 每個變數只使用一個 `const` 來宣告。eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

    > 為什麼？因為這樣更容易增加新的變數宣告，而且你也不用擔心替換  `;` 為 `,` 及加入的標點符號不同的問題。

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // （比較上述例子找出錯誤）
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> 將所有的 `const` 及 `let` 分組。

    > 為什麼？當你需要根據之前已賦值的變數來賦值給未賦值變數時相當有幫助。

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> 在你需要的地方賦值給變數，但是請把它們放在合理的位置。

    > 為什麼？因為 `let` 及 `const` 是在區塊作用域內，而不是函式作用域。

    ```javascript
    // bad - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // good
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="hoisting"></a>
## 提升

  - [14.1](#14.1) <a name='14.1'></a> `var` 宣告可以被提升至該作用域的最頂層，但賦予的值並不會。`const` 及 `let` 的宣告被賦予了新的概念，稱為[暫時性死區（Temporal Dead Zones, TDZ）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)。這對於瞭解為什麼 [typeof 不再那麼安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)是相當重要的。

    ```javascript
    // 我們知道這樣是行不通的
    // （假設沒有名為 notDefined 的全域變數）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 由於變數提升的關係，
    // 你在引用變數後再宣告變數是行得通的。
    // 注：賦予給變數的 `true` 並不會被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 直譯器會將宣告的變數提升至作用域的最頂層，
    // 表示我們可以將這個例子改寫成以下：
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // 使用 const 及 let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> 賦予匿名函式的變數會被提升，但函式內容並不會。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> 賦予命名函式的變數會被提升，但函式內容及函式名稱並不會。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 當函式名稱和變數名稱相同時也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> 宣告函式的名稱及函式內容都會被提升。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 想瞭解更多訊息，請參考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/)。

**[⬆ 回到頂端](#table-of-contents)**

<a name="comparison-operators--equality"></a>
## 條件式與等號

## Comparison Operators & Equality

  - [15.1](#15.1) <a name='15.1'></a> 請使用 `===` 和 `!==` ，別使用 `==` 及 `!=` 。eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)

  - [15.2](#15.2) <a name='15.2'></a> 像是 `if` 的條件語法內會使用 `ToBoolean` 的抽象方法強轉類型，並遵循以下規範：

    + **物件** 轉換為 **true**
    + **Undefined** 轉換為 **false**
    + **Null** 轉換為 **false**
    + **布林** 轉換為 **該布林值**
    + **數字** 如果是 **+0, -0, 或 NaN** 則轉換為 **false**，其他的皆為 **true**
    + **字串** 如果是空字串 `''` 則轉換為 **false**，其他的皆為 **true**

    ```javascript
    if ([0] && []) {
      // true
      // 陣列（即使為空）為一個物件，所以轉換為 true
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> 使用簡短的方式。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - [15.4](#15.4) <a name='15.4'></a> 想瞭解更多訊息請參考 Angus Croll 的 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)。
  - [15.5](#15.5) <a name='15.5'></a> Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`).
  - [15.6](#15.6) <a name='15.6'></a> 若 `case` 與 `default` 包含了宣告語法（例如：`let`、`const`、`function` 及 `class`）時使用大括號來建立區塊。

    > Why? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.
    > 為什麼？宣告語法可以在整個 `switch` 區塊中可見，但是只在進入該 `case` 時初始化。當多個 `case` 語法時會導致嘗試定義相同事情的問題。

    eslint rules: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {}
        break;
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {}
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  - [15.7](#15.7) <a name='15.7'></a> 不應該使用巢狀的三元運算子，且通常應該使用單行來表示。

    eslint rules: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // better
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  - [15.8](#15.8) <a name='15.8'></a> 避免不必要的三元運算子語法。

    eslint rules: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="blocks"></a>
## 區塊

  - [16.1](#16.1) <a name='16.1'></a> 多行區塊請使用大括號刮起來。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  - [16.2](#16.2) <a name='16.2'></a> 如果你使用 `if` 及 `else` 的多行區塊，請將 `else` 放在 `if` 區塊的結尾大括號後。eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="control-statements"></a>
## 控制陳述式

  - [17.1](#17.1) <a name='17.1'></a> 為避免控制陳述式（`if`、`while` 等）太長或超過該行字數限制，每組條件式可自成一行。邏輯運算子應置於行首。

    > 為什麼？行首的運算子可維持版面整齊，遵守和方法鏈類似的排版模式。還能提供視覺線索，讓複雜的邏輯述句更容易閱讀。

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  - [17.2](#17.2)  <a name='17.2'></a> 不要用選擇運算子（selection operators）來取代控制陳述式。

    ```javascript
    // bad
    !isRunning && startRunning();

    // good
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**
 
<a name="comments"></a>
## 註解

  - [18.1](#18.1) <a name='18.1'></a> 多行註解請使用 `/** ... */` ，包含描述，指定類型以及參數值還有回傳值。

    ```javascript
    // bad
    // make() 根據傳入的 tag 名稱回傳一個新的元件
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() 根據傳入的 tag 名稱回傳一個新的元件
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - [18.2](#18.2) <a name='18.2'></a> 單行註解請使用 `//`。在欲註解的上方新增一行進行註解。在註解的上方空一行，除非他在區塊的第一行。

    ```javascript
    // bad
    const active = true;  // 當目前分頁

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // 設定預設的類型為 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // 設定預設的類型為 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [18.3](#18.3) <a name='18.3'></a> 在註解前方加上 `FIXME` 或 `TODO` 可以幫助其他開發人員快速瞭解這是一個需要重新討論的問題，或是一個等待解決的問題。和一般的註解不同，他們是可被執行的。對應的動作為 `FIXME -- 重新討論並解決` 或 `TODO -- 必須執行`。

  - [18.4](#18.4) <a name='18.4'></a> 使用 `// FIXME:` 標注問題。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: 不該在這使用全域變數
        total = 0;
      }
    }
    ```

  - [18.5](#18.5) <a name='18.5'></a> 使用 `// TODO:` 標注問題的解決方式。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total 應該可被傳入的參數所修改
        this.total = 0;
      }
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="whitespace"></a>
## 空格

  - [19.1](#19.1) <a name='19.1'></a> 將 Tab 設定為兩個空格。eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙const name;
    }

    // bad
    function bar() {
    ∙const name;
    }

    // good
    function baz() {
    ∙∙const name;
    }
    ```

  - [19.2](#19.2) <a name='19.2'></a> 在大括號前加一個空格。eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [19.3](#19.3) <a name='19.3'></a> 在控制流程的語句（`if`, `while` 等等。）的左括號前加上一個空格。宣告的函式和傳入的變數間則沒有空格。eslint: [`space-after-keywords`](http://eslint.org/docs/rules/space-after-keywords.html), [`space-before-keywords`](http://eslint.org/docs/rules/space-before-keywords.html) jscs:  [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - [19.4](#19.4) <a name='19.4'></a> 將運算元用空格隔開。eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - [19.5](#19.5) <a name='19.5'></a> 在檔案的最尾端加上一行空白行。

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  - [19.6](#19.6) <a name='19.6'></a> 當多個方法鏈結（大於兩個方法鏈結）時請換行縮排。利用前面的 `.` 強調該行是呼叫方法，而不是一個新的宣告。eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    ```

  - [19.7](#19.7) <a name='19.7'></a> 在區塊的結束及下個語法間加上空行。jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  - [19.8](#19.8) <a name='19.8'></a> 別在區塊中置放空行。eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  - [19.9](#19.9) <a name='19.9'></a> 不要在括號內的兩側置放空格。eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  - [19.10](#19.10) <a name='19.10'></a> 不要在中括號內的兩側置放空格。eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  - [19.11](#19.11) <a name='19.11'></a> 在大括號內的兩側置放空格。eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`disallowSpacesInsideObjectBrackets`](http://jscs.info/rule/

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  - [19.12](#19.12) <a name='19.12'></a> 避免一行的程式碼超過 100 字元（包含空白）。eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

    > 為什麼？這樣確保可讀性及維護性。

    ```javascript
    // bad
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. ' +
      'Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="commas"></a>
## 逗號

  - [20.1](#20.1) <a name='20.1'></a> 不要將逗號放在前方。eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [20.2](#20.2) <a name='20.2'></a> 增加結尾的逗號：**別懷疑**eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)

    > 為什麼？這會讓 Git 的差異列表更乾淨。另外，Babel 轉譯器也會刪除結尾多餘的逗號，也就是說你完全不需要擔心在老舊的瀏覽器發生[多餘逗號的問題](es5/README.md#commas)。

    ```javascript
    // bad - 不含多餘逗號的 git 差異列表
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    };

    // good - 包含多餘逗號的 git 差異列表
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="semicolons"></a>
## 分號

  - [21.1](#21.1) <a name='21.1'></a> **對啦。**eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)

    ```javascript
    // bad
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    }());

    // good（防止當兩個檔案含有立即函式需要合併時，函式被當成參數處理）
    ;(() => {
      const name = 'Skywalker';
      return name;
    }());
    ```

    [瞭解更多](http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214)。

**[⬆ 回到頂端](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 型別轉換

  - [22.1](#22.1) <a name='22.1'></a> 在開頭的宣告進行強制型別轉換。
  - [22.2](#22.2) <a name='22.2'></a> 字串：

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [22.3](#22.3) <a name='22.3'></a> 數字：使用 `Number` 做型別轉換，而 `parseInt` 則始終以基數解析字串。eslint: [`radix`](http://eslint.org/docs/rules/radix)

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - [22.4](#22.4) <a name='22.4'></a> 如果你因為某個原因在做些瘋狂的事情，但是 `parseInt` 是你的瓶頸，所以你對於[性能方面的原因](http://jsperf.com/coercion-vs-casting/3)而必須使用位元右移，請留下評論並解釋為什麼使用，及你做了哪些事情。

    ```javascript
    // good
    /**
     * 使用 parseInt 導致我的程式變慢，改成使用
     * 位元右移強制將字串轉為數字加快了他的速度。
     */
    const val = inputValue >> 0;
    ```

  - [22.5](#22.5) <a name='22.5'></a> **注意：**使用位元轉換時請小心。數字為 [64 位元數值](http://es5.github.io/#x4.3.19)，但是使用位元轉換時則會回傳一個 32 位元的整數（[來源](http://es5.github.io/#x11.7)），這會導致大於 32 位元的數值產生異常 [討論串](https://github.com/airbnb/javascript/issues/109)，32 位元的整數最大值為 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [22.6](#22.6) <a name='22.6'></a> 布林：

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="naming-conventions"></a>
## 命名規則

  - [23.1](#23.1) <a name='23.1'></a> 避免使用單一字母的名稱，讓你的名稱有解釋的含義。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - [23.2](#23.2) <a name='23.2'></a> 使用駝峰式大小寫命名物件，函式及實例。eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - [23.3](#23.3) <a name='23.3'></a> 使用帕斯卡命名法來命名建構子或類別。eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - [23.4](#23.4) <a name='23.4'></a> 命名私有屬性時請在前面加底線 `_`。eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [23.5](#23.5) <a name='23.5'></a> 請別儲存 `this` 為參考。請使用箭頭函式或是 Function#bind。jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - [23.6](#23.6) <a name='23.6'></a> 如果你的檔案只有輸出一個類別，你的檔案名稱必須和你的類別名稱相同。

    ```javascript
    // 檔案內容
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // 在其他的檔案
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - [23.7](#23.7) <a name='23.7'></a> 當你導出為預設的函式時請使用駝峰式大小寫。檔案名稱必須與你的函式名稱一致。

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [23.8](#23.8) <a name='23.8'></a> 當你導出為單例 / 函式庫 / 空物件時請使用帕斯卡命名法。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ 回到頂端](#table-of-contents)**

<a name="accessors"></a>
## 存取器

  - [24.1](#24.1) <a name='24.1'></a> 屬性的存取器函式不是必須的。
  - [24.2](#24.2) <a name='24.2'></a> 別使用 JavaScript 的 getters 或 setters，因為它們會導致意想不到的副作用，而且不易於測試、維護以及進行推測。取而代之，如果你要建立一個存取器函式，請使用 getVal() 及 setVal('hello')。

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - [24.3](#24.3) <a name='24.3'></a> 如果屬性是布林，請使用 `isVal()` 或 `hasVal()`。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [24.4](#24.4) <a name='24.4'></a> 可以建立 get() 及 set() 函式，但請保持一致。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="events"></a>
## 事件

  - [25.1](#25.1) <a name='25.1'></a> 當需要對事件傳入資料時（不論是 DOM 事件或是其他私有事件），請傳入物件替代單一的資料。這樣可以使之後的開發人員直接加入其他的資料到事件裡，而不需更新該事件的處理器。例如，比較不好的做法：

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', (e, listingId) => {
      // do something with listingId
    });
    ```

    更好的做法：

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingId
    });
    ```

  **[⬆ 回到頂端](#table-of-contents)**


## jQuery

  - [26.1](#26.1) <a name='26.1'></a> jQuery 的物件請使用 `$` 當前綴。jscs: [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  - [26.2](#26.2) <a name='26.2'></a> 快取 jQuery 的查詢。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [26.3](#26.3) <a name='26.3'></a> DOM 的查詢請使用層遞的 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。[jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [26.4](#26.4) <a name='26.4'></a> 對作用域內的 jQuery 物件使用 `find` 做查詢。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="ecmascript-5-compatibility"></a>
## ECMAScript 5 相容性

  - [27.1](#27.1) <a name='27.1'></a> 參考 [Kangax](https://twitter.com/kangax/) 的 ES5 [相容性列表](http://kangax.github.io/es5-compat-table/)。

**[⬆ 回到頂端](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6 風格

  - [28.1](#28.1) <a name='28.1'></a> 以下是連結到各個 ES6 特性的列表。

1. [箭頭函式](#arrow-functions)
1. [類別](#constructors)
1. [物件簡寫](#es6-object-shorthand)
1. [簡潔物件](#es6-object-concise)
1. [可計算的物件屬性](#es6-computed-properties)
1. [模板字串](#es6-template-literals)
1. [解構子](#destructuring)
1. [預設參數](#es6-default-parameters)
1. [剩餘參數（Rest）](#es6-rest)
1. [陣列擴展](#es6-array-spreads)
1. [Let 及 Const](#references)
1. [迭代器及產生器](#iterators-and-generators)
1. [模組](#modules)

**[⬆ 回到頂端](#table-of-contents)**

<a name="standard-library"></a>
## 標準程式庫

[標準程式庫（Standard Library）](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)基於歷史因素，仍保有某些功能有缺陷的函式。

  - [29.1](#29.1) <a name='29.1'></a> 使用 `Number.isNaN` 而非 `isNaN`。

    > 為什麼？全域函式 `isNaN` 會先將任何非數值轉換為數值，如果轉換後之值為 NaN，則函式回傳 true。
    > 若真要轉換為數值，請表達清楚。

    ```javascript
    // bad
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // good
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  - [29.2](#29.2) <a name='29.2'></a> 使用 `Number.isFinite` 而非 `isFinite`。

    > 為什麼？全域函式 `isFinite` 會先將任何非數值轉換為數值，如果轉換後之值有限，則函式回傳 true。
    > 若真要轉換為數值，請表達清楚。

    ```javascript
    // bad
    isFinite('2e3'); // true

    // good
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ 回到頂端](#table-of-contents)**

<a name="testing"></a>
## 測試

  - [30.1](#30.1) <a name='30.1'></a> **如題。**

    ```javascript
    function foo() {
      return true;
    }
    ```

  - [30.2](#30.2) <a name="30.2"></a> **無題，不過很重要**：
    - 不論你用哪個測試框架，你都應該撰寫測試！
    - 力求撰寫許多的純函式，並盡量減少異常發生的機會。
    - 要對 stubs 及 mocks 保持嚴謹——他們可以讓你的測試變得更加脆弱。
    - 我們在 Airbnb 主要使用 [`mocha`](https://www.npmjs.com/package/mocha)。對小型或單獨的模組偶爾使用 [`tape`](https://www.npmjs.com/package/tape)。
    - 努力達到 100% 的測試涵蓋率是個很好的目標，即使實現這件事是不切實際的。
    - 每當你修復完一個 bug，_就撰寫回歸測試_。一個修復完的 bug 若沒有回歸測試，通常在未來肯定會再次發生損壞。

**[⬆ 回到頂端](#table-of-contents)**

<a name="performance"></a>
## 效能

  - [On Layout & Web Performance](http://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ 回到頂端](#table-of-contents)**

<a name="resources"></a>
## 資源

**學習 ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**請讀這個**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**工具**

  - Code Style Linters
    + [ESlint](http://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    + [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**其他的風格指南**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

**其他風格**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**瞭解更多**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**書籍**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**部落格**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](https://devchat.tv/js-jabber/)


**[⬆ 回到頂端](#table-of-contents)**

<a name="in-the-wild"></a>
## 誰在使用

  這是正在使用這份風格指南的組織列表。送一個 pull request 後我們會將你增加到列表上。

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Avant**: [avantcredit/javascript](https://github.com/avantcredit/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk/javascript](https://github.com/Bisk/javascript/)
  - **Blendle**: [blendle/javascript](https://github.com/blendle/javascript)
  - **Brainshark**: [brainshark/javascript](https://github.com/brainshark/javascript)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **Huballin**: [huballin/javascript](https://github.com/huballin/javascript)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JeopardyBot**: [kesne/jeopardy-bot](https://github.com/kesne/jeopardy-bot/blob/master/STYLEGUIDE.md)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **OutBoxSoft**: [OutBoxSoft/javascript](https://github.com/OutBoxSoft/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **React**: [/facebook/react/blob/master/CONTRIBUTING.md#style-guide](https://github.com/facebook/react/blob/master/CONTRIBUTING.md#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/rei/code-style-guides/blob/master/docs/javascript.md)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Springload**: [springload/javascript](https://github.com/springload/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **WeBox Studio**: [weboxstudio/javascript](https://github.com/weboxstudio/javascript)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ 回到頂端](#table-of-contents)**

<a name="translation"></a>
## 翻譯

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

<a name="the-javascript-style-guide-guide"></a>
## JavaScript 風格指南

  - [參考](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

<a name="chat-with-us-about-javascript"></a>
## 與我們討論 JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

<a name="contributors"></a>
## 貢獻者

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014-2016 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 回到頂端](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
