# Angular Signal

### Creation

```ts
const count = signal<number>(0);
const user = signal<User>({ id: "eab2", firstname: "Modeste", lastName: "A." });
```

### Read

```ts
count(); // 0
user().firstname + " " + user().lastName; // "Modeste A."
```

### Update

```ts
count.set(1);
// or
count.update(currentVal => currentVal + 1)

user.update(currentInfos => ({...currentInfos, lastName: "Assiongbon"})
```

### How it works

- Update content value => **notify consumer for value change without sending the value**
- Interrested **consumers are in charge to fetch the new value themselves** in order to get the last value.

### Push or Pull

### Observable : Push

The consumer is notified with the last value when a change occured.

### Signal: Hybrid (Push + Pull)

- **Push based** for change notification
- **Pull based** for value fetching

### Computed Signal (Kind of selector)

Signal which value is computed based on other_s signal_s (dependent_s signal_s) value.

```ts
const user = signal<User>({ id: "eab2", firstname: "Modeste", lastName: "A." });
const userFullName = computed(() => `${user().firstname} ${user().lastName}`);

userFullName(); // "Modeste A."

user.update((currentInfos) => ({ ...currentInfos, lastName: "Assiongbon" }));

userFullName(); // "Modeste Assiongbon"
```

**Note :**

- Value is memoized to avoid re-computation each time we try to get the content.
- Value is computed only when we pulled the content explicitly
- **computed signals** run their callback on start and when their dependencies change.

### Effect : Do something (effect) when dependent_s signal_s value changed

- Usage: logging, make API call, etc.

- Must be set in injection context (constructor time) beacause base on DestroyRef (use for auto-clean)
- **effects** run their callback on start and when their dependencies change.

**Note :** Do not update dependent signal in effect => infinite loop

### Angular

When a signal is used in a template, Angular subscribe to the signal change and update the template (trigger Change detection) when received notification of signal value changed.

### Usefull Posts

- [Demystifying the push & pull nature of Angular Signals](https://angularexperts.io/blog/angular-signals-push-pull)
- [Signals vs. Observables, what's all the fuss about?](https://www.builder.io/blog/signals-vs-observables)
