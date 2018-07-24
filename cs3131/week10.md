# COMP3131 - Week 10: Code Generation

## Code Template
### Arithmetic Expressions
```
[[E1 i+ E2]]:
        [[E1]]
        [[E2]]
        emit("iadd")
```

### Boolean (or Logical) Expressions: E1 && E2
```
        [[E1]]
        ifeq L1
        [[E2]]
        ifeq L1
        iconst_1
        goto L2
L1:
        iconst_0
L2:
```
- Will also need to handle short circuit evaluation

### Relational Expressions: E1 i> E2
```
        [[E1]]
        [[E2]]
        if_icmpgt L1
        iconst_0
        goto L2
L1:
        iconst_1
L2:
```

### Relational Expressions: E1 f> E2
```
        [[E1]]
        [[E2]]
        fcmpg
        ifgt L1
        iconst_0
        goto L2
L1:
        iconst_1
L2:
```
- Handling floating point, we get the difference first and then do the if jump

### Assignment Expression: a = E
```
[[E]]
istore_1
```

### Assignment Expression: LHS = RHS
```
[[LHS]]
[[RHS]]
appropriate store instruction
```

### if (E) S1 else S2
```
        [[E]]
        ifeq L1
        [[S1]]
        goto L2
L1:
        [[S2]]
L2:
```

### while (E) S
```
        push the continue label L1 to conStack
        push the break label L2 to brkStack
L1:
        [[E]]
        ifeq L2
        [[S]]
        goto L1
L2:
        Pop the continue label L1 from conStack
        Pop the break label L2 from brkStack
```
- Will also work when S is empty

### break and continue
- break: goto label marking the inst following the while
- continue: goto label marking the first inst of the while

### return E
```
[[E]]
ireturn
```

### Expression Statement: E;
```
[[E]]
pop (if it has a value left on the stack)
```

### Compound Statements
```
push the label marking the beginning of scope to scopeStart
push the label marking the end of scope to scopeEnd
[[DL]]
[[SL]]
Pop the scopeStart label
Pop the scopeEnd label
```

### Global Variable Declarations
- generate .field declarations
- generate the class initialiser `<clinit>`

### Local Variale Declarations
- Allocate indices for formal paramters and local variables
  - For a function, 0 is allocated to `this`
  - For main, 0 is allocated `argv` and 1 to implicitly declared variable `vc$`

## Generating Jasmin Directives
### `.limit locals XXX`
- Generated at the end of processing a function
- XXX is the current value for index

### `.var`
- Syntax: `.var var-index is name type-desc scopeStart-label scopeEnd-label`
- Generated when a var of formal para-decl is processed

### `.line XXX`
- Source line where the instructions between this .line and next are translated from, optional, maintain a current line

### `.limit stack XXX`
- XXX is the max depth of the operand stack
