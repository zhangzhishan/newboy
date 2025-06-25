---
title: cmd commands
tag: cmd
date: 2019-03-26 09:36:55
---

## for loop

```
for /l %%x in (1, 1, 3) do (
    echo %%x
)

for %%x in ("300" "500" "1999") do (
    echo %%x
)
```

update variables in for loop, put `setlocal enabledelayedexpansion` in the start of the file, and use `!var!`to reference a variable.

`echo xxx>>file`can be used to append text to a file, but there should be `echo xxx>file` to create the file.