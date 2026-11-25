## Lab Description :

<img width="912" height="484" alt="image" src="https://github.com/user-attachments/assets/4598f558-d910-49b1-adba-a4ee04b250b5" />

## Solution :

### Identifying source -

<img width="1919" height="696" alt="image" src="https://github.com/user-attachments/assets/666d0a46-e2de-415e-9f10-834b2f46b7f5" />

As DOM invader has identified, we can pollute the prototype via the sink in URI search property. If we enter the following in the URL, we could see that foo property has been added

<img width="867" height="362" alt="image" src="https://github.com/user-attachments/assets/1105a1c9-5299-4733-884b-96848f6fea2b" />


### Identifying gadget - 

Scanning for gadgets, DOM invader shows that the `sequence` property is polluted.

<img width="1822" height="652" alt="image" src="https://github.com/user-attachments/assets/6d577503-c613-4676-9342-4e2173197579" />

### Identifying sink -

We can seethe following stack trace in console which traces back to `searchLoggerAlternative.js`

<img width="1903" height="956" alt="image" src="https://github.com/user-attachments/assets/628e94a8-060b-4ddf-8799-255b0db744d2" />

<img width="1059" height="616" alt="image" src="https://github.com/user-attachments/assets/ebfbb612-2716-4742-b5fa-69686c9f810b" />

Notice that when we click exploit in DOM invader, it does not trigger a successful XSS. 

<img width="1475" height="398" alt="image" src="https://github.com/user-attachments/assets/04969486-b46e-4434-a2bc-1f9a33989894" />

Why alert(1) alone failed:

1. No own property: manager has no own sequence → reading manager.sequence falls through to Object.prototype.sequence (polluted value).
2. Pollution read in:

<img width="1718" height="720" alt="image" src="https://github.com/user-attachments/assets/f0519757-0110-46ad-9461-84669d5b5bae" />

```js
let a = manager.sequence || 1;   // a = "alert(1)-" (your polluted string)
```
3. Vulnerable increment (type confusion):

<img width="1734" height="875" alt="image" src="https://github.com/user-attachments/assets/c6b0f925-1548-4d48-b33a-5366b389406a" />

```js
manager.sequence = a + 1;   // string + number = concatenation, not math
// "alert(1)-" + 1 → "alert(1)-1"
```

4. Real sink — unescaped eval:

<img width="628" height="84" alt="image" src="https://github.com/user-attachments/assets/37a7cc2e-448b-426d-8d9e-1b6819ce4bd7" />

```js
eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');
// becomes: eval("...manager.macro(alert(1)-1)...")
```

**`manager.sequence` is concatenated straight into code passed to eval** — no quoting/sanitization. Whatever string is there gets executed as JS.

Result: alert(1) runs as a side effect; -1 just keeps the syntax valid (undefined - 1 = NaN), harmless downstream in manager.macro(NaN).

Resume the breakpoint & now we can see that XSS has been triggered successfully.

<img width="1919" height="685" alt="image" src="https://github.com/user-attachments/assets/210fe49e-4a27-4137-8b71-f30174c7f960" />


