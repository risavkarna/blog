## Suggestions from Papas and Uncles

Here are some great tips from John Papa and why you should (not) follow them:

## RxJS Subscriptions

![screenshot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594414506336/G85_Apzih.png)

The first one is from a 2019 ng-conf talk where he suggests that one should unsubscribe from all subscriptions reliably once we are done with them, typically in `ngOnDestroy`. 

It would be unfortunate to install his friend's `subsink` library just so you can add all subscriptions to it and then unsubscribe from the sink. RxJS `Subscription` allows you to do this natively. Here's a code snippet from the  [Firebase RxJS guide](https://rxjs-dev.firebaseapp.com/guide/subscription) :

```JavaScript
import { interval } from 'rxjs';

const observable1 = interval(400);
const observable2 = interval(300);

const subscription = observable1.subscribe(x => console.log('first: ' + x));
const childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  // Unsubscribes BOTH subscription and childSubscription
  subscription.unsubscribe();
}, 1000);
```

You can also abstract away this repeated `unsubscribe` in `ngOnDestroy` business as mentioned by Wojciech Trawiński [here](https://medium.com/javascript-everyday/yet-another-way-to-handle-rxjs-subscriptions-1f15554ce3b5):

```TypeScript

import { OnDestroy } from "@angular/core";
import { Subscription, Observable, PartialObserver } from "rxjs";

export abstract class SelfUnsubable implements OnDestroy {
  private subSink = new Subscription();

  ngOnDestroy() {
    this.subSink.unsubscribe();
  }

  protected subscribe<T>(
    observable: Observable<T>,
    observerOrNext?: PartialObserver<T> | ((value: T) => void),
    error?: (error: any) => void,
    complete?: () => void
  ): Subscription {
    return this.subSink.add(
      observable.subscribe(observerOrNext as any, error, complete)
    );
  }

  protected unsubscribe(innerSub: Subscription) {
    this.subSink.remove(innerSub);
    innerSub.unsubscribe();
  }
}
```
Now you have a less error-prone way of managing subscriptions without any external library and just good ol' OOP.

```TypeScript

import { Component, Input, OnInit } from "@angular/core";
import { interval, Subscription } from "rxjs";

import { SelfUnsubable } from "./selfUnsubable";

@Component({
  selector: "counter",
  template: `
    <div>Count {{ count }}</div>
  `
})
export class CounterComponent extends SelfUnsubable {
  count = 0;
  private manualSub: Subscription;

  ngOnInit() {
    // unsubscribed manually 
    this.manualSub = this.subscribe(interval(500), value => {
      this.count++;
      console.log(value);
    });

    // unsubscribed when component is destroyed
    this.subscribe(interval(1000), value => {
      this.count++;
      console.log(value);
    });

   setTimeout(this.unSubManually, 600);
  }

  unSubManually() {
    this.unsubscribe(this.manualSub);
  }
}
```

## NgRX Data

![screenshot (2).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594420522006/mhGVX3hBt.png)

As NgRX devs are cautious to point out, you do not usually need NgRX. By this they usually mean that your app probably does not have use cases to justify the hundreds of lines of code bloat that NgRX requires for each of your state models.

A neat boilerplate code reduction can be achieved by `@ngrx/data`. If you are already using NgRX, it goes without saying that you should at least consider using this library to significantly reduce your store related code's size. However, if you are not already using NgRX, should you use it now that `@ngrx/data` is there to simplify things for you?

I personally favor using a full-fledged client-side immutable database instead of some immutable 'store'. This has become easier than ever since PouchDB showed up and brought sanity to in-browser database-like solutions. Do you really need all the indirections, actions, effects, reducers, dispatchers and selectors for each of your models? Most, if not all, of the time, you just need a basic reading of pure functional programming practices and an actual database with immutability or append-only behavior. Also note that actions, effects, reducers, dispatchers and selectors can be thought of as general coding patterns instead of NgRX exclusive ideas. 

So, is NgRX completely avoidable for even the most complex state management use cases? Your mileage may vary with Couch-Pouch-like solution for state management. It even allows for syncing the current state to a remote server possibly running async analytics, e.g. on user behavior, with very few additional lines of code. There is a lot more that you usually need to do with your application state than just put it in a 'store' manager. Keep it all simple by just using a database and immutable data structures.

You can use PouchDB even with databases with topic-wise event hooks like the Realtime and Firestore databases from Firebase or with web APIs with webhooks or push / pub-sub semantics. In any case, complex UI states can be easily stored and managed in client side in-memory stores or browser-based databases with or without state management solutions -even with boilerplate reducing solutions like NgRX Data, NgXS or Akita. 

One particularly remarkable use case of NgRX is the possibility to create truly reactive updates to Angular without using zone.js. Here is an excellent talk from Mike Ryan regarding this exciting possibility.

%[https://youtu.be/rz-rcaGXhGk]

