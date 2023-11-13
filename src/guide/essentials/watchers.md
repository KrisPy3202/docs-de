# Watchers {#watchers}

## Basis-Beispiele {#basic-example}

Mit berechneten Eigenschaften können wir abgeleitete Werte deklarativ berechnen. Es gibt jedoch Fälle, in denen als Reaktion auf Zustandsänderungen "Seiteneffekte" ausgeführt werden müssen, z. B. Mutation des DOM oder Änderung eines anderen Teils des Zustands auf der Grundlage des Ergebnisses einer asynchronen Operation.

<div class="options-api">

Mit der Options-API können wir eine Funktion auslösen, [`watch` option](/api/options-state.html#watch) wenn sich eine reaktive Eigenschaft ändert:

```js
export default {
  data() {
    return {
      question: '',
      answer: 'Questions usually contain a question mark. ;-)'
    }
  },
  watch: {
    // wenn sich die Frage ändert, wird diese Funktion ausgeführt
    question(newQuestion, oldQuestion) {
      if (newQuestion.includes('?')) {
        this.getAnswer()
      }
    }
  },
  methods: {
    async getAnswer() {
      this.answer = 'Thinking...'
      try {
        const res = await fetch('https://yesno.wtf/api')
        this.answer = (await res.json()).answer
      } catch (error) {
        this.answer = 'Error! Could not reach the API. ' + error
      }
    }
  }
}
```

```vue-html
<p>
  Stelle eine Ja/Nein-Frage:
  <input v-model="question" />
</p>
<p>{{ answer }}</p>
```

[Try it in the Playground](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdD5cbmV4cG9ydCBkZWZhdWx0IHtcbiAgZGF0YSgpIHtcbiAgICByZXR1cm4ge1xuICAgICAgcXVlc3Rpb246ICcnLFxuICAgICAgYW5zd2VyOiAnUXVlc3Rpb25zIHVzdWFsbHkgY29udGFpbiBhIHF1ZXN0aW9uIG1hcmsuIDstKSdcbiAgICB9XG4gIH0sXG4gIHdhdGNoOiB7XG4gICAgLy8gd2hlbmV2ZXIgcXVlc3Rpb24gY2hhbmdlcywgdGhpcyBmdW5jdGlvbiB3aWxsIHJ1blxuICAgIHF1ZXN0aW9uKG5ld1F1ZXN0aW9uLCBvbGRRdWVzdGlvbikge1xuICAgICAgaWYgKG5ld1F1ZXN0aW9uLmluZGV4T2YoJz8nKSA+IC0xKSB7XG4gICAgICAgIHRoaXMuZ2V0QW5zd2VyKClcbiAgICAgIH1cbiAgICB9XG4gIH0sXG4gIG1ldGhvZHM6IHtcbiAgICBhc3luYyBnZXRBbnN3ZXIoKSB7XG4gICAgICB0aGlzLmFuc3dlciA9ICdUaGlua2luZy4uLidcbiAgICAgIHRyeSB7XG4gICAgICAgIGNvbnN0IHJlcyA9IGF3YWl0IGZldGNoKCdodHRwczovL3llc25vLnd0Zi9hcGknKVxuICAgICAgICB0aGlzLmFuc3dlciA9IChhd2FpdCByZXMuanNvbigpKS5hbnN3ZXJcbiAgICAgIH0gY2F0Y2ggKGVycm9yKSB7XG4gICAgICAgIHRoaXMuYW5zd2VyID0gJ0Vycm9yISBDb3VsZCBub3QgcmVhY2ggdGhlIEFQSS4gJyArIGVycm9yXG4gICAgICB9XG4gICAgfVxuICB9XG59XG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8cD5cbiAgICBBc2sgYSB5ZXMvbm8gcXVlc3Rpb246XG4gICAgPGlucHV0IHYtbW9kZWw9XCJxdWVzdGlvblwiIC8+XG4gIDwvcD5cbiAgPHA+e3sgYW5zd2VyIH19PC9wPlxuPC90ZW1wbGF0ZT4iLCJpbXBvcnQtbWFwLmpzb24iOiJ7XG4gIFwiaW1wb3J0c1wiOiB7XG4gICAgXCJ2dWVcIjogXCJodHRwczovL3NmYy52dWVqcy5vcmcvdnVlLnJ1bnRpbWUuZXNtLWJyb3dzZXIuanNcIlxuICB9XG59In0=)

Die `watch` Option unterstützt auch einen durch Punkte getrennten Pfad als Schlüssel:

```js
export default {
  watch: {
    // Hinweis: nur einfache Pfade. Ausdrücke werden nicht unterstützt.
    'some.nested.key'(newValue) {
      // ...
    }
  }
}
```

</div>

<div class="composition-api">

Mit der Composition API können [`watch` function](/api/reactivity-core.html#watch) wir einen Callback auslösen, sobald sich ein reaktiver Zustand ändert:

```vue
<script setup>
import { ref, watch } from 'vue'

const question = ref('')
const answer = ref('Questions usually contain a question mark. ;-)')

// Watch arbeitet direkt auf einem Ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    }
  }
})
</script>

<template>
  <p>
    Stelle eine Ja/Nein-Frage:
    <input v-model="question" />
  </p>
  <p>{{ answer }}</p>
</template>
```

[Try it in the Playground](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdCBzZXR1cD5cbmltcG9ydCB7IHJlZiwgd2F0Y2ggfSBmcm9tICd2dWUnXG5cbmNvbnN0IHF1ZXN0aW9uID0gcmVmKCcnKVxuY29uc3QgYW5zd2VyID0gcmVmKCdRdWVzdGlvbnMgdXN1YWxseSBjb250YWluIGEgcXVlc3Rpb24gbWFyay4gOy0pJylcblxud2F0Y2gocXVlc3Rpb24sIGFzeW5jIChuZXdRdWVzdGlvbikgPT4ge1xuICBpZiAobmV3UXVlc3Rpb24uaW5kZXhPZignPycpID4gLTEpIHtcbiAgICBhbnN3ZXIudmFsdWUgPSAnVGhpbmtpbmcuLi4nXG4gICAgdHJ5IHtcbiAgICAgIGNvbnN0IHJlcyA9IGF3YWl0IGZldGNoKCdodHRwczovL3llc25vLnd0Zi9hcGknKVxuICAgICAgYW5zd2VyLnZhbHVlID0gKGF3YWl0IHJlcy5qc29uKCkpLmFuc3dlclxuICAgIH0gY2F0Y2ggKGVycm9yKSB7XG4gICAgICBhbnN3ZXIudmFsdWUgPSAnRXJyb3IhIENvdWxkIG5vdCByZWFjaCB0aGUgQVBJLiAnICsgZXJyb3JcbiAgICB9XG4gIH1cbn0pXG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8cD5cbiAgICBBc2sgYSB5ZXMvbm8gcXVlc3Rpb246XG4gICAgPGlucHV0IHYtbW9kZWw9XCJxdWVzdGlvblwiIC8+XG4gIDwvcD5cbiAgPHA+e3sgYW5zd2VyIH19PC9wPlxuPC90ZW1wbGF0ZT4iLCJpbXBvcnQtbWFwLmpzb24iOiJ7XG4gIFwiaW1wb3J0c1wiOiB7XG4gICAgXCJ2dWVcIjogXCJodHRwczovL3NmYy52dWVqcy5vcmcvdnVlLnJ1bnRpbWUuZXNtLWJyb3dzZXIuanNcIlxuICB9XG59In0=)

### Watch Quellen Typen {#watch-source-types}

Das erste `watch` Argument kann aus verschiedenen Arten von reaktiven "Quellen" bestehen: Es kann ein ref (einschließlich berechneter refs), ein reaktives Objekt, eine Getter-Funktion oder ein Array mit mehreren Quellen sein:

```js
const x = ref(0)
const y = ref(0)

// Einfacher ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// array von mehreren Quellen
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
```

Beachten Sie, dass Sie eine Eigenschaft eines reaktiven Objekts nicht auf diese Weise überwachen können:

```js
const obj = reactive({ count: 0 })

// das wird nicht funktionieren da wir eine Nummer an watch() übermitteln
watch(obj.count, (count) => {
  console.log(`count is: ${count}`)
})
```

Verwenden Sie stattdessen einen Getter:

```js
// stattdessen, verwende einen getter:
watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`)
  }
)
```

</div>

## Deep Watchers {#deep-watchers}

<div class="options-api">

`watch` ist standardmäßig oberflächlich: Der Callback wird nur ausgelöst, wenn der überwachten Eigenschaft ein neuer Wert zugewiesen wurde - er wird nicht bei verschachtelten Eigenschaftsänderungen ausgelöst. Wenn Sie möchten, dass der Callback bei allen verschachtelten Änderungen ausgelöst wird, müssen Sie einen Deep Watcher verwenden:

```js
export default {
  watch: {
    someObject: {
      handler(newValue, oldValue) {
        // Hinweis: `newValue` wird hier gleich `oldValue` sein
        // bei verschachtelten Mutationen, solange das Objekt selbst
        // nicht ersetzt worden ist.
      },
      deep: true
    }
  }
}
```

</div>

<div class="composition-api">

Wenn Sie `watch()` direkt auf einem reaktiven Objekt aufrufen, wird implizit ein Deep Watcher erstellt - der Callback wird bei allen verschachtelten Mutationen ausgelöst:

```js
const obj = reactive({ count: 0 })

watch(obj, (newValue, oldValue) => {
  // feuert bei verschachtelten Eigenschaftsmutationen
  // Hinweis: `newValue` wird hier gleich `oldValue` sein
  // weil sie beide auf dasselbe Objekt verweisen!
})

obj.count++
```

Dies ist zu unterscheiden von einem Getter, der ein reaktives Objekt zurückgibt - im letzteren Fall wird der Callback nur ausgelöst, wenn der Getter ein anderes Objekt zurückgibt:

```js
watch(
  () => state.someObject,
  () => {
    // wird nur ausgelöst, wenn state.someObject ersetzt wird
  }
)
```

Sie können jedoch den zweiten Fall in einen Deep Watcher zwingen, indem Sie explizit die Option `deep` verwenden:

```js
watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // Hinweis: `newValue` wird hier gleich `oldValue` sein
    // *außer* state.someObject wurde ersetzt
  },
  { deep: true }
)
```

</div>

::: Warnung Mit Vorsicht zu verwenden
Deep Watch erfordert das Durchlaufen aller verschachtelten Eigenschaften des überwachten Objekts und kann bei großen Datenstrukturen teuer sein. Verwenden Sie es nur, wenn es notwendig ist, und achten Sie auf die Auswirkungen auf die Leistung.
:::

<div class="options-api">

## Eager Watchers \* {#eager-watchers}

`watch` ist standardmäßig faul: der Callback wird erst aufgerufen, wenn sich die überwachte Quelle geändert hat. Aber in einigen Fällen möchten wir, dass die gleiche Callback-Logik eifrig ausgeführt wird - zum Beispiel möchten wir einige anfängliche Daten abrufen und dann die Daten erneut abrufen, wenn sich der relevante Zustand ändert.

Wir können erzwingen, dass der Rückruf eines Watchers sofort ausgeführt wird, indem wir ihn als Objekt mit einer `handler`-Funktion und der Option `immediate: true` deklarieren:

```js
export default {
  // ...
  watch: {
    question: {
      handler(newQuestion) {
        // wird dies sofort bei der Erstellung der Komponente ausgeführt.
      },
      // eifrige Callback-Ausführung erzwingen
      immediate: true
    }
  }
  // ...
}
```

Die anfängliche Ausführung der Handler-Funktion wird kurz vor dem `created`-Hook erfolgen. Vue hat bereits die Optionen `data`, `computed` und `methods` verarbeitet, so dass diese Eigenschaften beim ersten Aufruf verfügbar sind.
</div>

<div class="composition-api">

## `watchEffect()` \*\* {#watcheffect}

`watch()` ist faul: der Callback wird erst aufgerufen, wenn sich die überwachte Quelle geändert hat. Aber in einigen Fällen möchten wir vielleicht, dass die gleiche Callback-Logik eifrig ausgeführt wird - zum Beispiel möchten wir einige anfängliche Daten abrufen und dann die Daten erneut abrufen, wenn sich der relevante Zustand ändert. Möglicherweise werden wir dies tun:

```js
const url = ref('https://...')
const data = ref(null)

async function fetchData() {
  const response = await fetch(url.value)
  data.value = await response.json()
}

// sofort abrufen
fetchData()
// ...dann auf Url-Änderung achten
watch(url, fetchData)
```

Dies kann vereinfacht werden durch [`watchEffect()`](/api/reactivity-core.html#watcheffect). `watchEffect()` ermöglicht es uns, einen Seiteneffekt sofort auszuführen und dabei automatisch die reaktiven Abhängigkeiten des Effekts zu verfolgen. Das obige Beispiel kann umgeschrieben werden als:
```js
watchEffect(async () => {
  const response = await fetch(url.value)
  data.value = await response.json()
})
```

Hier wird der Callback sofort ausgeführt. Während seiner Ausführung wird er auch automatisch `url.value` als eine Abhängigkeit verfolgen (ähnlich wie bei berechneten Eigenschaften). Wann immer sich `url.value` ändert, wird der Callback erneut ausgeführt.

Sie können mit [this example](/examples/#fetching-data)`watchEffect` und reaktivem Datenabruf in Aktion ausprobieren.

:::Tipp
`watchEffect` verfolgt Abhängigkeiten nur während ihrer **synchronen** Ausführung. Bei Verwendung mit einem asynchronen Callback werden nur die Eigenschaften verfolgt, auf die vor dem ersten `await`-Tick zugegriffen wird.
:::

### `watch` vs. `watchEffect` {#watch-vs-watcheffect}

`watch` und `watchEffect` erlauben es uns, reaktiv Seiteneffekte auszuführen. Ihr Hauptunterschied ist die Art und Weise, wie sie ihre reaktiven Abhängigkeiten verfolgen:

- `watch` verfolgt nur die explizit überwachte Quelle. Alles, worauf innerhalb des Rückrufs zugegriffen wird, wird nicht verfolgt. Außerdem wird der Rückruf nur ausgelöst, wenn sich die Quelle tatsächlich geändert hat. `watch` trennt die Abhängigkeitsverfolgung vom Seiteneffekt und gibt uns eine genauere Kontrolle darüber, wann der Callback ausgelöst werden soll.

- `watchEffect`, hingegen kombiniert die Verfolgung von Abhängigkeiten und Seiteneffekten in einer Phase. Sie verfolgt automatisch jede reaktive Eigenschaft, auf die während ihrer synchronen Ausführung zugegriffen wird. Dies ist bequemer und führt in der Regel zu kürzerem Code, macht aber die reaktiven Abhängigkeiten weniger explizit.

</div>

## Callback Flush Timing {#callback-flush-timing}

Wenn Sie den reaktiven Zustand verändern, kann dies sowohl Aktualisierungen der Vue-Komponenten als auch von Ihnen erstellte Watcher-Rückrufe auslösen.

Standardmäßig werden vom Benutzer erstellte Watcher-Callbacks **vor** den Aktualisierungen der Vue-Komponenten aufgerufen. Das bedeutet, wenn Sie versuchen, auf das DOM innerhalb eines Watcher-Callbacks zuzugreifen, wird das DOM in dem Zustand sein, bevor Vue irgendwelche Updates angewendet hat.

Wenn Sie in einem Watcher-Callback auf das DOM zugreifen wollen, ** nachdem** Vue es aktualisiert hat, müssen Sie die Option `flush: 'post'` angeben:

<div class="options-api">

```js
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'post'
    }
  }
}
```

</div>

<div class="composition-api">

```js
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
```

Post-flush `watchEffect()` hat auch einen praktischen Alias, `watchPostEffect()`:

```js
import { watchPostEffect } from 'vue'

watchPostEffect(() => {
  /* ausgeführt nach Vue-Updates */
})
```

</div>

<div class="options-api">

## `this.$watch()` \* {#this-watch}

Es ist auch möglich, zwingend Watcher zu erstellen, indem man die [`$watch()` instance method](/api/component-instance.html#watch) verwendet:

```js
export default {
  created() {
    this.$watch('question', (newQuestion) => {
      // ...
    })
  }
}
```

Dies ist nützlich, wenn Sie einen Watcher bedingt einrichten oder etwas nur als Reaktion auf eine Benutzerinteraktion beobachten wollen. Außerdem können Sie damit den Watcher frühzeitig beenden.

</div>

## Stopping a Watcher {#stopping-a-watcher}

<div class="options-api">

Watcher, die mit der Option `watch` oder der Instanzmethode `$watch()` deklariert wurden, werden automatisch gestoppt, wenn die Besitzerkomponente ausgehängt wird, so dass Sie sich in den meisten Fällen nicht selbst um das Stoppen des Watchers kümmern müssen.

In dem seltenen Fall, dass Sie einen Watcher stoppen müssen, bevor die Owner-Komponente aussteigt, gibt die `$watch()` API eine Funktion dafür zurück:

```js
const unwatch = this.$watch('foo', callback)

// ...wenn der Beobachter nicht mehr benötigt wird:
unwatch()
```

</div>

<div class="composition-api">

Watcher, die synchron innerhalb von `setup()` oder `<script setup>` deklariert werden, sind an die Instanz der Eigentümerkomponente gebunden und werden automatisch gestoppt, wenn die Eigentümerkomponente ausgehängt wird. In den meisten Fällen müssen Sie sich nicht darum kümmern, den Watcher selbst zu stoppen.

Der Schlüssel hierzu ist, dass der Watcher **synchron** erstellt werden muss: Wenn der Watcher in einem asynchronen Callback erstellt wird, wird er nicht an die Eigentümerkomponente gebunden und muss manuell gestoppt werden, um Speicherlecks zu vermeiden. Hier ist ein Beispiel:

```vue
<script setup>
import { watchEffect } from 'vue'

// dieser wird automatisch gestoppt
watchEffect(() => {})

// ...dieser wird nicht gestoppt!
setTimeout(() => {
  watchEffect(() => {})
}, 100)
</script>
```

Um einen Watcher manuell zu stoppen, verwenden Sie die zurückgegebene Handle-Funktion. Dies funktioniert sowohl für `watch` als auch für `watchEffect`:

```js
const unwatch = watchEffect(() => {})

// ...später, wenn es nicht mehr gebraucht wird
unwatch()
```

Beachten Sie, dass es nur sehr wenige Fälle geben sollte, in denen Sie Watcher asynchron erstellen müssen, und dass die synchrone Erstellung wann immer möglich bevorzugt werden sollte. Wenn Sie auf einige asynchrone Daten warten müssen, können Sie Ihre Watch-Logik stattdessen an Bedingungen knüpfen:

```js
//asynchron zu ladende Daten
const data = ref(null)

watchEffect(() => {
  if (data.value) {
    // etwas tun, wenn Daten geladen sind
  }
})
```

</div>
