#### EventBus
1. Events are usually cancelled by higher priority subscribers. Cancelling **is restricted** to event handling methods running **in posting thread ( ThreadMode.PostThread).**

```
You may cancel the event delivery process by calling cancelEventDelivery(Object event) from a subscriber’s event handling method. Any further event delivery will be cancelled, subsequent subscribers won’t receive the event.

// Called in the same thread (default)
@Subscribe
public void onEvent(MessageEvent event){
    // Process the event
    ...
    // Prevent delivery to other subscribers
    EventBus.getDefault().cancelEventDelivery(event) ;
}
```
