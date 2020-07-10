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