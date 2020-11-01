---
id: hooks-intro
title: હૂક્સ નો પરિચય
permalink: docs/hooks-intro.html
next: hooks-overview.html
---

*હૂક્સ* React 16.8 નો એક નવો ઉમેરો છે.તે તમને class નો ઉપયોગ કર્યા વગર state અને React ની અન્ય લાક્ષણિકતા વાપરવા દે છે.

```js{4,5}
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

સૌ પ્રથમ આપણે `useState`  "હૂક" વિષે શીખીશું,પરંતુ આ ઉદાહરણ માત્ર એક નાની જલક હતી.ચિંતા ના કરશો જો તે હજુ સમજાયું ના હોય તો!

**તમે  [આવતા પેજ](/docs/hooks-overview.html) થી હૂક શીખવાનું ચાલુ કરી સકો છો.** આ પેજ પર આપણે સમજવનું ચાલુ રાખીશું કે કેમ આપણે React માં હૂક ઉમેરી રહ્યા છે અને તે કેવી રીતે સારી એપ્લિકેશન બનાવવામાં મદદરૂપ થઈ શકે છે.
>નોધ
>
>React 16.8.0 એ પહેલું વર્ઝન છે જે હૂક્સ ને support કરે છે.Upgrading ના સમયે, બધા પેકેજ ને update કરવાનું ભૂલસો નહીં React DOM પણ.
>React Native [0.59 માં release](https://reactnative.dev/blog/2019/03/12/releasing-react-native-059) થી  હૂક્સ ને  support કરે છે.

## વિડિયો પરિચય  {#video-introduction}

React Conf 2018 માં Sophie Alpert અને Dan Abramov એ સૌ પ્રથમ હૂક્સ નો પરિચય આપ્યો હતો,ત્યારબાદ Ryan Florence એ બતાવ્યુ હતું કે કેવી રીતે તેનો ઉપયો કરી ને એપ્લિકેશન refactor થાય.અહી video જોવો:

<br>

<iframe width="650" height="366" src="//www.youtube.com/embed/dpw9EHDh2bM" frameborder="0" allowfullscreen></iframe>

## No Breaking Changes {#no-breaking-changes}

આગળ વધતાં પહેલા,નોધો કે હૂક્સ:

* **સંપૂર્ણ પસંદગી.** તમે લખેલો code બદલ્યા વગર અમુક components માં હૂક્સ નો ઉપયોગ કરી શકો છો.પરંતુ જો તમે ના ઈછતા હોય તો  તમને હૂક્સ જાણવાની કે અમલ માં મૂકવાની જરૂર નથી.
* **100% backwards-compatible.** હૂક્સ કોઈ મોટા ફેરફાર નથી ધરાવતું.
* **હમણાં ઉપલબ્ધ .** હૂક્સ હવે v16.8.0 થી ઉપલબ્ધ છે.

**React માથી ક્લાસ કાઢવાની કોઈ યોજના નથી.** તમે આ ધીરે ધીરે અપનાવાની હૂક્સ નીતિ વિષે વાંચી સકો છો. [નીચેના ભાગે](#gradual-adoption-strategy).

**હૂક્સ તમારા React ના જ્ઞાન ને બદલે નહીં.** પરંતુ, હૂક્સ તમને સીધી React concepts ની API આપસે જે તમે પેહલા થી જાણે છે : props, state, context, refs, and lifecycle.જે આપણે જોઈશું, હૂક્સ તેને Hooks also offer a new powerful way to combine them.

**જો તમે સીધું હૂક્સ સિખવા માંગો છો,તો તમે [આવતા પેજ પર જઈ સકો છો!](/docs/hooks-overview.html)** તમે એવું પણ વાંચી સકો છો કે કેમ અમે હૂક્સ નો ઉમેરો કરી રહ્યા છે,અને કેવી રીતે આપણે આખી applicaitons ફરી લખ્યા વગર તેનો ઉપયોગ કરી શકીએ છે.

## પ્રેરણા {#motivation}

હુક્સ એવી અસંખ્ય સમસ્યાઓનો હલ કરે છે જે આપણે હજારો components ના code અને જાળવણીના પાંચ વર્ષોથી સામનો કરવો પડ્યો છે.ભલે તમે React સિખી રહ્યા છો,રોજ વાપરો છો કે બીજી અલગ સમાન component મોડલ ધરાવતી library વાપરવાનું પસંદ કરો છો, તમે કદાચ આમાંની કેટલીક સમસ્યાઓ ઓળખો છો.

### It's hard to reuse stateful logic between components {#its-hard-to-reuse-stateful-logic-between-components}

React doesn't offer a way to "attach" reusable behavior to a component (for example, connecting it to a store). If you've worked with React for a while, you may be familiar with patterns like [render props](/docs/render-props.html) and [higher-order components](/docs/higher-order-components.html) that try to solve this. But these patterns require you to restructure your components when you use them, which can be cumbersome and make code harder to follow. If you look at a typical React application in React DevTools, you will likely find a "wrapper hell" of components surrounded by layers of providers, consumers, higher-order components, render props, and other abstractions. While we could [filter them out in DevTools](https://github.com/facebook/react-devtools/pull/503), this points to a deeper underlying problem: React needs a better primitive for sharing stateful logic.

With Hooks, you can extract stateful logic from a component so it can be tested independently and reused. **Hooks allow you to reuse stateful logic without changing your component hierarchy.** This makes it easy to share Hooks among many components or with the community.

We'll discuss this more in [Building Your Own Hooks](/docs/hooks-custom.html).

### Complex components become hard to understand {#complex-components-become-hard-to-understand}

We've often had to maintain components that started out simple but grew into an unmanageable mess of stateful logic and side effects. Each lifecycle method often contains a mix of unrelated logic. For example, components might perform some data fetching in `componentDidMount` and `componentDidUpdate`. However, the same `componentDidMount` method might also contain some unrelated logic that sets up event listeners, with cleanup performed in `componentWillUnmount`. Mutually related code that changes together gets split apart, but completely unrelated code ends up combined in a single method. This makes it too easy to introduce bugs and inconsistencies.

In many cases it's not possible to break these components into smaller ones because the stateful logic is all over the place. It's also difficult to test them. This is one of the reasons many people prefer to combine React with a separate state management library. However, that often introduces too much abstraction, requires you to jump between different files, and makes reusing components more difficult.

To solve this, **Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data)**, rather than forcing a split based on lifecycle methods. You may also opt into managing the component's local state with a reducer to make it more predictable.

We'll discuss this more in [Using the Effect Hook](/docs/hooks-effect.html#tip-use-multiple-effects-to-separate-concerns).

### Classes confuse both people and machines {#classes-confuse-both-people-and-machines}

In addition to making code reuse and code organization more difficult, we've found that classes can be a large barrier to learning React. You have to understand how `this` works in JavaScript, which is very different from how it works in most languages. You have to remember to bind the event handlers. Without unstable [syntax proposals](https://babeljs.io/docs/en/babel-plugin-transform-class-properties/), the code is very verbose. People can understand props, state, and top-down data flow perfectly well but still struggle with classes. The distinction between function and class components in React and when to use each one leads to disagreements even between experienced React developers.

Additionally, React has been out for about five years, and we want to make sure it stays relevant in the next five years. As [Svelte](https://svelte.dev/), [Angular](https://angular.io/), [Glimmer](https://glimmerjs.com/), and others show, [ahead-of-time compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) of components has a lot of future potential. Especially if it's not limited to templates. Recently, we've been experimenting with [component folding](https://github.com/facebook/react/issues/7323) using [Prepack](https://prepack.io/), and we've seen promising early results. However, we found that class components can encourage unintentional patterns that make these optimizations fall back to a slower path. Classes present issues for today's tools, too. For example, classes don't minify very well, and they make hot reloading flaky and unreliable. We want to present an API that makes it more likely for code to stay on the optimizable path.

To solve these problems, **Hooks let you use more of React's features without classes.** Conceptually, React components have always been closer to functions. Hooks embrace functions, but without sacrificing the practical spirit of React. Hooks provide access to imperative escape hatches and don't require you to learn complex functional or reactive programming techniques.

>Examples
>
>[Hooks at a Glance](/docs/hooks-overview.html) is a good place to start learning Hooks.

## Gradual Adoption Strategy {#gradual-adoption-strategy}

>**TLDR: There are no plans to remove classes from React.**

We know that React developers are focused on shipping products and don't have time to look into every new API that's being released. Hooks are very new, and it might be better to wait for more examples and tutorials before considering learning or adopting them.

We also understand that the bar for adding a new primitive to React is extremely high. For curious readers, we have prepared a [detailed RFC](https://github.com/reactjs/rfcs/pull/68) that dives into motivation with more details, and provides extra perspective on the specific design decisions and related prior art.

**Crucially, Hooks work side-by-side with existing code so you can adopt them gradually.** There is no rush to migrate to Hooks. We recommend avoiding any "big rewrites", especially for existing, complex class components. It takes a bit of a mindshift to start "thinking in Hooks". In our experience, it's best to practice using Hooks in new and non-critical components first, and ensure that everybody on your team feels comfortable with them. After you give Hooks a try, please feel free to [send us feedback](https://github.com/facebook/react/issues/new), positive or negative.

We intend for Hooks to cover all existing use cases for classes, but **we will keep supporting class components for the foreseeable future.** At Facebook, we have tens of thousands of components written as classes, and we have absolutely no plans to rewrite them. Instead, we are starting to use Hooks in the new code side by side with classes.

## વારંવાર પૂછાતા પ્રશ્નો {#frequently-asked-questions}

અમે [હૂક્સ FAQ પેજ](/docs/hooks-faq.html) તૈયાર કર્યું છે જે હૂક્સ ને લગતા તમામ પ્રશ્નો નો જવાબ આપે છે.

## આગળનું પગલું {#next-steps}

આ પેજ ના અંત સુધીમાં, તમારે પાસે હુક્સ કઈ સમસ્યાઓ નો હલ કરે છે તેના વિશે રફ વિચાર હોવો જોઈએ, પરંતુ ઘણી વિગતો કદાચ અસ્પષ્ટ રહી હસે.ચિંતા ના કરશો! **ચલો હવે આપણે [આગળ ના પેજ](/docs/hooks-overview.html) પર જઈએ જ્યાં આપણે ઉદાહરણ થી હૂક્સ સિખવાનું ચાલુ કરીશું.**
